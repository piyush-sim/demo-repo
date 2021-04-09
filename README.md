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
     - [Press Esc --> :wq --> Press Enter]

 *sim@workstation-sim:~/Work/projects/infra/vms/stage$* `vim provision.stage.root.sh`
 
     - [Press i for insert]
     - 
     - sudo su -l  vagrant  provision.stage.user.sh
     - 
     - [Press Esc --> :wq --> Press Enter]

 *sim@workstation-sim:~/Work/projects/infra$* `git add vms/stage/`
 
 *sim@workstation-sim:~/Work/projects/infra$* `git status`
 
 *sim@workstation-sim:~/Work/projects/infra$* `git config --global user.email "you@example.com"`
 
 *sim@workstation-sim:~/Work/projects/infra$* `git commit -m "Added stage vm" vms/cassandra/Vagrantfile vms/dev/Vagrantfile --author=ramesh`
 
 *sim@workstation-sim:~/Work/projects/infra$* `git add .gitignore`
 
 *sim@workstation-sim:~/Work/projects/infra$* `git commit -m "Added .vagrant to ignore" --author=ramesh`
 
 *sim@workstation-sim:~/Work/projects/infra$* `git status`
 
 *sim@workstation-sim:~/Work/projects/infra$* `git rm -r --cached .`
 
 *sim@workstation-sim:~/Work/projects/infra$* `git status`

 *sim@workstation-sim:~/Work/projects/infra$* `git add .`

 *sim@workstation-sim:~/Work/projects/infra$* `git pull`
 
     - Username : rames-simadvisory
     - password : ****************
 
 *sim@workstation-sim:~/Work/projects/infra$* `git push`
 
     - Username : rames-simadvisory
     - Password : ****************

4 Let's spin it up

  *sim@workstation-sim:~/Work/projects/infra/vms/stage$* `vagrant up`

  *sim@workstation-sim:~/Work/projects/infra/vms/stage$* `vagrant ssh`
  
5 Inside Stage VM
  
  *vagrant@stage* `mkdir temp`
  
  *vagrant@stage:~/temp$* `wget http://localhost:8080/`
  
  *sim@workstation-sim:~/temp$* `cd /etc/nginx/sites-enabled/`
  
  *sim@workstation-sim:/etc/nginx/sites-enabled$* `sudo vim stgsimai.myddns.me`
  
    - Password for sim : ***********
    - proxy_pass         http://192.168.33.15:8080/; # stage

  *sim@workstation-sim:/etc/nginx/sites-enabled$* `sudo nginx -s reload`
  
6 Check if stgsimai.myddns.in showing Wildfly 







  
  
  
