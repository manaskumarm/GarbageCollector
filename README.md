## Garbage Collection

![image](https://github.com/manaskumarm/GarbageCollector/assets/14363425/32505745-366e-445c-8f5b-d40befe8272d)

Garbage collection(GC) in .NET is a memory management technique that automatically deallocates memory occupied by objects that are no longer in use, preventing memory leaks and improving system efficiency. GC is triggered automatically or explicitly when certain conditions are met, such as low memory, a specific time interval, or explicit calls to GC.Collect().

1. **Memory Allocation**:
When an object is created in a .NET application, memory is allocated from the managed heap to store that object.

2. **Generation-Based Approach**:
The managed heap is divided into three generations: Generation 0, Generation 1, and Generation 2. Objects that survive garbage collections are promoted to higher generations.

    Generation 0: This is the youngest generation and contains short-lived objects. An example of a short-lived object is a temporary variable. Garbage collection occurs most frequently in this generation.
    Most objects are reclaimed for garbage collection in Gen 0 and do not survive to the next generation. Objects that survive a Gen 0 garbage collection are promoted to Gen 1.
    
    Generation 1: This generation contains short-lived objects and acts as a buffer between short-lived objects and long-lived objects. Objects that survive a Gen 1 garbage collection are promoted to Gen 2.
    
    Generation 2: This generation contains long-lived objects. An example of a long-lived object is an object in a server application that contains static data that lives for the duration of the process. Objects that survive a Gen 2 garbage collection remain in Gen 2. A Gen 2 garbage collection is also known as a full garbage collection because it reclaims all objects in all generations.

![image](https://github.com/manaskumarm/GarbageCollector/assets/14363425/35b0e342-e027-4529-ab40-f5290f9fce2b)

3. **Mark and Sweep**:
The garbage collector identifies and marks objects that are still reachable (live objects) by starting from root objects (global variables, references in CPU registers, etc.).
Unreachable objects are considered candidates for collection.

4. **Compact and Promote**:
Garbage collector compacts the memory by moving live objects to fill gaps created by collected objects, reducing fragmentation.
Surviving objects in Generation 0 are promoted to Generation 1, and so on.

5. **Finalization**:
Objects that have a finalizer (a method that runs before an object is garbage collected) are put in a special queue for finalization. Finalization is performed after the marking and sweeping stages. Then finalization queue is processed, and the finalizer methods of objects are executed.

6. **Reclaimation**:
After all the previous stages, memory is reclaimed and ready for new object allocations.

The CLR provides support for automatic memory management. Managed memory (memory allocated using the C# operator new) does not need to be explicitly released. It is released automatically by the garbage collector (GC). It primarily deals with managed resources (memory occupied by managed objects) and works automatically in the background. In order to release of unmanaged resources **Disposable pattern** plays vital role.

****Disposable pattern****

The IDisposable pattern in C# is a design pattern used for resource management, specifically to release unmanaged resources such as file handles, database connections, or network connections. It is crucial for cleaning up resources explicitly, rather than relying solely on the garbage collector. The pattern revolves around the IDisposable interface and the Dispose method.
```
using System;

public class MyDisposableClass : IDisposable
{
    // Flag to track whether Dispose has been called
    private bool disposed = false;

    // Resource cleanup logic in the Dispose method
    protected virtual void Dispose(bool disposing)
    {
        if (!disposed)
        {
            if (disposing)
            {
                // Release managed resources here
            }

            // Release unmanaged resources here

            disposed = true;
        }
    }

    // Public method to allow users to release resources explicitly
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    // Finalizer to handle cleanup if Dispose is not called
    ~MyDisposableClass()
    {
        Dispose(false);
    }
}
```
  The class implements the IDisposable interface.
  
  The Dispose method is responsible for releasing both managed and unmanaged resources.
  
  The Dispose(bool disposing) method is used for conditional resource cleanup.
  
  The finalizer (~MyDisposableClass) ensures that resources are released if Dispose is not called explicitly.
  
  The GC.SuppressFinalize(this) call informs the garbage collector that the finalizer doesn't need to be executed, as resource cleanup has already been performed.

  By combining the automatic memory management of the Garbage Collector with the explicit resource cleanup of the Disposable pattern, developers can ensure efficient and comprehensive management of both managed and unmanaged resources in .NET applications
