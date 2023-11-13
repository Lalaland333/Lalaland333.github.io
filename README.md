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

