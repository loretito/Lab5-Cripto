# Laboratorio 5 - Criptografia y Seguridad en Redes

By: Loreto Ñancucheo

## Parte 1

Para compilar y levantar los contenedores, ejecutar los siguientes comandos:

```bash
sudo docker-compose build
```

```bash
sudo docker-compose up
```

Luego, para utilizar fatt y obtener el hassh del tráfico de los contenedores, primero se debe clonar el repositorio:

```bash
git clone https://github.com/0x4D31/fatt.git
```

```bash 
cd fatt
```

Una vez dentro del directorio, el comando para analizar el tráfico capturado y extraer el hassh es el siguiente:

```bash
python3 fatt.py -r ../wireshark/<NombreCaptura>.pcapng -p -j
```

## Parte 2

Para modificar la configuración de OpenSSH, primero se debe acceder al contenedor c3:

```bash
docker exec -it c3 /bin/bash
```

Dentro del contenedor, se clona el repositorio de OpenSSH:

```bash
git clone https://github.com/openssh/openssh-portable.git .
```

Luego, se edita el archivo `version.h`:

```bash
vim version.h
```

El contenido a agregar al archivo es el siguiente:

```c
/* $OpenBSD: version.h,v 1.103 2024/09/19 22:17:44 djm Exp $ */

#define SSH_VERSION     "OpenSSH_9.9"

#define SSH_PORTABLE    "p1"
#define SSH_RELEASE     SSH_VERSION SSH_PORTABLE
```

Para completar la instalación de OpenSSH, se ejecutan los siguientes comandos:


```bash
autoreconf
./configure 
make 
make install
```

## Parte 3

Para reducir el tamaño del Key Exchange Init del servidor a menos de 300 bytes, primero se debe acceder al contenedor c4:

```bash
docker exec -it c4 /bin/bash
``` 

Se edita el archivo `/etc/ssh/sshd_config`:

```bash
vim /etc/ssh/sshd_config
```

Se deben agregar las siguientes líneas al archivo:

```txt
Ciphers aes128-ctr
HostKeyAlgorithms ecdsa-sha2-nistp256
KexAlgorithms ecdh-sha2-nistp256
MACs hmac-sha2-256
```

Finalmente, se sale del contenedor y se reinicia para aplicar los cambios:

```bash
docker restart c4
```
