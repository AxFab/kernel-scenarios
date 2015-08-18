# Kernel scenarios

Those are test scenario for my kernel.

You will need this repository to launch functional tests.
Place both repository `kernel` and `kernel-scenarios` on the same folder.
Then use this command to launch all test and get coverage at the same time.

    make ARCH=um coverage

The last result I get was:

  - lines: 74.5% (2335 of 3133 lines)
  - functions: 86.6% (206 of 238 functions)
  - branches: 52.3% (714 of 1366 branches)

Thoses tests compensate for the lack of unit-testing.

## Write new tests. 

To write a new test you need to create a new folder and add a new line on 
kernel/src/_um/start.c:

    until = until || testCase ("nameOfTheNewFolder");

Then add one file `*.strace` by thread that will be running during your tests.
the strace file is similar to the output of an strace. The kernel will parse
those to execute the correct syscalls. those file are names with the pattern:
`{folder}/Proc{pid}-Th{tidx}.strace` (tidx is a thread id, incremented for 
each process).

Finaly don't forget the file named `Hdw.sta` which contains various lines starting
either by C, S or H, which will fix the timeline of the CPU (C for syscall, S for scheduler ticks and H for hardware interruption).
The format of this last file doesn't suit me much, but it's the best way I 
found yet to avoid randomness during tests while keeping scheduler alive.

