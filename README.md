# rowhammer
Experimental Linux kernel module that attempts to offer an in-kernel defence against the "rowhammer" exploit

# What is rowhammer? 

Rowhammer is a well-documented exploit that allows an attacker to corrupt data in memory. The attacker could find the memory address of a bit adjacent to the memory of interest, at which point they could repeatedly hammer that bit, and as the memory controller must rewrite the data after every read, this can cause neighbour bits to discharge slightly, potentially causing the bit to flip. 

This leads to corruption of data. Due to the fact that it affects virtually all platforms, it is extremely dangerous, however it is difficult to exploit since it is difficult to find memory adjacent to the data of interest to begin with.

# How can we defend against it?

My implementation will work by implementing an interrupt for roughly every million cache misses. We can track the cache miss statistics using the PMU built into most modern CPUs.
However, this comes at a price. Our module will trigger when cache-miss rate exceeds a certain amount, forcing the CPU to delay by about 64ms. Naturally, a 64-ms delay is far, far from ideal, and this will be especially noticeable on real-time and latency-sensitive systems. **Hence why this is tagged as experimental and I would strongly advise against importing into your kernel**.

# Installation

Again, **I would not recommend installing it**, for the reasons mentioned above.

But if you want to test it, download the code as a .zip and extract it to a folder. Open a terminal in that directory, and first compile by running make.

Then, run sudo insmod rowhammer.ko

You can check if it's been loaded by running lsmod.

Uninstall by running sudo rmmod rowhammer
