# Vagrant + Chef Apache Deployment / Despliegue Apache con Vagrant + Chef

## English

This repository contains a local infrastructure automation example that provisions an Ubuntu virtual machine with Vagrant and configures Apache using Chef Solo.

The deployment installs Chef, applies the `apache` cookbook, creates an Apache virtual host, and publishes a sample `index.html` page from the cookbook into the configured document root.

### Stack

- Vagrant
- VirtualBox
- Ubuntu Bionic 64 (`ubuntu/bionic64`)
- Chef Solo
- Apache HTTP Server

### Project Structure

```text
.
|-- Vagrantfile
|-- solo.rb
|-- node.json
|-- installChef.sh
|-- apply-chef-solo.sh
|-- index.html
`-- chef-repo/
    `-- cookbooks/
        `-- apache/
            |-- attributes/default.rb
            |-- files/index.html
            |-- recipes/default.rb
            |-- recipes/facts.rb
            `-- templates/virtual-hosts.conf.erb
```

### What the Deployment Does

- Creates an Ubuntu virtual machine with 1024 MB of RAM.
- Forwards guest port `80` to host port `8080`.
- Creates a private network using IP `192.168.33.40`.
- Installs Chef inside the virtual machine.
- Runs the Chef Solo provisioner.
- Installs the `apache2` package.
- Removes the default Apache site configuration.
- Creates a custom virtual host from `virtual-hosts.conf.erb`.
- Sets the Apache document root to `/vagrant`.
- Copies the cookbook HTML page to the document root.
- Restarts Apache when the virtual host configuration changes.

### Requirements

Install the following tools on your host machine:

- [Vagrant](https://developer.hashicorp.com/vagrant)
- [VirtualBox](https://www.virtualbox.org/)
- Internet access to download the Vagrant box and Chef installer

### How to Deploy

From the repository root, run:

```bash
vagrant up
```

Vagrant will create the virtual machine, install Chef, and execute the Chef Solo provisioning process automatically.

### Access the Web Server

After provisioning finishes, open one of these URLs in your browser:

```text
http://localhost:8080
http://192.168.33.40
```

You should see the sample UNIR Apache page.

### Useful Commands

Check the VM status:

```bash
vagrant status
```

Connect to the VM:

```bash
vagrant ssh
```

Re-run provisioning:

```bash
vagrant provision
```

Stop the VM:

```bash
vagrant halt
```

Destroy the VM:

```bash
vagrant destroy
```

### Manual Chef Solo Execution

If you need to run Chef Solo manually inside the VM:

```bash
vagrant ssh
cd /vagrant
sudo /opt/chef/bin/chef-solo -c solo.rb -j node.json
```

The Chef Solo configuration is defined in:

- `solo.rb`: cookbook path configuration.
- `node.json`: run list with `recipe[apache::default]`.

### Troubleshooting

If `http://localhost:8080` does not load:

- Confirm the VM is running with `vagrant status`.
- Re-run provisioning with `vagrant provision`.
- Verify Apache inside the VM with:

```bash
sudo systemctl status apache2
```

If the private IP does not respond:

- Make sure VirtualBox host-only networking is enabled.
- Check that the IP `192.168.33.40` is not already used by another VM.

---

## Espanol

Este repositorio contiene un ejemplo de automatizacion de infraestructura local que aprovisiona una maquina virtual Ubuntu con Vagrant y configura Apache usando Chef Solo.

El despliegue instala Chef, aplica el cookbook `apache`, crea un virtual host de Apache y publica una pagina `index.html` de ejemplo desde el cookbook hacia el document root configurado.

### Tecnologias

- Vagrant
- VirtualBox
- Ubuntu Bionic 64 (`ubuntu/bionic64`)
- Chef Solo
- Servidor HTTP Apache

### Estructura del Proyecto

```text
.
|-- Vagrantfile
|-- solo.rb
|-- node.json
|-- installChef.sh
|-- apply-chef-solo.sh
|-- index.html
`-- chef-repo/
    `-- cookbooks/
        `-- apache/
            |-- attributes/default.rb
            |-- files/index.html
            |-- recipes/default.rb
            |-- recipes/facts.rb
            `-- templates/virtual-hosts.conf.erb
```

### Que Hace el Despliegue

- Crea una maquina virtual Ubuntu con 1024 MB de RAM.
- Redirige el puerto `80` de la maquina invitada al puerto `8080` del host.
- Crea una red privada con la IP `192.168.33.40`.
- Instala Chef dentro de la maquina virtual.
- Ejecuta el provisionador Chef Solo.
- Instala el paquete `apache2`.
- Elimina la configuracion por defecto de Apache.
- Crea un virtual host personalizado desde `virtual-hosts.conf.erb`.
- Configura el document root de Apache en `/vagrant`.
- Copia la pagina HTML del cookbook al document root.
- Reinicia Apache cuando cambia la configuracion del virtual host.

### Requisitos

Instala las siguientes herramientas en tu maquina host:

- [Vagrant](https://developer.hashicorp.com/vagrant)
- [VirtualBox](https://www.virtualbox.org/)
- Acceso a internet para descargar la box de Vagrant y el instalador de Chef

### Como Desplegar

Desde la raiz del repositorio, ejecuta:

```bash
vagrant up
```

Vagrant creara la maquina virtual, instalara Chef y ejecutara automaticamente el proceso de aprovisionamiento con Chef Solo.

### Acceso al Servidor Web

Cuando termine el aprovisionamiento, abre una de estas URLs en tu navegador:

```text
http://localhost:8080
http://192.168.33.40
```

Deberias ver la pagina Apache de ejemplo de UNIR.

### Comandos Utiles

Verificar el estado de la VM:

```bash
vagrant status
```

Conectarse a la VM:

```bash
vagrant ssh
```

Ejecutar nuevamente el aprovisionamiento:

```bash
vagrant provision
```

Detener la VM:

```bash
vagrant halt
```

Destruir la VM:

```bash
vagrant destroy
```

### Ejecucion Manual de Chef Solo

Si necesitas ejecutar Chef Solo manualmente dentro de la VM:

```bash
vagrant ssh
cd /vagrant
sudo /opt/chef/bin/chef-solo -c solo.rb -j node.json
```

La configuracion de Chef Solo esta definida en:

- `solo.rb`: configuracion de la ruta de cookbooks.
- `node.json`: run list con `recipe[apache::default]`.

### Solucion de Problemas

Si `http://localhost:8080` no carga:

- Confirma que la VM esta encendida con `vagrant status`.
- Ejecuta nuevamente el aprovisionamiento con `vagrant provision`.
- Verifica Apache dentro de la VM con:

```bash
sudo systemctl status apache2
```

Si la IP privada no responde:

- Asegurate de que la red host-only de VirtualBox este habilitada.
- Comprueba que la IP `192.168.33.40` no este siendo usada por otra VM.
