# Tomcat con Vagrant

Despliegue de Interfaces Web en una máquina virtual Ubuntu

---

## 1. Inicializar Vagrant con Ubuntu

Creamos un `Vagrantfile` usando la caja de Ubuntu Jammy:

```bash
vagrant init ubuntu/jammy64
```

---

## 2. Configuración del Vagrantfile

Editamos el `Vagrantfile` para definir la VM, redireccionamiento de puertos y recursos:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  # Redireccionamos el puerto 8080
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # Configuración de VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end
end
```

---

## 3. Levantar la máquina virtual

Iniciamos la VM y accedemos por SSH:

```bash
vagrant up
vagrant ssh
```

---

## 4. Actualizar la máquina e instalar JDK

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

---

## 5. Descargar Apache Tomcat

Nos movemos a `/opt` y descargamos Tomcat 10:

```bash
cd /opt
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.18/bin/apache-tomcat-10.1.18.tar.gz
```

---

## 6. Descomprimir y organizar Tomcat

```bash
sudo tar -xvzf apache-tomcat-10.1.18.tar.gz
sudo mv apache-tomcat-10.1.18 tomcat
```

---

## 7. Asignar permisos a Tomcat

```bash
sudo chown -R vagrant:vagrant /opt/tomcat
sudo chmod -R 755 /opt/tomcat
```

---

## 8. Iniciar Tomcat

```bash
/opt/tomcat/bin/startup.sh
```

---

## 9. Configuración de usuarios de Tomcat

Editamos el archivo `tomcat-users.xml` para añadir un usuario administrador:

```bash
sudo nano /opt/tomcat/conf/tomcat-users.xml
```

Contenido:

```xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  
  <user username="admin" password="adminpassword"
        roles="manager-gui,manager-script"/>
</tomcat-users>
```

Reiniciamos Tomcat para aplicar los cambios:

```bash
/opt/tomcat/bin/shutdown.sh
/opt/tomcat/bin/startup.sh
```

---

## 10. Tarea: Despliegue de aplicación web

### Clonar el repositorio

```bash
git clone https://github.com/cameronmcnz/rock-paper-scissors.git
cd rock-paper-scissors
git checkout patch-1
```

### Editar `pom.xml` para añadir plugin de Tomcat

```xml
<plugin>
   <groupId>org.apache.tomcat.maven</groupId>
   <artifactId>tomcat7-maven-plugin</artifactId>
   <version>2.2</version>
   <configuration>
      <url>http://localhost:8080/manager/text</url>
      <server>TomcatServer</server>
      <path>/juego-suerte</path>
   </configuration>
</plugin>
```

### Desplegar la aplicación

```bash
mvn clean compile package tomcat7:deploy
mvn tomcat7:redeploy
sudo cp target/roshambo.war /opt/tomcat/webapps/juego-suerte.war
```

---

✅ **¡Listo!** Ahora la aplicación debería estar accesible en `http://localhost:8080/juego-suerte`.
