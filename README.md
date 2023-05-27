# Ansible WordPress Capstone

Ansible Wordpress Capstone es un proyecto desarrollado para automatizar el despliegue de un sitio web Wordpress en servidores Linux basados en RedHat o Debian. Este proyecto ha sido probado exitosamente en Centos 7 y Ubuntu 20.

Este proyecto es el resultado del Curso [Mastering Ansible Automation](https://www.coursera.org/learn/mastering-ansible-automation) de Coursera.


## Que aprendí en el curso y con el desarrollo de este proyecto?
- Conocer los conceptos básicos de la sintaxis de Ansible y la administración de un host.
- Administrar paquetes, usuarios y archivos de con Ansible.
- Administrar módulos de Ansible de terceros y proveedores de la nube.
- Implementar WordPress en una instancia de servidor Linux usando Ansible.
- Conectar la instancia a una base de datos para que WordPress almacene sus datos usando Ansible.
- Gestionar las versiones del código en GitHub.


## Instalación

Primero debes [Instalar Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) según las instrucciones para tu Sistema Operativo.

Lo siguiente, en todos los nodos administrados debe estar creado el usuario `ansibleuser`, con el que ansible se conectará a los nodos administrados. El usuario debe tener privilegios `sudo` y tener la misma contraseña en todos los nodos administrados.
```
adduser ansibleuser (managed_node1)
adduser ansibleuser (managed_node2)
...
```

Lo siguiente, copiar la clave pública del nodo de control hacia el directorio ssh del usuario `ansibleuser` de todos los nodos administrados.
```
ssh-copy-id ansibleuser@managed_node1
ssh-copy-id ansibleuser@managed_node2
...
```

Clona el repositorio en el nodo de control
```
git clone https://github.com/edssn/Ansible-Capstone.git
```

En el nodo de control, modifica el archivo `Ansible-Capstone/inventory/main.yml` y agrega las ips de tus nodos administrados
```
all:
  hosts:
    ip_managed_node1:
    ip_managed_node2:
    ...
```

Ejecuta el playbook de ansible
```
ansible-playbook -i inventory/main.yml wordpress.yml -K

BECOME password:
```

La ejecución del comando solicitará una contraseña. Debes ingresar la contraseña del usuario `ansibleuser` Al ingresar la contraseña, las tareas del playbook iniciaran su ejecución en todos los nodos administrados.
```
PLAY [all] *******************************************************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************************************
ok: [192.168.56.105]
ok: [192.168.56.106]

TASK [wordpress : Print My Os Family] ****************************************************************************************************************************************************
ok: [192.168.56.105] => {
    "ansible_os_family": "RedHat"
}
ok: [192.168.56.106] => {
    "ansible_os_family": "Debian"
}
......
```

Por último, abre tu navegador y escribe la ip de cada uno de tus nodos administrados en el puerto 80. Deberías visualizar la página de instalación de Wordpress

##### Nodo Administrado 1 (192.168.56.105)
![Working Image](/assets/managed_node1.png)

##### Nodo Administrado 2 (192.168.56.106)
![Working Image](/assets/managed_node2.png)