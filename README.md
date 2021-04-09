## Developing Staging VM

1 Always take snapshot of dev and cassandra box before altering anything

  *sim@workstation-sim:~/Work/projects/infra/vms/dev$* `vagrant snapshot save dev-20210408`
  
  *sim@workstation-sim:~/Work/projects/infra/vms/cassandra$* `vagrant snapshot save cassandra-20210408` 
  
2 Check Vagrant snapshot

  *sim@workstation-sim:~/Work/projects/infra/vms/dev$* `vagrant snapshot list`
  
3 Make staging

 *sim@workstation-sim:~/Work/projects/infra/vms$* `mkdir stage`
 
 *sim@workstation-sim:~/Work/projects/infra/vms$* `cp dev/* stage/`
 
 *sim@workstation-sim:~/Work/projects/infra/vms/stage$* `mv provision.dev.root.sh  provision.stage.root.sh`
 
 *sim@workstation-sim:~/Work/projects/infra/vms/stage$* `mv provision.dev.user.sh  provision.stage.user.sh`
 






