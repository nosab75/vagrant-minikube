$script = <<-'SCRIPT'
	apt update -y
	apt install -y curl wget apt-transport-https
	apt install docker.io -y
	curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
	install minikube-linux-amd64 /usr/local/bin/minikube
	usermod -aG docker $USER
	curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
	chmod +x kubectl
	mv kubectl /usr/local/bin/
        echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
        sysctl -p
        echo "root:root" | chpasswd
	echo 'source <(kubectl completion bash)' >> ${HOME}/.bashrc && source ${HOME}/.bashrc
	minikube start --force --driver=docker

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "master" do |master|
  master.vm.box = "generic/ubuntu1804"
  master.vm.hostname = "master"
  master.vm.network :public_network, ip: "192.168.1.99"
  #master.vm.provision "shell", inline: "echo root:root | chpasswd"
  master.vm.provision "shell", inline: $script
  master.vm.box_download_insecure=true
  master.vm.provider "virtualbox" do |v|
      #v.gui = true
      v.memory = 2048
      v.cpus = 2
    end
end

  config.vm.define "win10" do |win10|
  win10.vm.box = "gusztavvargadr/windows-10"
  win10.vm.hostname = "win10"
  win10.vm.network :public_network, ip: "192.168.1.100"
  win10.vm.box_download_insecure=true
  win10.vm.provider "virtualbox" do |v|
      v.gui = true
      v.memory = 2048
      v.cpus = 2
    end
end
end
