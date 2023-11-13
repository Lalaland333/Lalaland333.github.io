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


参考链接
https://forum.quantumatk.com/index.php?topic=10079.0
https://www.intel.com/content/www/us/en/docs/mpi-library/developer-guide-linux/2021-6/error-message-bad-termination.html
https://community.intel.com/t5/Intel-oneAPI-Math-Kernel-Library/BAD-TERMINATION-OF-ONE-OF-YOUR-APPLICATION-PROCESSES/m-p/1190675
