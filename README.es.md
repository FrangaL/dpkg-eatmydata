# Un contenedor simple para hacer dpkg más rápido (e inseguro)

El propósito de este paquete es intercambiar seguridad por velocidad de ejecución en
operaciones de _dpkg_ (y cualquier programa que pueda ejecutar) usando
[eatmydata] (https://launchpad.net/libeatmydata) para deshabilitar el disco
llamadas de sincronización. Esto es útil en situaciones donde la pérdida de datos en
El caso de un corte de energía no es un concierto, e.g. en la configuración efímera
_chroot_ entornos para construir paquetes Debian.

**No lo use en entornos de producción donde le interesen
¡integridad de los datos!**

En la instalación, `/usr/bin/dpkg` se hace a un lado y se reemplaza con un
secuencia de comandos de contenedor utilizando _dpkg-divert(1)_, este proceso es por supuesto invertido al eliminar este paquete.


debootstrap --include=eatmydata .....

- $R es su directorio chroot

```bash
for archive in $R/var/cache/apt/archives/*eatmydata*.deb; do
  dpkg-deb --fsys-tarfile "$archive" >$R/eatmydata
  tar -xkf $R/eatmydata -C $R
  rm -f $R/eatmydata
done
```
