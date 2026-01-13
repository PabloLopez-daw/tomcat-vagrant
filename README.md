# tomcat-vagrant
Despliegue de Interfaces Web


1. Hacemos un vagrant init ubuntu para para generar el vagrant file con ubuntu

``` bash
    vagrant init ubuntu/jammy64
```

2. Configuramos el vagrant file y ponemos el siguiente codigo para hacer una maquina virtual

``` ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.network "forwarded_port", guest: 8080, host: 8080

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end
end
```
3. Hacemos vagrant up y vagrant ssh, levantamos la maquina y nos metemos en ella 

4. Actualizamos la maquina y instalamos el jdk

``` bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```
5. Descargamos el tomcat dentro de opt

``` bash
cd /opt
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.18/bin/apache-tomcat-10.1.18.tar.gz
```

6. Descomprimimos el tar.gz que nos han dado al descargarlo y lo movemos en otro directorio tomcat

``` bash 
sudo tar -xvzf apache-tomcat-10.1.18.tar.gz
sudo mv apache-tomcat-10.1.18 tomcat
```

