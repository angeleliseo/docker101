# Archivo de configuracion para Maquina Virtual con Docker
Solo hace falta que copies y pegues este el bloque de codigo abajo en un archivo llamado vagrantfile sin extension en un directorio de tu computadora
y ejecutes el comando

```cmd
vagrant up
```
Esto levantara una maquina virtual lista para usarse con docker

Puedes entrar por ssh a tu maquina virtual con el sigiuente comando
```cmd
vagrant ssh
```

## Codigo a copiar

```vagrant
Vagrant.configure("2") do |config|
  # IT Learning Mexico, maquina para practicas docker

  config.vm.box = "shekeriev/ubuntu-20-04-server"
  config.vm.synced_folder ".", "/vagrant", type: "nfs", crate: "true"
  config.vm.define "ITLMX-Docker-Lab" do |dlab|
    dlab.vm.hostname = "itlmx-docker-lab"
    dlab.vm.network "private_network", ip: "192.168.33.11"
    dlab.vm.network "forwarded_port", guest: 8080, host: 8080, host_ip: "0.0.0.0"
    dlab.vm.network "forwarded_port", guest: 33060, host: 33060, host_ip: "0.0.0.0"
    dlab.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.name = "itlmx-docker-lab"
    end
    dlab.vm.provision "shell", inline: <<-SHELL
      sudo apt install docker.io -y
      docker --version
      echo "Docker instalado!"
      sudo gpasswd -a vagrant docker
      echo "Usuario local agregado a el grupo de docker"
    SHELL
  end
end
```
