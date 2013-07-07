---
layout: post
title: "New implementation of atomic for RTEMS"
date: 2013-07-07 14:23
comments: true
categories: GSoc RTEMS
---

Now lots of operation system have support their own atomic implementation. In the last year's GSOC project i have also design atomic API for RTEMS and also ported the architecture-dependent implementation from FreeBSD to RTEMS. In the original design i have consider the roadmap of the atomic support for RTEMS, its first principle is to be easy to move to the C11 atomic implementation. Now it is time to move to C11 atomic because GCC4.8 and clang3.3 have supported this feature already. 

My mentor Sebastian Huber have ported the stdatomic.h from FreeBSD to newlib. You can build it from the RSB(Rtems Source Builder). The build command is as follow (I build it not based on the latest RSB, If the latest RSB support stdatomic.h ignore me):
{% codeblock lang:bash %}
git clone git://git.rtems.org/rtems-source-builder.git
cd rtems-source-builder
patch -p1 < stdatomic-newlib.patch
cd rtems
../source-builder/sb-set-builder --log=l-arm.txt 1 --prefix=$HOME/development/rtems/4.11 4.11/rtems-arm
{% endcodeblock %}
stdatomic-newlib.patch is as follow :
{% codeblock lang:bash %}
diff --git a/rtems/config/tools/rtems-gcc-4.8.1-newlib-cvs-1.cfg b/rtems/config/tools/rtems-gcc-4.8.1-newlib-cvs-1.cfg
index f39225d..458aa8b 100644
--- a/rtems/config/tools/rtems-gcc-4.8.1-newlib-cvs-1.cfg
+++ b/rtems/config/tools/rtems-gcc-4.8.1-newlib-cvs-1.cfg
@@ -7,7 +7,7 @@
 %include %{_configdir}/versions.cfg
 
 %define gcc_version    4.8.1
-%define newlib_version 31-May-2013
+%define newlib_version 15-June-2013
 %define mpfr_version   3.0.1
 %define mpc_version    0.8.2
 %define gmp_version    5.0.5
{% endcodeblock %} 
For mow information about RSB usage please refer the [RTEMS Source Builder][1]

If everything is ok you will see the toolchains have been installed under $HOME/development/rtems/4.11. 

This week i have finished the new atomic API design and implementation for arm architecture based on the stdatomic.h. But now it is only working on SMP mode (tested on bsp realview_pbx_a9_qemu), the TODO list in later will be:
	1.	Add the check to whether some architecture support stdatomic.h
	2.	New test cased design.
	3.	Other architectures support.


[1]: http://www.rtems.org/ftp/pub/rtems/people/chrisj/source-builder/source-builder.html "RTEMS Source Builder"