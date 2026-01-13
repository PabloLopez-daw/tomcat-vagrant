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

7. Le damos los permisos necesarios al tomcat 
``` bash
sudo chown -R vagrant:vagrant /opt/tomcat
sudo chmod -R 755 /opt/tomcat
```

8. Arramcamos el Tomcat con el siguiente comando 
``` bash
/opt/tomcat/bin/startup.sh
```

# Prueba 

9. Configuramos los usuarios de tomcat con los siguientes codigos 
``` bash
sudo nano /opt/tomcat/conf/tomcat-users.xml
```

``` xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>

  <user username="admin" password="admin123"
        roles="manager-gui,manager-script"/>
</tomcat-users>

```

10. Apagamos el tomcat y lo volvemos a iniciar 
``` bash 
/opt/tomcat/bin/shutdown.sh
/opt/tomcat/bin/startup.sh
```

# Tarea 

1. Clonar reposistorio de github y nos movemos a la rama correcta 
``` bash 
git clone https://github.com/cameronmcnz/rock-paper-scissors.git
cd rock-paper-scissors
git checkout patch-1
```
