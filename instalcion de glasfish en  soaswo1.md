### Para instalar   glassfish  en el servidor    soawso se    tuvieron que  bajar   paquetes para poder  hacer la instalación,  los paquetes que se bajaron son los  siguientes:

```bash
rpm -i libopenssl1.0.0-1.0.2e-3.mga6.i586.rpm

rpm -i libopenssl1.0.0-1.0.0j-1.mga2.i586.rpm

rpm -i libpq5-9.4.5-3.mga6.i586.rpm
```

posteriormente se  descargo el paquete   para  postgress 


 postgresql90-libs-9.0.20-1PGDG.rhel6.x86_64.rpm



se  ejecuto el  comando 

```bash
rmp -i postgresql90-libs-9.0.20-1PGDG.rhel6.x86_64.rpm

```





## Instalacion de glassfish


###   se creo   un  directorio glassfish en la siguiente ruta

/usr/local/glassfish

5.3  se   descargo  y descomprimio   glassfish  en el directorio   

/usr/local/glassfish/

unzip /home/AdminInfo/glassfish-4.1.1.zip


5.1 crear el usuario  glaassfish  de sistema 

```bash
useradd -r -d /usr/local/glassfish/glassfish4 glassfish
```

5.4   se  cambiaron los permisos    al directprio   donde    esta  glassfish 


chown glassfish:glassfish -R /usr/local/glassfish/glassfish4


5.5 Se crea archivo servicio (/etc/init.d/glassfish4)

```bash
#!/bin/bash
#
# Glassfish     This shell script takes care of starting and stopping
#               the Glassfish 4.1.1 Server
#
# chkconfig: - 64 36
# description:     Glassfish 4.1.1 server.
# processname: glassfish

### BEGIN INIT INFO
# Provides: glassfish
# Required-Start: $local_fs $remote_fs $network $named
# Required-Stop: $local_fs $remote_fs $network
# Short-Description: start an stop Glassfish Application Server
# Description: Glassfish Application Server J2EE
### END INIT INFO

# Source function library.
#. /etc/rc.d/init.d/functions

# Source networking configuration.
#. /etc/sysconfig/network

# Java Enviroment Variables
. /etc/profile.d/java.sh

#
prog="glassfish"

USER=glassfish
DIR=/usr/local/glassfish/glassfish4/bin
EXEC=$DIR/asadmin

DOMAIN=domain1

START="start-domain $DOMAIN"
STOP="stop-domain $DOMAIN"
RESTART="restart-domain $DOMAIN"
STATUS="list-applications"

start(){
     echo "Starting $prog:"
     su $USER -c "$EXEC $START"
     return $?
}

stop(){
     echo "Stop $prog:"
     su $USER -c "$EXEC $STOP"
     return $?
}

restart(){
     echo $"Restarting $prog:"
     su $USER -c "$EXEC $RESTART"
     return $?
}

as_status() {
    echo $"Status $prog:"
    su $USER -c "$EXEC $STATUS"
    if [ $? = 0 ]; then
         return 0
    else
         return 3
    fi
}

# See how we were called.
case "$1" in
  start)
    start
    ;;
  stop)
    stop
    ;;
  status)
    as_status
    ;;
  restart)
    restart
    ;;
  *)
    echo $"Usage: $0 {start|stop|status|restart}"
    exit 2
esac
```

5.6 Se activa el servicio

```bash
chkconfig glassfish4 on                                      
chkconfig glassfish4 on --level 35

```

5.7 Se modifica archivo de configuración de glassfish para indicar el path de JAVA asenv.conf

```bash
AS_JAVA="/usr/java/current"

```

5.8 Se modifica el límite de archivos que puede crear el usuario glassfish


```bash
/etc/apache2/sites-available

```



5.8.1 Se agrega un nuevo límite en (/etc/security/limits.conf)


```bash

glassfish soft nofile 32768
glassfish hard nofile 65536

```
5.8.2 Se habilita el modulo para el comando su en el archivo (/etc/pam.d/su), se descomenta:

```bash

session required pam_limits.so

```

habilitar  consola  de  glassfish desde la ip externa

8.1 Se cambia master password entramos  ala ruta 

```bash

/usr/local/glassfish/glassfish4/glassfish/bin

./asadmin start-domain

```

asadmin change-master-password --savemasterpassword=true
asadmin start-domain




8.2 Se cambia password del dominio


```bash

/usr/local/glassfish/glassfish4/glassfish/bin

```

se  ejecuta el siguiente  comando 

```bash
./asadmin change-admin-password

```



user: admin
password:1nf0t3c.2016.
















































cambiamos los permisos  al jenkins


 chown -R jenkins:jenkins jenkins


9  reiniciamos  servicios con  root



se  edito el   archivo hosts  

 nano /etc/hosts


172.18.89.5 sonar.dads.infotec.mx
172.18.89.5 gitlab.dads.infotec.mx
172.18.89.5 artifactory.dads.infotec.mx




./asadmin start-domain
