Vagrant.configure("2") do |config|
    
    # Sync host working directory to virtual machine
    config.vm.synced_folder '.', '/vagrant'

    # SSH settings - Uses vagrant insecure key
    config.ssh.insert_key = false

    $add_repository_key = <<-SCRIPT
    echo "Installing Jenkins repository key"
    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    SCRIPT

    $add_repository = <<-SCRIPT
    echo "Adding Jenkins repository"
    add-apt-repository 'deb https://pkg.jenkins.io/debian-stable binary/'
    SCRIPT

    $install_java_jdk8 = <<-SCRIPT
    apt-get update
    apt install -y openjdk-8-jre
    SCRIPT

    $install_jenkins = <<-SCRIPT
    apt install -y jenkins
    SCRIPT

    # VirtualBox.
    config.vm.define "jenkins2" do |virtualbox|
      virtualbox.vm.hostname = "jenkins2"
      virtualbox.vm.box = "ubuntu/xenial64"
      virtualbox.vm.box_version = "20190419.0.0"
      virtualbox.vm.box_check_update = true
      virtualbox.vm.define "jenkins2"
      virtualbox.vm.network "private_network", type: "dhcp",
          virtualbox__intnet: "NatNetwork"
      virtualbox.vm.network "forwarded_port", guest:8080, host:8081
      config.vm.provider :virtualbox do |v|
        v.gui = false
        v.memory = 4096
        v.cpus = 4
        v.name = "jenkins2"
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
      end
      config.vm.provision "shell", inline: $add_repository_key
      config.vm.provision "shell", inline: $add_repository
      config.vm.provision "shell", inline: $install_java_jdk8
      config.vm.provision "shell", inline: $install_jenkins
      config.vm.provision "shell", inline: "echo Hello, World"
    end

  end