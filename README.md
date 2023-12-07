
# Proyecto: Generación de Imagen de Linux con Yocto para SBC Beagle Bone

## Descripción del Proyecto
El objetivo de este proyecto es generar una imagen de Linux utilizando Yocto Project para ser utilizada en una Single Board Computer (SBC), específicamente la Beagle Bone. Yocto Project es un conjunto de herramientas que facilita la creación de sistemas embebidos personalizados.

## Pasos del Proyecto

### 1. Configuración del Entorno
Asegúrate de tener el entorno de desarrollo configurado con las herramientas necesarias, incluyendo Yocto Project.

### 2. Construir el contenedor
Construir el contenedor mediante la instrucción
docker-compose build
Esta instruccion contruira el contenedor instalando todas las dependencias necesarias para la construcción de el entorno para yocto

### 3. Levantar el contenedor
Levantamos el contenedor mediente la instrucción
```bash
docker-compose -d
```

### 4. Ingresar al contenedor 
Mediante el Id del contenedor ingresamos al contenedor
El id se puede obtener mediante la instrucción
```bash
docker ps
```
con el id ingresamos al contenedor mediante la instrucción
```bash
docker exec -it <ID_del_Contenedor> /bin/bash
```

### 5. Descargar del repositorio de yocto y configurar
Se descarga mediante la instrucción debtro de la carpeta output

```bash
cd output

git clone git://git.yoctoproject.org/poky -b kirkstone

cd poky

source oe-init-build-env 
```

luego en el archivo /output/poky/build/conf/local.conf

se cambia  

MACHINE ?= "beaglebone-yocto"

y al ultimo

RM_OLD_IMAGE = "1"
INHERIT += "rm_work"


dentro de la carpeta build se ejecuta
```bash
bitbake core-image-minimal
```

Se espera a que compile 



### 6. Se guarda la imagen en la sd

inserta imagen



# Puesta en Marcha del Sistema Básico con Yocto

**1. Asignar Memoria Estática:**
Dirígete a `/etc/network` y edita el archivo `interfaces` con `vi interfaces`. Comenta las líneas existentes y agrega la siguiente configuración con la IP deseada:

```bash
auto eth0 
iface eth0 inet static 
    address 192.168.2.250 
    netmask 255.255.255.0 
    gateway 192.168.2.1 
```

**2. Asignar Contraseña al Usuario Root:**
Ejecuta passwd root y asigna la contraseña.

**3. Ejecutar Script al Inicio para Configurar FPGA:**
Guarda el archivo de configuración del FPGA en /home/root/output_file.rbf. Crea el script programfpga.sh en /etc/init.d/ con el siguiente contenido:

```bash

echo 0 > /sys/class/fpga-bridge/fpga2hps/enable
echo 0 > /sys/class/fpga-bridge/hps2fpga/enable
echo 0 > /sys/class/fpga-bridge/lwhps2fpga/enable
dd if=/home/root/output_file.rbf of=/dev/fpga0 bs=1M
echo 1 > /sys/class/fpga-bridge/fpga2hps/enable
echo 1 > /sys/class/fpga-bridge/hps2fpga/enable
echo 1 > /sys/class/fpga-bridge/lwhps2fpga/enable
chmod 777 /etc/init.d/programfpga.sh
```
```bash
update-rc.d programfpga.sh defaults
```

**4. Ejecutar Programa Después del Inicio del Sistema:**
Crea el script /etc/init.d/miprograma con el siguiente contenido:

```bash
APP_DIR="/home/root"
APP_EXECUTABLE="testyocto"

case "$1" in
    start)
        cd $APP_DIR
        ./$APP_EXECUTABLE &
        ;;
    stop)
        pkill -f $APP_EXECUTABLE
        ;;
    restart)
        $0 stop
        $0 start
        ;;
    *)
        echo "Uso: $0 {start|stop|restart}"
        exit 1
        ;;
esac

exit 0
```
Asegúrate de dar permisos de ejecución con chmod +x testyocto y luego ejecuta 

```bash
update-rc.d miprograma defaults
```