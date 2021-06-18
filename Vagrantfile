Vagrant.configure("2") do |config|


    config.vm.define "docker_registry" do |docker|
       docker.vm.box = "bento/ubuntu-20.04"
       docker.vm.network "private_network", ip: '192.168.20.10'
       docker.vm.hostname = "registry"
       docker.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 2
    end
       docker.vm.provision :shell, privileged: true, inline: $install_docker
       docker.vm.provision :shell, privileged: true, inline: $config_registry
end


end



$install_docker = <<-SCRIPT
echo "Updating apt-get"
sudo apt-get -qq update
echo "Installing packages"
sudo apt-get -y install \
apt-transport-https \
ca-certificates \
url \
gnupg \
lsb-release > /dev/null 2>&1
echo "Add Docker’s official GPG key"
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg > /dev/null
echo "Set up the stable repository."
echo \
"deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
echo "Installing Docker"
sudo apt-get update > /dev/null 2>&1
sudo apt-get -y install docker-ce docker-ce-cli containerd.io > /dev/null 2>&1

echo "Start docker service"
sudo service docker start

SCRIPT

$config_registry = <<-SCRIPT

sudo sed -i '/\[ v3_ca \]/a subjectAltName = IP:192.168.20.10'  /etc/ssl/openssl.cnf

SCRIPT


# sudo sed -i '/\[ v3_ca ]/a subjectAltName = IP:192.168.20.10' /etc/ssl/openssl.cnf
#
# sed -i '/\[ v3_ca \]/a subjectAltName = IP:192.168.20.10'  /etc/ssl/openssl.cnf
