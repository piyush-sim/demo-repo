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
 
 *sim@workstation-sim:~/Work/projects/infra/vms/stage$* `vim Vagrantfile`
 
     - [Press i for insert]
     - 
     - config.vm.hostname = "stage"
     - config.vm.define = "stage"
     - config.vm.network "private_network", ip: "192.168.33.15"
     - vb.memory = "2048"
     - vb.name = "stage"
     - config.vm.provision "file", source: "provision.stage.user.sh", destination: "provision.stage.user.sh"
     - config.vm.provision "shell", path: "../provision.all.root.sh"
     - config.vm.provision "shell", path: "provision.stage.root.sh"
     - 
     - [Press Esc --> :wq]

 *sim@workstation-sim:~/Work/projects/infra/vms/stage$* `vim provision.stage.root.sh`
 
     - [Press i for insert]
     - 
     - sudo su -l  vagrant  provision.stage.user.sh
     - 
     - [Press Esc --> :wq]





