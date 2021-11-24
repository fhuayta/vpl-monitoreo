# MONITOREO ENTORNO

Para que el ansible pueda funcionar mediantes un host-jump se debe tener/agregar las siguiente configuracion en el archivo "config" ubicacdo en "/home/<-USUARIO->/.ssh/config"

1) Agregar configuración "Host *"
2) Agregar configuración "Host bastionDirnoplu"

```
Host *
        Port 22
        StrictHostKeyChecking no
        UserKnownHostsFile /dev/null
        ServerAliveInterval 60
        ServerAliveCountMax 30

Host bastionDirnoplu
        Hostname dirnoplu.gob.bo
        Port 2025
        User agetic

```



El playbook de Ansible está compuesta por los siguientes componentes:

```
   ├── inventory
   ├── playbook.yml
   └── roles
       └── loki
          ├── handlers
          │   └── main.yml
          ├── tasks
          │   └── main.yml              <---- Descripción de todo el proceso
          ├── templates
          │   ├── init.service.j2       <---- Plantilla para daemon para loki
          │   └── loki-local-config.yaml.j2          <---- Configuración de host para loki
          └── vars
              └── main.yml              <---- Variables generales de todo el play book

```
El inventory consta de los hosts a ser instalados, se deben añadir las IP's correspondientes:

Dentro de la carpeta configurar los host en el archivo "inventory":
```
[<nombre_de_grupo>]
<nombre_del_hosts> ansible_host=<IP_correspondiente>

-------------ejemplo----------------
[loki]
server1 ansible_host=192.168.122.236
server2 ansible_host=192.168.122.175

```

Para el despliegue y ejecución del sistema se debe seguir los siguientes pasos:

Ingresar a la carpeta principal *ansible-loki*

```
$ cd ansible-loki
```

Ejecutando la siguiente instrucción para desplegar loki

```
$ ansible-playbook -i inventory playbook.yml -K
```

O en su defecto hacer correr el anterior script con el subfijo -kK que indica que se solicitaran las claves/contraseñas del SSH y del sudo.

La sentencia quedaria asi:

```shell
$ ansible-playbook -i inventory playbook.yml -K
```
