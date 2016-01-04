---
layout: post
title:  "Java Reference"
date:   2015-05-11 1:25:09
categories: java programming
tags: java annotation reference "weak reference" "soft reference" "phantom reference" "reference queue"
---

In this I will talk about various subclass of `java.lang.ref.Reference`. As written in its docs:

**Reference**: It defines the operations common to all reference objects.  Because reference objects are implemented in close cooperation with the garbage collector, this class may not be sub-classed directly (constructor are of default scope).

Mainly the reference is controlled by java itself but there are some ways to control reference at program time also. This tutorial is about that only. Though I am also a bit curios about `Reference` class itself. There are so many children of this class and so many methods. There are many questions in my mind like: *How Reference class is evolved?, How GC control reference?, What are all the children of reference class are?*. I will try to read about them, but for this particular post let focus on different reference class children which are exposed by java to giv some control of reference to programmer:

1. `java.lang.ref.SoftReference`
2. `java.lang.ref.WeakReference`
3. `java.lang.ref.PhantomReference`

> A referent is a object referred by an reference variable

If GC comes to check an object which has only Soft/Weak/Phantom references, then it **can be** cleared by GC. Except strong reference GC can clean any objects with only non-strong reference. This thing is common between all three type of references, let see the differences between them:

## Soft Reference
A referent with only soft reference will be cleared only when GC is eager to clear memory, such as in case of potential OOM. But java ensures that every referent with only soft/weak/phantom reference will be cleared before OOM happens. In reality when the soft reference will be cleared is defined by GC implementation. But SoftReference maintains two variables (which are not maintained by weak/phantom):

1. **timestamp**: Timestamp updated by each invocation of the get method. The VM may use this field when selecting soft references to be cleared, but it is not required to do so.
2. **clock**: Timestamp clock, updated by the garbage collector

So any GC can use these fields and defines a strategy to clear soft references like: recently-created or recently-used soft references.

```java
Counter prime = new Counter(); // prime holds a strong reference
SoftReference<Counter> soft = new SoftReference<Counter>(prime) ; //soft reference variable has SoftReference to Counter Object created at line 2
prime = null; // now Counter object is eligible for garbage collection but only be collected when JVM absolutely needs memory

```

Direct instances of this class may be used to implement simple caches; this class or derived subclasses may also be used in larger data structures to implement more sophisticated caches. Thus a sophisticated cache can, for example, prevent its most recently used entries from being discarded by keeping strong referents to those entries, leaving the remaining entries to be discarded at the discretion of the garbage collector.

## Weak Reference
A referent with only weak reference **will be** cleared by GC on checking (checking happens at GC discretion).

Weak references are most often used to implement *canonicalizing mappings*, such as a hash table whose keys and values will be removed from the map if they become otherwise unreferenced by the program.


Before moving to phantom reference we need to understand one more concept and that is `java.lang.ref.ReferenceQueue`.

#### Reference Queue
All the references also have a constructor which takes a reference of reference queue along-with the referent. When a reference is getting cleared by GC then this reference will the enqueue-d in the reference queue. This gives the control to the programmer to keep polling reference queue and knowing which reference is cleared by GC. One use case is to destroy the reference object itself as GC only clears the referent but if the reference has a strong reference (as in case of cache) the reference variable will not be cleared.

```java
ReferenceQueue refQueue = new ReferenceQueue(); //reference will be stored in this queue for cleanup
DigitalCounter digit = new DigitalCounter();
PhantomReference<DigitalCounter> phantom = new PhantomReference<DigitalCounter>(digit, refQueue);
```

## Phantom Reference
Phantom references are most often used for scheduling pre-mortem cleanup actions in a more flexible way than is possible with the Java finalization mechanism. This is written in java docs. Not sure what this actually mean but seems that the extra flexibility is reference queues. The get method of a phantom reference always returns null. It is possible to create a phantom reference with a null queue, but such a reference is completely useless: Its get method will always return null and, since it does not have a queue, it will never be enqueued.

Unlike soft and weak references, phantom references are not automatically cleared by the garbage collector as they are enqueued. An object that is reachable via phantom references will remain so until all such references are cleared or themselves become unreachable.

Invoking clear() on a phantom reference object is the coup de gras for its referent, sending the referent from the phantom reachable state to its final resting place: unreachability.

One of the link in references has mentioned that in case of weak reference the finalize is called before actual cleaning but in case of phantom reference it ic called actually after cleaning.

## Comparison
Soft references are for implementing memory-sensitive caches, weak references are for implementing canonicalizing mappings that do not prevent their keys (or values) from being reclaimed, and phantom references are for scheduling pre-mortem cleanup actions in a more flexible way than is possible with the Java finalization mechanism.

