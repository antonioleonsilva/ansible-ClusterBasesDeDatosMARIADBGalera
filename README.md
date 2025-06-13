# Clúster de Bases de Datos MariaDB Galera - Ansible 
Despliegue automatizado con Ansible de un clúster de bases de datos MariaDB Galera en nodos físicos con balanceador de carga (HAProxy alojado en Docker) para repartir el tráfico entre los nodos del clúster y PHPMyAdmin (alojado en Docker) como interfaz de administración de las bases de datos.

## Tabla de Contenidos

- [Descripción](#descripción)
- [Características](#características)
- [Requisitos](#requisitos)
- [Adaptación](#adaptación)
- [Instalación](#instalación)

## Descripción

Este proyecto automatiza el despliegue de un clúster de Bases de datos MariaDB Galera de alta disponibilidad con múltiples nodos, con balanceador de carga y con interfaz gráfica de administración utilizando Ansible.

## Características

- Instalación y configuración automatizada con Ansible.
- Clúster de Bases de datos altamente disponible y con redundancia de datos.
- Fácil adaptación a distintos entornos y nodos.
- Servicio altamente escalable.

## Requisitos

- Ansible versión 2.17 o superior
- Docker instalado y funcionando en el nodo/s en el que se implementará
- Acceso SSH configurado y permisos necesarios para Ansible en todos los nodos. Se recomienda el uso de autentificación con par de claves para evitar indicar los usuarios y contraseñas de los nodos en texto plano en el fichero de inventario.

## Adaptación
- Modifica el archivo [intentory/hosts.ini](inventory/hosts.ini) para definir los nodos que formarán parte del clúster (grupo nodos_cluster) y el/los nodo/s que alojarán los contenedores de Docker. 
- El despliegue está diseñado para que solo sea necesario indicar los nodos clientes en el fichero de inventario. No es necesario modificar ningún otro fichero para que el despliegue diseñado funcione, pero sientete con libertad de modificarlo según tus necesidades.

> ⚠️ **Advertencia:** Este proyecto contiene la figura de un nodo router (Ubuntu Server) que en el diseño original reenviaba los puertos desde una red externa hacia la topología permtiendo así que el servicio web sea accesible desde redes ajenas a la que pertenecian los nodos participantes al despliegue. Si en tu topoligía no dispones de esta figura **y por lo tanto, por defecto, este despliegue solo funcionará en tu red local**, no debes ejeuctar la tarea que crearía la regla de reenvío de puertos en el nodo router. Para ello, en el fichero principal de tareas [main.yaml](main.yaml), elimina la tarea llamada  *Reenvío de puertos*.

## Instalación

Clona este repositorio y ejecuta el playbook con el fichero de inventario personalizado según tu topología:

```bash
git clone https://github.com/antonioleonsilva/ansibe-sistemaMonitorizacionMultinodo.git
cd ansible-sistemaMonitorizacionMultinodo
ansible-playbook -i inventory/hosts.ini main.yaml
```
Para acceder a la interfaz de PHPMyAdmin accede desde un navegador web a la dirección IP del servidor Docker por el puerto 8080. El usuario y contraseña por defecto es **admin**
```bash
http://IP_DEL_SERVIDOR:8080
```
Para utilizar el clúster con otro programa o aplicación utiliza la dirección IP del servidor Docker que aloja el balanceador de carga y el puerto 3306

