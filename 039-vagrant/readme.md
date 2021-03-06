#Build Podcast ep 039 - Vagrant
[Screencast link](http://build-podcast.com/vagrant/)

[Vagrant](http://www.vagrantup.com/) creates reproducible, portable development environments that can be packaed and passed on to other developers to create an exact copy. In this episode, we will learn how to installed virtual machines with packages as well as view them in the host machine. We will run Vagrant wuth a simple html page and a [node.js](http://nodejs.org/) "hello world" project.

version: 1.2.1

#Background on Vagrant 

1. [Main website](http://www.vagrantup.com/)
2. [Github](https://github.com/mitchellh/vagrant)
3. [Documentation](http://docs.vagrantup.com/v2/)

#Things to learn with Vagrant 

##1. install

1. [download](http://downloads.vagrantup.com/)
2. check version in the command line:

    ```
    $ vagrant --version
    Vagrant version 1.2.0
    $ vagrant --help
    $ vagrant <command> -h
    ```
1. start a vagrant config file in an empty project folder. this will create `Vagrantfile`

    ```
    $ mkdir project_name
    $ cd project_name
    $ vagrant init
    ```

1. edit `Vagrantfile` to add the boxname

    ```
    Vagrant.configure("2") do |config|
      config.vm.box = "boxname"
    end
    ```
    
1. add a base image or box to quickly clone a virtual machine. this will create a folder `.vagrant` with an [ubuntu box](http://www.vagrantbox.es/)

    ```
    $ vagrant box add boxname http://files.vagrantup.com/precise32.box
    $ vagrant box list
    $ vagrant box remove <name> <provider>
    ```

1. boot up the guest machine

    ```
    $ vagrant up
    ```
    
1. ssh into the machine

    ```
    $ vagrant ssh
    ```
1. exit ssh 

    ```
    $ exit
    ```
    
1. 3 ways of teardown:
    1. save current running state of machine and stop it

        ```
        vagrant suspend
        ```
    1. cleanly shut down machine
    
        ```
        vagrant halt
        ```
    1. nothing left - disk space and RAM is reclaimed by host machine
        
        ```
        vagrant destroy
        ```
1. remove box

    ```
    vagrant box remove <boxname> <provider>
    ```
 
   
##2. synced files

1. in the guest virtual machine

    ```
    $ vagrant ssh
    $ ls /vagrant
    Vagrantfile
    ```
    
1. create a file on local machine and view it in the guest virtual machine

    ```
    $ touch index.html
    $ vagrant ssh
    $ ls /vagrant
    index.html Vagrantfile
    ```
1. create a file in the guest virtual machine and view it in the local machine

    ```
    $ vagrant ssh
    $ touch /vagrant/readme.md
    $ exit
    $ ls
    index.html readme.md Vagrantfile
    ```

##3. installing softwares

1. in the same folder as the file `Vagrantfile`, create a new file `bootstrap.sh` with the following code:

    ```
    #!/usr/bin/env bash
    apt-get update
    apt-get install -y apache2
    rm -rf /var/www
    ln -fs /vagrant /var/www
    ```
1. configure Vagrant to run the script for installing and setup port forwarding in host machine:

    ```
    Vagrant.configure("2") do |config|
      config.vm.box = "boxname"
      config.vm.provision :shell, :path => "bootstrap.sh"
      config.vm.network :forwarded_port, host: 4567, guest: 80
    end
    ```
1. run the installations
    - if the virtual machine is not running

    ```
    $ vagrant up
    ```
    - if the virtual machine is already running
    
    ```
    $ vagrant reload 
    ```

##4. package box for reusing

1. create an inline script inside file `Vagrantfile`


    ```
    $script = <<SCRIPT
    apt-get update
    apt-get install -y apache2
    rm -rf /var/www
    ln -fs /vagrant /var/www
    SCRIPT
    
    Vagrant.configure("2") do |config|
      ...
      config.vm.provision :shell, :inline => $script
      ...
    end
    ```

1. package the currently running environment into a re-usable box called `package.box`

    ```
    $ vagrant up
    $ vagrant package --output package.box --vagrantfile Vagrantfile
    ```


##5. reusing box

1. create another brand new project folder that has the same `package.box` 

    ```
    $ vagrant init # amend the box name accordingly
    $ vagrant box add <new-boxname> <package.box>  
    $ vagrant up  
    ```

##6. nodejs with ubuntu box

1.  create a new `Vagrantfile` in a new folder

    ```
    $script = <<SCRIPT
    apt-get update
    echo "Y" | apt-get install nodejs
    node --version
    ln -fs /vagrant /home/vagrant/<new-folder-name>
    SCRIPT
    
    Vagrant.configure("2") do |config|
      config.vm.box = "base"
      config.vm.box_url = "http://files.vagrantup.com/precise32.box"
      config.vm.provision :shell, :inline => $script
      config.vm.network :forwarded_port, host: 3000, guest: 3000
    end
    ```
1. create a new `index.js` hello world for nodejs

    ```
    var http = require('http');
    http.createServer(function (req, res) {
      res.writeHead(200, {'Content-Type': 'text/plain'});
      res.end('Hello World with Vagrant!\n');
    }).listen(3000);
    console.log('Server running at http://127.0.0.1:3000/');
    ```
1. start the guest machine:

    ```
    $ vagrant up
    ```
    
1. start the node server in the guest machine and visit [127.0.0.1:3000](http://127.0.0.1:3000) in the browser

    ```
    $ vagrant ssh
    $ cd <folder-name>
    $ node index.js
    ```
 

#More Resources on Vagrant 
1. [Vagrant generator](http://rove.io/)
1. [Vagrant boxes](http://www.vagrantbox.es/)
1. [Vagrant - what, why and how](http://net.tutsplus.com/tutorials/php/vagrant-what-why-and-how/)
2. [Introduction to Vagrant](http://www.slideshare.net/salizzar/introduction-to-vagrant)

#Related Build Podcast episodes
1. [Virtual box](http://build-podcast.com/virtualbox/)
2. [SSH](http://build-podcast.com/ssh/)
3. [Apache](http://build-podcast.com/apache/)

#Build Link of this episode

1. [codepen.io](http://codepen.io/)