Soft and weak references are automatically cleared by the collector before being added to the queues with which they are registered, if any. Therefore soft and weak references need not be registered with a queue in order to be useful, while phantom references do. An object that is reachable via phantom references will remain so until all such references are cleared or themselves become unreachable. A PhantomReference is not automatically cleared when it is enqueued, so when we remove a PhantomReference from a ReferenceQueue, we must call its clear() method or allow the PhantomReference object itself to be garbage-collected.

**The lifecycle cycle of GC is ([More info - Finalization](http://localhost:4000/java/programming/2015/05/24/garbage-collection.html)):**

The garbage collector enqueues soft, weak, and phantom reference objects in different situations to indicate three different kinds of reachability state changes. The meanings of the six reachability states and the circumstances under which state changes occur are as follow:

1. strongly reachable - An object can be reached from the roots without traversing any reference objects. An object begins its lifetime in the strongly reachable state and remains strongly reachable so long as it is reachable via a root node or another strongly reachable object. The garbage collector will not attempt to reclaim the memory occupied by a strongly reachable object.
2. softly reachable - An object is not strongly reachable, but can be reached from the roots via one or more (uncleared) soft reference objects. The garbage collector may reclaim the memory occupied by a softly reachable object. If it does so, it clears all soft references to that softly reachable object. When the garbage collector clears a soft reference object that is associated with a reference queue, it enqueues that soft reference object.
3. weakly reachable - An object is neither strongly nor softly reachable, but can be reached from the roots via one or more (uncleared) weak reference objects. The garbage collector must reclaim the memory occupied by a weakly reachable object. When it does so, it clears all the weak references to that weakly reachable object. When the garbage collector clears a weak reference object that is associated with a reference queue, it enqueues that weak reference object.
4. resurrectable - An object is neither strongly, softly, or weakly reachable, but may still be resurrected back into one of those states by the execution of some finalizer.
5. phantom reachable - An object is not strongly, softly, nor weakly reachable, has been determined to not be resurrectable by any finalizer (if it declares a finalize() method itself, then its finalizer will have been run), and is reachable from the roots via one or more (uncleared) phantom reference objects. As soon as an object referenced by a phantom reference object becomes phantom reachable, the garbage collector will enqueue it. The garbage collector will never clear a phantom reference. All phantom references must be explicitly cleared by the program.
6. unreachable - An object is neither strongly, softly, weakly, nor phantom reachable, and is not resurrectable. Unreachable objects are ready for reclamation.

From this two things can be understood:
1. The GC enqueues soft and weak reference objects when their referents are leaving the relevant reachability state, it enqueues phantom references when the referents are entering the relevant state.
2. The GC clears soft and weak reference objects before enqueueing them, but not phantom reference objects.

Thus, the garbage collector enqueues soft reference objects to indicate their referents have just left the softly reachable state. Likewise, the garbage collector enqueues weak reference objects to indicate their referents have just left the weakly reachable state. But the garbage collector enqueues phantom reference objects to indicate their referents have entered the phantom reachable state. Phantom reachable objects will remain phantom reachable until their reference objects are explicitly cleared by the program.

> Because the phantom reachable state is only attained after an object passes through the resurrectable state, a phantom reference object provides no way to access to its referent. Invoking get() on a phantom reference object always returns null, even if the phantom reference hasn't yet been cleared, because if it returned a strong reference to the phantom reachable object, it would in effect resurrect the object. Thus, once an object reaches phantom reachability, it cannot be resurrected.

### Problem with Finalization
As per doc, You should also use finalization only when it is absolutely necessary. Finalization is a non-deterministic -- and sometimes unpredictable -- process. The less you rely on it, the smaller the impact it will have on the JVM and your application.

In Effective Java, 2nd ed., Joshua Bloch says, there is a severe performance penalty for using finalizers... So what should you do instead of writing a finalizer for a class whose objects encapsulate resources that require termination, such as files or threads? Just provide an explicit termination method, and require clients of the class to invoke this method on each instance when it is no longer needed.

### When to use phantom reference

Phantom Reference can be used in situations, where sometime using finalize() is not  sensible thing to do.This reference type differs from the other types defined in java.lang.ref Package because it isn't meant to be used to access the object, but as a signal that the object has already been finalized, and the garbage collector is ready to reclaim its memory.

### References

1. [Java doc](http://docs.oracle.com/javase/6/docs/api/java/lang/ref/package-summary.html#reachability)
2. [Finalization and Phantom References - DZone](http://java.dzone.com/articles/finalization-and-phantom)
3. [javarevisited](http://javarevisited.blogspot.in/2014/03/difference-between-weakreference-vs-softreference-phantom-strong-reference-java.html)
4. [understanding-weak-references](https://weblogs.java.net/blog/2006/05/04/understanding-weak-references)
5. [Why are Phantom References not cleared as they are enqueued?](http://stackoverflow.com/questions/7048767/why-are-phantom-references-not-cleared-as-they-are-enqueued)