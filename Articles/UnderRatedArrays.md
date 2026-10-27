# Arrays are underrated

They were probably the first data structure you learned about, they were probably the structure you first reached for when solving any problem you could think of and then you learnt about more complex structures such as graphs, trees, DeQueues etc and you started to prioritise using these things over the humble array and I get it you want to learn new things you want to have a big toolset for the correct problem and that is absolutely understandable.
The problem is you may be using a hammer to knock in a nail or even worse you may be spending all your time and energy charging your batteries and unpacking your power saw when you could have finished the job faster with your hand saw all along.

So this article is all about the humble array! When to use this amazing structure, when to avoid it and since all spectrums can be split into threes when you are in a big pot of maybe as well!

## The good

From my stint in the world of games development I was genuinely surprised to see the amount of code that boils down to just a looping array. Sure there are systems in place that reduce the amount this occurs and and 9 times out of 10 that data is either an array of pointers or a contiguous block of component data, both approaches benefiting from being laid out predictably in memory. Fundamentally many of the systems that need to be the most performant use this system. 

Probably the most famous of this is the ECS (Entity component system) which does exactly that (perhaps a full article on this later) by creating entities which are really just IDs that reference collections of components stored elsewhere in memory. So the component may consist of just a gravity script and the reality is that in some part of the engine there is probably just a big list of all the entities with a gravity component that is run through in a loop during the physics update.

However there is a damned good reason for their frequency despite their simplicity! When used correctly they are fast! They are compact! and ultimately they are recognisable to both seniors and juniors alike.

### The speed

Why are they so fast you may ask? Well when I was first starting out I thought this was a really simple question with a simple answer.

"Because they are all next to each other."

It turns out my more naive self was somewhat correct, but maybe we can do better than just that.

Contiguous memory

Arrays are stored in a single block of memory.
Accessing the next element is just moving to the next address.
Compare this to linked lists where every node could be anywhere in memory.

CPU cache lines

CPUs don't fetch one integer at a time.
They fetch chunks of memory called cache lines.
If you access one array element, you've often already loaded several neighbouring elements "for free".

Spatial locality

Programs often access nearby data shortly after accessing current data.
Arrays naturally benefit from this because nearby elements are physically nearby in memory.
This is the real reason "next to each other" matters.

Hardware prefetching

Modern CPUs are clever.
If they see you walking through an array sequentially, they'll often start loading future cache lines before you even ask for them.
The CPU is effectively predicting your next move.

Branch prediction

Tight array loops tend to be very predictable.
Predictable code is easier for CPUs to optimise.
A simple for loop often lets the processor stay in its happy place.

SIMD / Vectorisation

Compilers can sometimes process multiple array elements at once.
For example adding 4, 8, or even 16 values in a single instruction.
This gets much harder with scattered data structures.

Less pointer chasing

Following pointers means waiting for memory locations that aren't known beforehand.
Arrays let the CPU know exactly where the next element will be.
This reduces stalls.

Cache misses are expensive

Accessing RAM is dramatically slower than accessing cache.
Often the performance difference isn't "array vs linked list".
It's really "cache hit vs cache miss".

Memory overhead

Arrays store mostly data.
Linked structures typically need extra pointers.
More pointers means more memory and often poorer cache utilisation.

The caveat

Arrays are fast when you're mostly reading or iterating.
They're less amazing when you're constantly inserting or removing items in the middle.
This transitions nicely into your "The Bad" section later.
