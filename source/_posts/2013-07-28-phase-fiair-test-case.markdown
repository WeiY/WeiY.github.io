---
layout: post
title: "phase fiair test case"
date: 2013-07-28 23:51
comments: true
categories: GSoc RTEMS
---

The new atomic API draft design has been completed and the first version has been push into RTEMS mainline. Now the atomic ops implemenation is dependent on C11 stdatomic.h, so if the toolchain of some architecture does not support stdatomic.h it will not have atomic support(now it will post some error log when in the process of rtems compiling). And arm architecture can build successfully using atomic support. But sparc architecture lack of 'cas' instruction support in the toolchain. About this issue i have post a mail to gcc org [atomic support for LEON3 platform][1] and the gcc developer will consider add 'cas' support for LEON3.

Then i am working on the new atomic test case design. The first step is to provide some example about usage of atomic API. There is a good reference about atomic API design and test case design, [Concurrency kit][2]. It has support many atomic API and also provide lots of test case for according API. Becasue phase fair lock is very important for embedded system such RTEMS, i start port the phase fair lock test case from Concurrency kit to RTEMS. Now it has been completed, code is uploaded to github [phase fair test case][3]. There is still some problem about the printf under the SMP environment, i will try to make it work and submit to mainline.

[1]: http://gcc.gnu.org/ml/gcc/2013-07/msg00272.html "RTEMS Source Builder"
[2]: http://concurrencykit.org/ "Concurrency kit"
[3]: https://github.com/WeiY/rtems/commit/25fb61a5252db423b8f55c0d57c151c7271ebfdb "phase fair test case"