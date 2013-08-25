---
layout: post
title: "the properties of phase-fair rw lock"
date: 2013-08-25 22:58
comments: true
categories: GSoc RTEMS
---

Phase-fair reader writer locks is a new class of reader-writer locks. Phase-fair rw lock has the following properties:
	1.	Reader phases and writer phases alternate.
	2.	Writers are subject to FIFO ordering, but only with regard to other writers.
	3.	At the start of each reader phase, all currently-unsatisfied read requests are satisfied (exactly one write request is satisfied at the start of a writer phase) 
	4.	And during a reader phase, newly-issued read requests are satisfied only if there are no unsatisfied write requests pending.

In some sense, phase-fair locks can be understood as being a reader preference lock when held by a writer, and being a writer preference lock when held by readers, i.e., readers and writers are “polite” and yield to each other. For example:

	1.	T1-> thread1 issues a read request then satisfied.
	2.	T2-> thread2 issues a write request then unsatisfied(because thread1 in read phase).
	3.	T3-> thread3 issues a read request then unsatisfied(because thread2 writer already wait).
	4.	T4-> thread4 issues a write request then unsatisfied(wait in FIFO order regard to thread2)
	5.	T5-> thread1 releases read lock and then thread2 write request satisfied.
	6.	T6-> thread5 issues a read request then unsatisfied.
	7.	T7-> thread2 releases write lock and then thread3 and thread5 read request satisfied at the same time.
	8.	T8-> thread3 and thread5 release their read lock, then thread1 write request satisfied. 

Reference:

 1: http://www.rtems.org/ftp/pub/rtems/people/chrisj/source-builder/source-builder.html
 
 2: http://concurrencykit.org/cgit/cgit.cgi/ck/tree/include/ck_pflock.h
