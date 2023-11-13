# MPI ERROR： BAD TERMINATION OF ONE OF YOUR APPLICATION PROCESSES

I met an error when I excute an application, the messages are follows:


=========================================
=   BAD TERMINATION OF ONE OF YOUR APPLICATION PROCESSES
=   PID 433969 RUNNING AT c1u12n01
=   EXIT CODE: 9
=   CLEANING UP REMAINING PROCESSES
=   YOU CAN IGNORE THE BELOW CLEANUP MESSAGES
=========================================


I use LSF system to submit an MPI job.

#BSUB -J Jpj
#BSUB -n 360
#BSUB -o 20231110Ex2-152.out
#BSUB -e 20231110Ex2-152.err
#BSUB -W 720
#BSUB -q batch
#BSUB -R "span[ptile=36]"
module purge
module load mpi/impi-2017u3-intel-2017u4
cd $LS_SUBCWD
mpirun ./main 

# 出错原因
MPI exit code 9 means that the program recieved the SIGKILL event, see https://www.intel.com/content/www/us/en/develop/documentation/mpi-developer-guide-linux/top/troubleshooting/error-message-bad-termination.html. This means that the process was killed by an outside process like the job scheduler or the OS. The reason is typically that the program used more memory than it was allowed or that a runtime limit was exceeded. Be sure to submit the job in way so that the job scheduler sends you an email when it aborts the program. Such an email will typically state the reason for why the job was killed.

# 常见 Error Message: Bad Termination

NOTE: The values in the tables below may not reflect the exact node or MPI process where a failure can occur.

# Case 1

Error Message

===================================================================================
=   BAD TERMINATION OF ONE OF YOUR APPLICATION PROCESSES
=   RANK 1 PID 27494 RUNNING AT node1
=   KILLED BY SIGNAL: 11 (Segmentation fault)
===================================================================================
or:

===================================================================================
=   BAD TERMINATION OF ONE OF YOUR APPLICATION PROCESSES
=   RANK 1 PID 27494 RUNNING AT node1
=   KILLED BY SIGNAL: 8 (Floating point exception)
===================================================================================
Cause

One of MPI processes is terminated by a signal (for example, Segmentation fault or Floating point exception) on the node01.

Solution

Find the reason of the MPI process termination. It can be the out-of-memory issue in case of Segmentation fault or division by zero in case of Floating point exception.

# Case 2

Error Message

================================================================================
= BAD TERMINATION OF ONE OF YOUR APPLICATION PROCESSES
= RANK 1 PID 20066 RUNNING AT node01
= KILLED BY SIGNAL: 9 (Killed)
================================================================================
Cause

One of MPI processes is terminated by a signal (for example, SIGTERM or SIGKILL) on the node01 due to:

the host reboot;
an unexpected signal received;
out-of-memory manager (OOM) errors;
killing by the process manager (if another process was terminated before the current process);
job termination by the Job Scheduler (PBS Pro*,  SLURM*) in case of resources limitation (for example, walltime or cputime limitation).
Solution

Check the system log files.
Try to find the reason of the MPI process termination and fix the issue.

# 参考链接
https://forum.quantumatk.com/index.php?topic=10079.0
https://www.intel.com/content/www/us/en/docs/mpi-library/developer-guide-linux/2021-6/error-message-bad-termination.html
https://community.intel.com/t5/Intel-oneAPI-Math-Kernel-Library/BAD-TERMINATION-OF-ONE-OF-YOUR-APPLICATION-PROCESSES/m-p/1190675
