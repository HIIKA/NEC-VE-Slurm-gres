NEC-VE-Slurm-gres-Plugin
--------------------------------------------------------

gres_plugin for Slurm 
It allows for scheduling ve cards on nodes using gres in Slurm but not to share one ve between jobs.

I edited the preceding work (https://github.com/henkela/SX-Aurora-Slurm-Plugin) for the latest build, and rebuilt.

See detail ->https://github.com/henkela/SX-Aurora-Slurm-Plugin before try this.


Getting started
--------------------------------------------------------

Compiling

You should be used to compiling custom code for Slurm. Briefly:
  #. Clone this repo
  #. Run autoreconf
  #. Run ./configure --prefix=
  #. make && make install 
  #. Don't forget make contribs && make contribs-install

Slurm-Configuration
  #. You should have the nodes configured in your slurm.conf or an approriate include file. The node definition should look similar to:
    `GresTypes=ve`
    
    `Nodename=<nodename> Gres=ve:<count>`
  2. Your gres.conf shold contain at least:
    `Nodename=<nodename> Name=ve Type=<type> File=/dev/veslot[0-3]`
  3. Your cgroups.conf shold have device constrains to avoid device conflicts.
    `ContrainDevices=yes`
  4. Your cgroup_allowed_devices_file.conf should contain at the very least:
    `/dev/cpu/\*/\* `
  5. Once the configuration files are changed restart slurmctld and the slurmds. Run like:
    `srun --gres=ve:1 env|sort`
  6. Check for SLURM-variables being set, i.e. VE_NODE_NUMBER should also be there.
  7. If you want to run a mpi processes, Slurm will set VE_NODE_NUMBER like 0,1,2... , so you have to write something your own bash script to revise as 0-2 and pass params for NEC MPI because current version NEC MPI only support ``mpiexec -ve <first>[-last]`` style. Also be careful with run the job that require 2 ve cards on the 4 cards environment. NEC MPI don't recognize multiple process by ve not adjacent to each other, e.g. ``-ve 0,2``
  8. Alternative solution is use ``--exclusive`` to isolate the job. :)
  9. I'm not sure about *multihosts with multi ve cards* environment now.
