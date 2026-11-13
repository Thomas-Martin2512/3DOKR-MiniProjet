Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"

  nodes = {
    "manager" => "192.168.56.10",
    "worker1" => "192.168.56.11",
    "worker2" => "192.168.56.12"
  }

  nodes.each do |name, ip|
    config.vm.define name do |node|
      node.vm.hostname = name
      node.vm.network "private_network", ip: ip

      node.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
      end

      node.vm.provider "vmware_desktop" do |vm|
        vm.vmx["memsize"] = "2048"
        vm.vmx["numvcpus"] = "2"
      end

      node.vm.provision "shell", inline: <<-SHELL
        if ! command -v docker >/dev/null; then
          curl -fsSL https://get.docker.com | sh
          usermod -aG docker vagrant
        fi
      SHELL
    end
  end
end