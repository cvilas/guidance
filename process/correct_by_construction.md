# Correct by construction: APIs that are easy to use and hard to misuse

Notes from [Matt Godbolt's talk](https://youtu.be/nLSm3Haxz0I) at [C++ On Sea](https://cpponsea.uk/2020/schedule/) 2020.

## The goal

- Write software such that "If the code compiles, it's correct". Not always possible to achieve this, but the idea is to catch errors and problems early in the development workflow
- C++ toolbox to help with this:
  - `const`, `consteval` and `constexpr`
  - Scoped enumerations (enum class)
  - RAII
  - Static code analysis
  - Strong types

## Example 1: An API call

Let's say we have this, a function call to trade stocks:

```cpp
void sendOrder(
  const char* symbol, 
  bool buy, 
  int quantity, 
  double price);
```

Easy to make an expensive mistake `sendOrder("GOOG", false, 1000.00, 100)` : Sell 1000 shares for 100$ each instead of 100 shares for 1000$ each.  To begin with, by default you should:

> Advice #1: Turn on all the warnings all the times and make them compiler errors (enable `-Wall -Werror -Wextra`)

However, `gcc -c -Wall -Werror -Wextra` won't catch the error above. A slight improvement would be to have user-defined types to clarify the meaning of what we are passing to the function:

```cpp
class Price{/**/};

class Quantity 
{
private:
  unsigned int quantity_;
public:
  explicit Quantity(int quantity) : quantity_(quantity) {}
  unsigned int value() const { return quantity_; }
};

void sendOrder(const char* symbol, bool buy, Quantity quantity, Price price);
```

> Advice #2: Use named types to clarify semantics

Note that we are passing `price` and `quantity` by value. With reference/pointer semantics, you need to worry about who else has concurrent access to the variables and what they are doing with them while `sendOrder` is processing them.

> Advice #3: Use value semantics. They are easier to reason about in code compared to reference semantics, and easier to check for correctness.

Notice also that the constructor is marked `explicit`, and avoids implicit conversion during copy initialisation; i.e. you cannot do `Quantity q = 1;`. See [https://en.cppreference.com/w/cpp/language/explicit](https://en.cppreference.com/w/cpp/language/explicit)

> Advice #4: Use `explicit` when declaring single-argument constructors

Although `Quantity::quantity_` is declared `unsigned int` but `Quantity thisDoesntSeemRight(-100);` will still compile fine. One way to fix this is to:

> Advice #5: Enable `-Wsign-conversion` compiler flag.

If that is not possible due to too many false positives (especially in low level code), then use C++ traits to enforce requirements on the parameters. Eg:

```cpp
template<typename T>
explicit Quantity(T quantity) : quantity_(quantity) {
  static_assert(std::is_unsigned_v<T>, "Use only unsigned types");
}
```

or in C++20, use Concepts like so:

```cpp
template<typename T>
requires std::is_unsigned_v<T>
explicit Quantity(T quantity) : quantity_(quantity) {}
```

> Advice #6: Apply requirements and constraints on types so that they are automatically checked, preferably at compile time.

Note that `Quantity q(100)` will now fail because 100 represents a signed integer type. You will have to do `Quantity q(100u)`. However, some unavoidable edge cases may still make it through in certain use cases, such as this: `Quantity q(static_cast<unsigned int>(atoi("123")));`. Help your users catch such errors with helper methods. The following demonstrates a `static .. from()` method that implements run-time checks as follows:

```cpp
class Quantity 
{
public:
  template<typename T>
  static Quantity from(T quantity) {
    if(quantity < 0 || quantity > Quantity::max()) {
      throw std::runtime_error("Invalid quantity");
    }
    return Quantity(static_cast<std::make_unsigned<T>>(quantity));
  }
};
```

GSL (Guidelines Support Library) provides a nifty alternative to check that a value passed in will fit into the variable that needs to hold it.

```cpp
class Quantity 
{
public:
  template<typename T>
  static Quantity from(T quantity) {
    if(quantity < 0 || quantity > Quantity::max()) {
      throw std::runtime_error("Invalid quanitty");
    }
    return Quantity(gsl::narrow<decltype(quantity_)>(quantity));
  }
};
```

> Advice #7: Use GSL to help you write better code that follows the C++ Core Guidelines

([https://github.com/microsoft/GSL](https://github.com/microsoft/GSL))

Avoid verbosity by using user-defined literals, especially where a lots of constants are required, such as in unit tests. 

```cpp
consteval Quantity operator""_qty(unsigned long long value) {
  return Quantity::from(value);
}

consteval Price operator""_dollars(unsigned long long value) {
  /* Note: Yes, using integers instead of double to represent 
     dollars here. Don't use doubles for currency!*/
  return Price::from(value);
}

// usage
sendOrder("GOOG", false, 100_qty, 1000_dollars);
```

Note that we use `consteval` functions to do this and not `constexpr`, since `constexpr` functions aren't necessarily evaluated at compile time. If we used `constexpr` to define the literals, it may not catch overflow errors such as passing `9999999999_qty` as a parameter, which cannot fit in `unsigned int`. So,

> Advice #8: Use `consteval` to guarantee compile-time evaluation.

By the way, you should

> Advice #9: Write unit tests. They aren't necessarily there to catch errors as you develop new code (although it can help there too). More importantly, they are a means to catch future regressions.

Now have a look at `sendOrder` in the call above. What does the second parameter mean, again?

> Advice #10: Don't pass naked `bool`s (`true` or `false` flags) unless their meaning is absolutely clear from the context.

Use an `enum class` instead. It's a strong type and you will get a decent error message if you screw up. Now, the folllowing code looks *just right* by inspection.

```cpp
enum class BuyOrSell {Buy, Sell};

sendOrder("GOOG", BuyOrSell::Buy, 50_qty, 975_dollars);
```

## Example 2: A messaging protocol

Let's start with this one

```cpp
/* Must be 13 bytes long */
struct MessageHeader 
{
  uint64_t sequence_num;
  uint32_t message_size;
  char type; /* Must be 'A', 'D' or 'M'. Describes what the payload
               (rest of the message) is: Add, Delete, Modify.*/
};

/* Types of messages */
struct AddMessage{/*..*/};
struct DeleteMessage{/*..*/};
struct ModifyMessage{/*..*/};

/* message handlers */
void handle(const AddMessage& add);
void handle(const DeleteMessage& add);
void handle(const ModifyMessage& add);

/* A single convenience function/wrapper */
template<typename Msg>
void handle(const void* payload) 
{
  handle(*static_cast<const Msg*>(payload));
}

/* process header and then payload */
void on_message(const MessageHeader& hdr, const void* payload) {
  if(hdr.type == 'A') {
    handle<AddMessage>(payload);
  } else if(hdr.type == 'D') {
    handle<DeleteMessage>(payload);
  } else if(hdr.type == 'M') {
    handle<ModifyMessage>(payload);
  }
}
```

Enforcing requirements in comments is no good, as the compiler does not see it. Can we turn comments like `/* Must be 13 bytes long */` into something the compiler can actually check? An improved version:

```cpp
enum class MessageType : char 
{
  Add = 'A',
  Modify = 'M',
  Delete = 'D'
};

struct [[gnu::packed]] MessageHeader 
{
  uint64_t sequence_num;
  uint32_t message_size;
  MessageType type; 
};

// ** Check **. This can be in the header, telling the reader, in code, what
// the requirement is
static_assert(sizeof(MessageHeader) == 13);

struct AddMessage{/*..*/};
struct DeleteMessage{/*..*/};
struct ModifyMessage{/*..*/};

void handle(const AddMessage& add);
void handle(const DeleteMessage& add);
void handle(const ModifyMessage& add);

template<typename Msg>
void handle(const void* payload) {
  handle(*static_cast<const Msg*>(payload));
}

void on_message(const MessageHeader& hdr, const void* payload) 
{
  switch(hdr.type) {
    case MessageType::Add: return handle<AddMessage>(payload); 
    case MessageType::Delete: return handle<DeleteMessage>(payload); 
    case MessageType::Modify: return handle<ModifyMessage>(payload); 
  }
  throw std::runtime_error("Invalid message type " + std::to_string(static_cast<int>(hdr.type)));
}
```

- We replaced the `char` to indicate message type with a strongly-typed enum that enforces constraints on what values `MessageHeader::type` could take.
- We used `static_assert` on the header size to sanity check the requirement on header size. Notice that `MessageHeader` as previously defined was not actually 13 bytes but 16 because it would be aligned to the boundary of the biggest type in the struct which is uint64_t. Using a static_assert informed us via a compiler error that we need to use a packed type to enforce the size.
- We did not define the `default` case in `switch()` , allowing the compiler to raise an error if all the possible cases aren't handled. This could happen, for instance if `enum MessageType` is extended some time in the future by somebody else, which is easily done in things like state machines.

To summarise

> Advice #11: Use `static_assert` to check expectations are satisfied at compile time

> Advice #12: Use enum classes for things like labels and choices, even for sized tags.

> Advice #13: Prefer 'no-default' `switch()` to `if()`, so compiler can catch unhandled cases.

## Example 3: Comment smells

Using comments to push the responsibility of doing the right thing on to the user isn't very nice:

```cpp
class MyWidget 
{
  std::mutex mutex_;
public:
  // don't forget to unlock
  void lock();

  // only valued if you locked
  void unlock();

  // must hold lock
  void tinker_with(int amount);
};

// usage somewhere in code
void tinker(MyWidget& widget) {
  widget.lock();
  widget.tinker(123);
  widget.tinker(222);
  widget.unlock();
}
```

Similar to the concept of 'affordances' in the design of physical products, it is better if the code is structured in such a way that misuse is not possible. Following is an improved version of the code above. Using a scoped lock ensures that the lock goes out of scope automatically, so the user does not have to remember to explicitly unlock it. Note, however, that `lock()` must be marked `[[nodiscard]]` to ensure the compiler throws an error if we don't capture the return value (as in line `(1)`). The lock would immediately have gone out of scope at line `(1)`. 

```cpp
class MyWidget 
{
private:
  std::mutex mutex_;
  void tinker(int amount);
public:
  [[nodiscard]] std::scoped_lock<std::mutex> lock();

  // only valid if you locked
  void unlock();
};

// usage somewhere in code
void tinker(MyWidget& widget) {
  // widget.lock()           // (1) - Oops. Lock goes out of scope rightaway
  auto lock = widget.lock(); // (2) - OK
  widget.tinker(123);
  widget.tinker(222);
}
```

> Advice #14: Protect invariants with code. Use mechanisms such as `scoped_lock` to automatically do the right thing, without relying on the user to have read a manual to do so.

(An invariant is a statement about program variables that is true every time the execution of the program reaches the invariant.)

We can do better. In the following, we make `tinker()` private and implement a mutator interface which gives the user a `Tinkerable` on demand, while maintaining locks automatically in the right state. The lock is acquired during construction of `Tinkerable` and released when it goes out of scope. 

```cpp
class MyWidget 
{
private:
  std::mutex mutex_;

  void tinker(int amount);
  [[nodiscard]] std::scoped_lock<std::mutex> lock();
  void unlock();

public:
  class Tinkerable;

  Tinkerable getTinkerable() {
    return Tinkerable(*this);
  }
};

class MyWidget::Tinkerable 
{
private:
  MyWidget& widget_;
  std::scoped_lock<std::mutex> lock_;
  friend MyWidget;

  explicit Tinkerable(MyWidget& widget) : widget_(widget), lock_(widget_.mutex_) {}
public:
  void tinker(int amount) {
    widget_.tinker(amount);
  }
};

// usage somewhere in code
void tinker(MyWidget& widget) {
  auto tinkerable = widget.getTinkerable();
  tinkerable.tinker(123);
  tinkerable.tinker(222);
}
```

Another approach is for the caller to provide `MyWidget` with a tinker function to use (i.e., 'Don't call us, we'll call you!')

```cpp
class MyWidget 
{
private:
  std::mutex mutex_;

  class WidgetState {
  private:
    int state = 123;
  public:
    void tinker(int amount);
  };
  WidgetState state_;

  [[nodiscard]] std::scoped_lock<std::mutex> lock();
  void unlock();

public:
  template <typename TinkerFunc>
  void tinker(TinkerFunc func) {
    std::scoped_lock lock(mutex_);
    func(state_);
  }
};
```

## Example 4: 'Destructive Separation*'

(* Matt Godbolt's terminology)

There are sometimes cases where we want to prepare/initialise/stage an object and then perform an operation on it, after which the original object becomes unusable for further operations or certain types of operations. The following is an example 

```cpp
class TelemetrySystem 
{
public:

  // usage instructions:
  // - First call register() on all variables of interest.
  // - Then, call initialise() to start telemetry. Once initialised, 
  //   you cannot call register() again
  // - Once initialised, call publish() periodically to sample and 
  //   publish all registered variables simultaneously.
  // - You cannot call initialise() more than once.
  template<typename T>
  void register(const std::string& name, const T* var);

  void initialise();

  void publish();
};
```

Again, this forces upon the user the responsibility to do the right thing by following instructions in a comment. There is a better way to express such requirements in code, and uses a little known feature in the C++ standard. 

```cpp
class TelemetryRegistry 
{
public:
  template<typename T>
  void register(const std::string& name, const T* var);

  /* Resources used in the initialistion process are transferred to the 
  InitialisedTelemetry object. You cannot call initialise twice!*/
  [[nodiscard]] InitialisedTelemetry initialise() &&;
};

class InitialisedTelemetry 
{
public:
  void publish();
};

void use() 
{
  TelemetryRegistry telemetry_registry;
  telemetry_registry.register("motor voltage", &motor_voltage);
  telemetry_registry.register("encoder counts", &encoder_counts);
  ///...

  // The following won't compile
  // auto initialised_telemetry = telemetry_registry.start(); 

  // instead, you have to do this. Also provides a hint to the user 
  // that since telemetry_registry has been moved, it is not usable 
  // after the start operation  
  auto initialised_telemetry = std::move(my_registry).initialise(); 
  ... // some operations ...
  initialised_telemetry.publish();
  ... // some operations ....
  initialised_telemetry.publish();
}
```

Notice the `&&` after the method `TelemetryRegistry::initialise()`. It changes the r-valueness of the `this` pointer - it tells the compiler that the `initialise()` method is only valid if the `TelemetryRegistry` object is temporary or about to move out of scope. This example demonstrates a design pattern similar to but not same as  the single responsibility principle by separating the concerns.

> Advice #15: Apply separation of concerns to ensure the correct ordering of API sequence of operations.

Note that calling `register()` after TelemetryRegistry has been `move`-d from will *unfortunately* not raise a compiler error at the moment, due to the semantics of `move()` operation. However, a static analyzer such as clang-tidy will catch this and report it as a potential issue.

> Advice #16: Setup your build system to run static analysis during compile time.

## Example 5: RAII

Resource Aquisition Is Initialisation. But RAII is not just about resources. A better name would have been CADR: Constructor Acquires, Destructor Releases. Scoped lock usage in the previous section is an example of this. Here's an example of some code that could be made better

```cpp
class EventSource 
{
public:
  struct Listener 
  {
    virtual ~Listener() = default;
    virtual void on_event() = 0;
  };

  // Don't forget to unsubscribe! <- YIKES
  void subscribe(Listener &listener);
  void unsubscribe(Listener& listener);
};
```

Note the instruction to the user in a comment. Here's a better way. A `Subscription` object binds together the event source with the listener and keeps the invariants

```cpp
class EventSource 
{
  // ..
private:
  void add(Listener& listener);
  void remove(Listener& listener);
public:
  [[nodiscard]] Subscription subscribe(Listener& listener) {
    return Subscription(*this, listener);
  }
};

class EventSource::Subscription 
{
  Subscription(EventSource& src, Listener& listener) : source_(source), listener_(listener) {
    source_.add(listener_);
  }

  ~Subscription() {
    source_.remove(listener_);
  }

  //... special member functions. Don't allow copy but allow move
  Subscription(const Subscription&) = delete;
  Subscription(const Subscription&&) {/**/}
};
```

RAII is also useful for handling

- Recursion depth (for tracking things like indentation during printing, scoped logging severity, etc)
- Re-entrancy (eg: locking on construction of a modifier object so that another object cannot gain concurrent access)
- Timers (eg: taking timestamp on construction and then again on destruction to report processing duration)

END

Vilas Chitrakaran
