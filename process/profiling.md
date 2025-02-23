# Profiling tools

- CPU profiling: [hotspot](https://github.com/KDAB/hotspot)
  - How to use it: <https://youtu.be/grOHeyQlL6c>
- Memory profiling: [heaptrack](https://github.com/KDE/heaptrack)
- Build profiling (to determine where most time is spent during builds):
  - Configure build system to use Ninja with `-GNinja` option at CMake configuration step
  - Notice that a `.ninja_log` file is generated in the build folder
  - Convert the `.ninja_log` into a trace file using [`ninjatracing`](https://github.com/nico/ninjatracing)
  - View the trace file in e.g. https://ui.perfetto.dev/