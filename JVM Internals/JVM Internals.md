<!-- $theme: default -->

# JVM Internals
## Chun Li
<!-- Disclaimer: The JVM is an ongoing project. JVM internals change all the time. -->

---

<!-- Keep in mind that if you are looking at articles describing "Android JVM internals", you should remember Android runs off of Dalvik, or the DVM and has a different architecture. -->
<!-- For this presentation I am mostly referring to the HotSpot JVM, which is the reference implementation maintained by Oracle -->
# Background
redacted

---
# Threads
* So many things! What do they all do?
* `main` - thread created by good old main method
* JVM System Threads:
	* `VM Thread`
	* `GC task threads` - support different types of GC that occur
	* `VM Periodic Task Thread` - Responsible for timer events, schedules periodic operations.
	* `C1` and `C2 Compiler Threads` - JIT compilation of bytecode to native machine code
  		* `C1` has faster startup, `C2` has more agressive optimization
  	* 
---

# References
<http://blog.jamesdbloom.com/JVMInternals.html>
And a lot of StackOverflows...