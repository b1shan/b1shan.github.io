---
layout: post
title:  "eBPF: A Paradigm Shift for InfoSec"
date:   2021-09-13
categories: ebpf
---
I have twice been as excited in my career. First, was Go. Second, is eBPF. 

I was writing Python. It was good, but not enough. Not enough for my needs. Then [`Go`][go] happened. Things changed. But this post isn't about Go. This is about [`eBPF`][ebpf].

As a software security engineer, I build security products. For the last few years, this involved systems programming. You know, OS stuff, agents et al. However, with the evolving threat landscape and the demands on modern service infrastructure, to me, it has exceedingly become apparent that to solve a certain class of problems in security and then evolve, implementing solutions further closer to the Linux Kernel is the way. 

This is typically done in two ways (actually, three ways): 
- Writing custom Linux Kernel Modules (LKM)
- Upstreaming patch to kernel mainline
- Maintaining your own kernel fork

Typically, the LKM route is the path of the least resistance for most use cases. LKMs are great. In fact, wonderful. We all use them knowingly or unknowingly, e.g. [iptables][iptables]. Not just open source, but this is how several commercial solutions are written, e.g. CrowdStrike [Falcon][crowdstrike]. 

Basically, if you want performance, you need to run your code in the kernel. But LKMs are high cost, high risk:
- In software, maintenance and operations costs more than writing software itself. By some magnitude. LKMs are very difficult to maintain at scale. 
- They have the potential to crash the kernel itself. They are also inherently vulnerable to security weaknesses.

However, if you are in a situation like mine: small team, LKMs are *nearly* non-starters. A vast majority of the InfoSec engineering teams that I have come across are small. Even for large teams, the economics with LKMs at scale aren't truly awesome.  

`LKMs are nearly non-starters if you can use eBPF. It is by design: low cost and low risk.` High portability results in low cost, and the runtime sandboxing equates to low risk. BTW, like LKMs, you still get the best possible performance! Allow me to go overboard and say, "It is *the future*". Frankly, at this point, to call it *a technology* would be an understatement. *eBPF is a fundamental paradigm shift if you are in InfoSec.* 

Let this fanfare not lead to any illusions. Make no mistake, Linux Kernel Modules are important. In fact, incredibly important. You know: tech debt. We all have it. *eBPF is only available on recent kernel versions, for e.g., only [Red Hat 8][rhel8] onwards you can realistically use eBPF.* 

Also, eBPF does not solve everything (not the goal), such as support for [LSMs][krsi] came not too long ago. But things are changing. Changing fast.

Meanwhile, if eBPF solves your use case, I do not see any reason why one would not use eBPF over Linux Kernel Modules, other than, of course, the need to support older Linux Kernels. Even then, I would argue using eBPF for supported Kernels.

In the upcoming posts, I will get in more detail about the specifics. Especially, my experiences. 

Not to forget, there is a great intro and further references to eBPF on its official site [`here`][ebpf].

[ebpf]: https://ebpf.io
[go]: https://golang.org/
[iptables]: https://www.netfilter.org/
[crowdstrike]: https://www.crowdstrike.com/blog/tech-center/install-falcon-sensor-for-linux/
[rhel8]: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/8.0_release_notes/rhel-8_0_0_release#kernel_technology_preview
[krsi]: https://lkml.org/lkml/2020/3/27/704