# Lessons from Dyson

I was a member of the team that delivered Dyson's 360 Eye robot, their first commercial robotic vacuum cleaner. Here are some lessons for life from my stint there.

## On Product Engineering

These are extracted from a [talk](https://tv.theiet.org/?videoid=7390) that [Mike Aldred](https://www.linkedin.com/in/mike-d-aldred/) gave at the IET in 2015, which covers Dyson’s product development philosophy quite well. The online video is . 

- For a robot to succeed as a new product it must
  - Solve a real problem
  - Be better (than alternatives)
  - Be different (from alternatives. Don’t do a ‘me too’ product)
- Focus on the benefits the product brings to the consumer, not on the technology
  - Vision and robotics are enabling technologies. They are not product themselves.
  - Customers may initially get excited about the underlying technology. But they wears away soon
- Set difficult challenges to solve with a new product. 
  - Develop an ‘aspirational’ spec.
  - Easy = not worth doing (somebody else will beat us to it). 
- “Start as a scientist, end as an engineer”
  - Two definite stages to development: Idea generation, followed by product development. Develop a scientific framework, then build the product on top of it
  - Idea generation: Widen your horizons, keep many options open.
    - Challenge received wisdom. “Try something which could obviously never work”. 
    - Don't follow a particular approach just because everybody else is doing it.
    - Be obtuse, and intentionally so. **Go against the grain; do the opposite of conventional wisdom and the obvious.**
  - Product development: Narrow down to a specific product, at a specific date, to a specific spec.
    - Improve resolution over iterations. Make it manufacturable, reliable, safe.
    - Eliminate superfluous design that doesn’t ultimately benefit the customer, even if the technology is interesting to us as engineers.
    - **Product development phase is about critical decisions made early.**
  - Do not isolate idea generation team from production team. Don’t prototype an idea as proof of concept and throw it over the wall for production team to deal with. There has to be a continuation of knowledge sharing and engagement, cooperation, and collaboration.
  - Bias to action. if something can be tested easily without going into a massive theoretical exercise, do that first. Understand the potential, then follow up with detailed theoretical analysis. Don’t jump straight into analysis mode.
- Dyson 360 Eye aspiration spec
  - **Vacuum cleaner first, robot second**. Not the other way around.
  - Must work better than a human
  - Autonomous enough to set the user free to do other things. **Partial autonomy isn’t useful** (no babysitting)
- Dyson 360 Eye specific challenges
  - Robot odometry lies. That’s why vision.
  - Camera 10cm off the floor in a cluttered environment
  - Standard camera wouldn’t work; restricted field of view would risk robot being misguided by the obstacle directly in front of it. This product is required to get close to objects -> occlusions happen all the time. Hence, panoramic camera.
  - **Use the simplest lowest common denominator technologies** that get the job done. Why use a HD colour camera when VGA monochrome does the job well enough? We would be throwing away all the extra information from high resolution sensors anyway. Information optimisation is critical to ensure we don’t exceed CPU budget.
  - Features look different when camera gets close. Feature tracking lifetime is a challenge. On an omnidirectional camera, exposure control that’s optimal on the entire image is a challenge. Blurred features produce features that can’t be tracked well.
- On simulation tools
  - Initially under-estimated simulator as a development tool. Need a really good tools team and test team. Algorithm-in-the-loop tests are valuable to prove core technologies and ensure individual blocks work as expected.
  - Simulator is only as good as the effort put into its development.
  - For autonomous robotic products, **the product is only as good as the simulator** it has been tested in. Because it’s prohibitive to do statistically significant number of test cases in the real world all the time. Simulator catches regressions during development.
  - Simulator only gives you the confidence to take the product into the real world and test it there. Simulator has to be continuously improved by folding back lessons learnt from the real world or it loses relevance quickly.
- On testing
  - Simulations get you to a point where we can start testing
  - No matter what you know about the real world, it’s the stuff that you don’t know that will hurt the product.
  - **20% idea generation, 10% development, 70% testing**. Idea generation phase is where we ‘think’ we have solved a problem. Testing phase is where we ‘know’ we have solved the problem.
  - Start testing early, as soon as proof of concept builds are made.
  - Analysis tools for test data sets are important. If the number of tests are statistically significant, then the data sets are going to be huge. Automated test data analysis tools are required to make the analysis problem tractable.
  - 75000+ home trials were completed on the 360 Eye before it was released as a product
  - Real world tests exercise failure modes engineers never thought of. **Users will use the system in ways that are unexpected**. Because they don’t have the level of knowledge the design engineer has. Don’t expect them to read manuals, but communicate effectively how the system works and make it user friendly.
- Knowledge management
  - Gain knowledge (eg: vision systems), document it and improve on it for future products
  - Gain knowledge that will be relevant 3 - 5 years from now (What will people need in products and services 5 years down the line). Dyson 360 Eye started development on the challenging vision system during a time when the key feature marketed on cell phones of the time were polyphonic ring tones and illuminated buttons. **Challenge yourself to develop technology that will be relevant in the future, not today**.
  - Accept that you may not be the expert at everything. Identify the expert (go to academia) and work with them from early on.
  - Knowledge has to be shared within the team. No point in having isolated experts who have that expertise only in their heads. When they leave, the team loses that expertise. Have systems that capture expertise for posterity. (eg: documents colocated with sources)
  - Disseminate all knowledge across the team - more importantly the failures - that’s where learning lies.

> An inventor’s path is chorused with groans, riddled with fist-banging and punctuated by head scratches. Stumbling upon the next great invention in an ‘ah-ha!’ moment is a myth. It is only by learning from mistakes that progress is made - James Dyson.

## On Persistence

As a long-distance runner at school, James Dyson learned that almost everyone gets tired and feels like giving up at the same time....and that's exactly when you have to go the extra mile. (Amelia Boone in ‘Tools of Titans’ says the same thing. When you are tired is when everyone else is tired too. That’s the time to push yourself, because everyone else around you is at the point of giving up)

## On Marketing

Dyson’s marketing material is engineering driven. Their ads communicate how and why Dyson products are engineered the way they are. They explain what engineering choices were made to make their products better than the competition. Here’s an example demonstrating that approach: https://www.youtube.com/watch?v=OadhuICDAjk.

Dyson does not micromessage. What the employee hears is what the customer hears.