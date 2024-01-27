![image](https://github.com/manaskumarm/GarbageCollector/assets/14363425/32505745-366e-445c-8f5b-d40befe8272d)

Garbage collection in .NET is a memory management technique that automatically deallocates memory occupied by objects that are no longer in use, preventing memory leaks and improving system efficiency.

1. **Memory Allocation**:
When an object is created in a .NET application, memory is allocated from the managed heap to store that object.

2. **Generation-Based Approach**:
The managed heap is divided into three generations: Generation 0, Generation 1, and Generation 2.
New objects are initially allocated in Generation 0. Objects that survive garbage collections are promoted to higher generations.

3. **Garbage Collection Triggers**:
GC is triggered automatically or explicitly when certain conditions are met, such as low memory, a specific time interval, or explicit calls to GC.Collect().

4. **Mark and Sweep**:
The garbage collector identifies and marks objects that are still reachable (live objects) by starting from root objects (global variables, references in CPU registers, etc.).
Unreachable objects are considered candidates for collection.

5. **Compact and Promote**:
Garbage collector compacts the memory by moving live objects to fill gaps created by collected objects, reducing fragmentation.
Surviving objects in Generation 0 are promoted to Generation 1, and so on.

6. **Finalization**:
Objects with finalizers (special methods for cleanup) go through an additional phase before being reclaimed.

7. **Reclaimation**:
Collected memory is reclaimed, and the managed heap is ready for new allocations.
