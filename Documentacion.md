# Tarea - Servicio de transferencia de ficheros

### ProFTP

<http:www.proftpd.org/>

### Infraestructura

> Reutilizamos las MV de la práctica de **ssh**. Dos MV de una **red NAT**:

> - Servidor: con un Ubuntu server sin entorno gráfico. - Usuario: **sergio**, contraseña: **sergio**.
> - Casa: con un Lubuntu con el entorno gráfico por defecto (LXQt). - Usuario: **carmen**, contraseña: **carmen**.

> Desde el equipi **Casa** nos conectaremos al equipo **Servidor** mediante una conexión **ssh** autentificándonos mediante claves asimétricas **ed25519**.


### Instalación y uso básico

> 1. Acceder al servidor:

~~~
ssh -i ~/.ssh/id_ed25519 10.0.2.4
~~~

> Casi toda la instalación y configuración la debemos hacer con privilegios de administrador podemos ejecutar **sudo** en todas las instrucciones o cambiar al usuario administrador **sudo su**.

1. Accedemos al servidor.

![Acceder al servidor](./img/Imagen_01.PNG)

2. Introducimos la contraseña del Servidor.

![Introducir contraseña del Servidor](./img/Imagen_02.PNG)

3. Hemos accedido al Servidor.

![Hemos accedido al servidor](./img/Imagen_03.PNG)

> 2. Instalar porftpd:
~~~
sudo apt update
sudo apt install proftpd
~~~

- Actualizar Servidor

1. Escribimos el comando sudo apt update.

![Actualizar](./img/Imagen_04.PNG)

2. Introducimos la contraseña del Servidor.

![Escribir contraseña del servidor](./img/Imagen_05.PNG)

3. El servidor está actualizado.

![Actualizado servidor](./img/Imagen_06.PNG)

- Instalar proftpd

1. Instalamos el proftpd.

![Instalar proftpd](./img/Imagen_07.PNG)

2. Confirmamos la instalación.

![Confirmar instalación](./img/Imagen_08.PNG)

3. Tenemos el proftpd instalado.

![Proftpd instalado](./img/Imagen_09.PNG)

> 3. Realizar algunos cambios en el archivo de configuración:
~~~
sudo nano /etc/proftpd/proftpd.conf
~~~

> - Cambiar el nombre del servidor. 
> - Desactivar el protocolo IP versión 6.
> - No mostrar el mensaje de bienvenida hasta que el usuario no se haya autenficado correctamente.

> Puedes obtener más información sobre las distintas directivas de configuración en: <http://www.proftpd.org/docs/directives/configuration_full.html>. Revisa las directivas: **DeferWelcome**, **DisplayConnect**, **DisplayLogin**, **DisplayChdir**, **DisplayGoAway**, **DisplayQuit**, **AccessGrantMsg** y **AccessDenyMsg**.

- No mostrar el mensaje de bienvienida (DeferWelcome)

1. Escribimos el comando sudo nano /etc/proftpd/proftpd.conf.

![Escribir comando](./img/Imagen_10.PNG)

2. Entramos al archivo de configuración.

![Archivo de configuración](./img/Imagen_11.PNG)

3. Cambiamos el DeferWelcome off a on.

![Cambiar DeferWelcome](./img/Imagen_12.png)

![DeferWelcome cambiado](./img/Imagen_13.png)

- Desactivar el protocolo IP versión 6.

1. Cambiamos el on a off.

![Cambiar UserIPv6](./img/Imagen_14.PNG)

![UserIPv6 cambiado](./img/Imagen_15.PNG)

- Cambiar el nombre del servidor.

1. Cambiamos el nombre del servidor.

![Cambiar nombre del servidor](./img/Imagen_16.PNG)

![Nombre del servidor cambiado](./img/Imagen_17.PNG)


> 4. Comprobar estado del servicio **proftpd**:
~~~
sudo systemctl status proftpd
~~~

1. Escribimos el siguiente comando: sudo systemctl status proftpd

![Introducir comando](./img/Imagen_18.PNG)

2. Vemos el estado del servicio.

![Ver estado del servicio](./img/Imagen_19.PNG)

> 5. Con los siguientes comandos lo activaremos para que se inicie al arrancar el servidor y lo iniciaremos:
~~~
sudo systemctl enable proftpd
sudo systemctl start proftpd
~~~

> Otros comandos del servicio son:
~~~
sudo systemctl enable proftpd
sudo systemctl start proftpd
sudo systemctl stop proftpd
sudo systemctl restart proftpd
sudo systemctl status proftpd
sudo systemctl reload proftpd
sudo systemctl show proftpd
~~~

1. Activamos el servicio.

![Activar el servicio](./img/Imagen_20.PNG)

![Servidor activado](./img/Imagen_21.PNG)

2. Iniciamos el servicio.

![Iniciar el servicio](./img/Imagen_22.PNG)

> 6. Reglas firewall:
~~~
sudo ufw enable
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
sudo ufw status
~~~

- Activar Firewall

![Activar el firewall](./img/Imagen_23.PNG)

![Confirmar activación del firewall](./img/Imagen_24.PNG)

![Firewall activado](./img/Imagen_25.PNG)


- Permitir puerto 20/tcp

![Puerto 20/tcp permitido](./img/Imagen_26.PNG)

- Permitir puerto 21/tcp

![Escribir comando para permitir puerto 21/tcp](./img/Imagen_27.PNG)

- Ver estado del Firewall

![Escribir comando estado Firewall](./img/Imagen_28.PNG)

![Ver estado del Firewall](./img/Imagen_29.PNG)

> 7. Probar desde el cliente qué puertos tiene abiertos del servidor, en nuestro ejemplo desde el equipo **Casa** ejecutaremos:
~~~
nmap 10.0.2.4 -p 1-1024
~~~

> Si no tienes instalada esta utilidad, instalalá con: **sudo apt install nmap**. Esta comprobación también se puede hacer desde el propio servidor, pero en menos fiable que desde otro equipo ya que puede conectarse por localhost.

1. Instalamos nmap.

![Instalar nmap](./img/Imagen_30.PNG)

2. Confirmamos la instalación.

![Confirmar instalación](./img/Imagen_31.PNG)

![Nmap instalado](./img/Imagen_32.PNG)

3. Escribimos el comando nmap 10.0.2.4 -p 1-1024.

![Escribir comando nmap](./img/Imagen_33.PNG)

4. Vemos los puertos abiertos.

![Ver puertos abiertos](./img/Imagen_34.PNG)

> 8. Conexión FTP desde el cliente, en nuestro ejemplo el equipo **Casa**. (-A: forzar modo activo):
~~~
ftp -A 10.0.2.4
~~~

> Para evitar problemas con el Firewall en todo momento utilizaremos el modo de transferencia 'activo'. Este modo utiliza unicamente los puertos 20 y 21 del servidor.

1. Introducimos el comando ftp -A 10.0.2.4.

![Introducir comando para conexión FTP](./img/Imagen_35.PNG)

2. Escribimos el nombre de usuario del servidor.

![Escribir nombre de usuario del servidor](./img/Imagen_36.PNG)

3. Escribimos la contraseña del usuario.

![Escribir contraseña del usuario del servidor](./img/Imagen_37.PNG)

4. La conexión se establece.

![Conexión FTP establecida](./img/Imagen_38.PNG)

> 9. Comprobar en el equipo **Servidor** qué conexiones están establecidas con otros equipos:
~~~
ss |grep tcp
~~~

![Introducir comando para ver conexiones establecidas](./img/Imagen_39.PNG)

![Ver conexiones establecidas](./img/Imagen_40.PNG)

> 10. Instalar el cliente gráfico **Filezilla** en el equipo **Casa**. Crea una nueva conexión en el **Gestión de sitios**. Conectarse al equipo **Servidor** con el protocolo FTP y cifrado FTP plano. Recuerda conectar el modo **activo** en la pestaña **Opciones de transferencia** del Gestor de sitios.
~~~
sudo apt install filezilla
~~~

1. Instalamos Filezilla.

![Introducir comando para instalar firezilla](./img/Imagen_41.PNG)

2. Escribimos la contraseña y confirmamos la instalación.

![Introducir contraseña de Casa y confirmar instalación](./img/Imagen_42.PNG)

![Firezilla instalado](./img/Imagen_43.PNG)

3. Abrimos Firezilla.

![Abrir Firezilla](./img/Imagen_44.PNG)

4. Seleccionamos 'Archivos' y 'Gestor de sitios'.

![Seleccionar 'Archivos' y 'Gestor de sitios'](./img/Imagen_45.PNG)

5. Pulsamos el botón 'Nuevo sitio'.

![Pulsar botón 'Nuevo sitio'](./img/Imagen_46.PNG)

6. Seleccionamos el cifrado FTP plano y rellenamos la información.

![Seleccionar cifrado FTP plano y rellenar información](./img/Imagen_47.PNG)

7. Vamos a 'Opciones de Transferencia' y seleccionamos opción 'activo'

![Ir a 'Opciones de Transferencia' y seleccionar opción 'activo'](./img/Imagen_48.PNG)

8. Pulsamos el botón 'Conectar'.

![Pulsar botón 'Conectar'](./img/Imagen_49.PNG)

9. Pulsamos OK.

![Pulsar OK](./img/Imagen_50.PNG)

10. La conexión está establecida.

![Conexión establecida](./img/Imagen_51.PNG)


### Configurar una cuenta anónima

> 1. Volveremos a realizar algunos cambios en el archivo de configuración:
~~~
sudo nano /etc/proftpd/proftpd.conf
~~~
> Descomentamos todo el bloque **<Anonymous ~ftp>** de modo que quede de la siguiente forma:

~~~
<Anonymous ~ftp>
User ftp
Group nogroup
# We want clients to be able to login with "anonymous" as well as
"ftp"
UserAlias anonymous ftp
# Cosmetic changes, all files belongs to ftp user
DirFakeUser on ftp
DirFakeGroup on ftp

RequireValidShell off

# Limit the maximum number of anonymous logins
MaxClients 10

# We want 'welcome.msg' displayed at login, and '.message' displayed
# in each newly chdired directory.
DisplayLogin welcome.msg
DisplayChdir .message

# Limit WRITE everywhere in the anonymous chroot
<Directory *>
 <Limit WRITE>
   DenyAll
 </Limit>
</Directory>

# Uncomment this if you're brave.
# <Directory incoming>
#   # Umask 022 is a good standard umask to prevent new files and>
#   # (second parm) from being group and world writable.
#   Umask022 022
#   <Limit READ WRITE>
#     DenyAll
#     </Limit>
#       <Limit STOR>
#         AllowAll
#     </Limit>
# </Directory>

</Anonymous>

~~~

1. Escribimos el comando sudo nano /etc/proftpd/proftpd.conf.

![Escribir comando](./img/Imagen_52.PNG)

2. Introducimos la contraseña del servidor.

![Escribir contraseña del servidor](./img/Imagen_53.PNG)

3. Entramos al archivo de configuración.

![Archivo de configuración](./img/Imagen_54.PNG)

4. Buscamos el bloque <Anonymous ~ftp>

![Buscar bloque <Anonymous ~ftp>](./img/Imagen_55.PNG)

5. Descomentamos el bloque.

![Descomentar bloque <Anonymous ~ftp>](./img/Imagen_56.PNG)

![Descomentar bloque <Anonymous ~ftp>](./img/Imagen_57.PNG)

6. Guardamos los cambios.

![Guardar cambios](./img/Imagen_58.PNG)

> 2. Reiniciaremos el servicio **proftpd**:
~~~
sudo systemctl restart proftpd
~~~

![Escribir comando para reiniciar servicio proftpd](./img/Imagen_59.PNG)

![Servicio proftpd reiniciado](./img/Imagen_60.PNG)

3. Puedes probar que los archivos de configuración son correctos con el siguiente comando:
~~~
sudo /usr/sbin/protfpd --configtest -c /etc/proftpd/proftpd.conf
~~~

![Escribir comando](./img/Imagen_61.PNG)

![Ver si los archivos de configuración son correctos](./img/Imagen_62.PNG)

4. Prueba que puedes acceder al servidor sin escribir contraseñas usando los dos usuarios anónimos: anonymous y ftp. Realiza la prueba tanto desde el terminal como desde la aplicación gráfica filezilla.

> Realmente al instalar ProFTP se crea el usuario **ftp**, anonymous es un alias de este usuario. Puedes verlo en la directiva **UserAlias** del bloque **<Anonymous >** del archivo de configuración. Además puedes ver que este usuario existen en el sistema en el archivo de usuarios: **sudo cat /etc/passwd**.

- Terminal

1. Accedemos al servidor FTP.

![Acceder al servidor FTP](./img/Imagen_63.PNG)

2. Ingresamos con 'Anonymous'.

![Ingresar con 'Anonymous'](./img/Imagen_64.PNG)

![Acceso correcto](./img/Imagen_65.PNG)

3. Volvemos a acceder al servidor FTP.

![Acceder al servidor FTP](./img/Imagen_63.PNG)

4. Ingresamos con 'ftp'.

![Ingresar con 'ftp'](./img/Imagen_66.PNG)

![Acceso correcto](./img/Imagen_67.PNG)


- Aplicación gráfica Filezilla.

1. Rellenamos la información con 'Anonymous'.

![Rellenar información con 'Anonymous'](./img/Imagen_68.PNG)

2. Pulsamos 'Conexión rápida'

![Pulsar 'Conexión rápida'](./img/Imagen_69.PNG)

3. Pulsamos 'OK'.

![Pulsar 'OK'](./img/Imagen_70.PNG)

4. La conexión está establecida.

![Conexión establecida](./img/Imagen_71.PNG)

5. Volvemos a rellenar la información pero con 'ftp'.

![Rellenar información con 'ftp'](./img/Imagen_68.PNG)

6. Pulsamos 'Conexión rápida'.

![Pulsar 'Conexión rápida'](./img/Imagen_72.PNG)

7. Pulsamos 'OK'.

![Pulsar 'OK'](./img/Imagen_73.PNG)

8. La conexión está establecida.

![Conexión establecida](./img/Imagen_74.PNG)

> 5. Si queremos que el usuario anónimo tenga una carpeta diferente, tenemos que crear dicha carpeta, por ejemplo:
~~~
sudo mkdir -p /var/ftp/anonimo
sudo chown ftp:nogroup /var/ftp/anonimo
~~~

1. Escribimos el comando sudo mkdir -p /var/ftp/anonimo.

![Escribir primer comando](./img/Imagen_75.PNG)

2. Introducimos la contraseña del servidor.

![Introducir contraseña del servidor](./img/Imagen_76.PNG)

![Carpeta creada](./img/Imagen_77.PNG)

3. Escribimos el comando sudo chown ftp:nogroup /var/ftp/anonimo.

![Escribir segundo comando](./img/Imagen_78.PNG)

![Propietario del archivo cambiado](./img/Imagen_79.PNG)

> 6. Después debes indicarlo en el fichero de configuración, cambiando la etiqueta **<Anonymous ~ftp>** por **<Anonymous /nombre-de-carpeta>**. Por ejemplo: **<Anonymous /var/ftp/anonimo>**.

1. Editamos el archivo de configuración.

![Editar archivo de configuración](./img/Imagen_80.PNG)

2. Cambiar etiqueta '<Anonymous>'.

![Cambiar etiqueta '<Anonymous>'](./img/Imagen_81.PNG)

3. Guardamos los cambios.

![Guardar cambios](./img/Imagen_82.PNG)

> 7. Para saber que estamos en esta nueva carpeta, le vamos a crear un fichero dentro:
~~~
sudo touch /var/ftp/anonimo/UsuarioAnonimo.txt
~~~
> Otra opción sería crear/modificar el archivo **welcome.msg** que nos indica la directiva **DisplayLogin**.

![Crear archivo](./img/Imagen_83.PNG)

> 8. Recuerda reiniciar el servicio **proftpd**:
~~~
sudo systemctl restart proftpd
~~~

![Escribir comando para reiniciar servicio proftpd](./img/Imagen_59.PNG)

![Servicio proftpd reiniciado](./img/Imagen_60.PNG)

> 9. Puedes probar que los archivos de configuración son correctos con el siguiente comando:
~~~
sudo /usr/sbin/proftpd --configtest -c /etc/proftpd/proftpd.conf
~~~

![Probar archivos de configuración](./img/Imagen_84.PNG)

> 10. Vuelve a probar a acceder al servidor usando los usuarios: anonymous y ftp. Tanto desde el terminal como desde la aplicación gráfica filezilla. Esta vez debe aparecer el nuevo directorio de trabajo.

- Terminal

![Acceder servidor con anonymous y mostrar archivo](./img/Imagen_85.PNG)

![Acceder servidor con ftp y mostrar archivo](./img/Imagen_86.PNG)

- Aplicación gráfica Firezilla

![Acceder servidor con anonymous y mostrar archivo](./img/Imagen_87.PNG)

![Acceder servidor con ftp y mostrar archivo](./img/Imagen_88.PNG)


### Configuración de usuarios virtuales

> Los usuarios virtuales son aquellos que no son usuarios de sistema pero si tienen acceso a algunos recursos a través del servicio FTP. Los crearemos usando el comando **ftpasswd**. Los usuarios y las contraseñas se almacenan en un fichero que vamos a crear en **/etc/proftpd/ftpd.passwd**:

> 1. Primero vamos a crear las carpetas de trabajo que le vamos a asignar a cada usuario virtual:
~~~
sudo mkdir /var/ftp/ftp01
sudo mkdir /var/ftp/ftp02
sudo chown ftp:nogroup /var/ftp/ftp01 /var/ftp/ftp01
~~~

![Crear archivos y cambiar propietario del archivo](./img/Imagen_89.PNG)

> 2. Ahora nos situamos en el directorio de configuración de **proftpd** y crearmos un archivo vacio para generar los usuarios virtuales.
~~~
cd /etc/proftpd/
sudo touch ftpd.passwd
~~~

![Situarnos en /etc/proftpd/ y crear archivo](./img/Imagen_90.PNG)

> 3. Después creamos los usuarios virtuales ejecutando el comando **ftpasswd**:
~~~
sudo ftpasswd --passwd --name=ftp01 --uid=3001 --gid=3001 --
home=/var/ftp/ftp01 --shell=/bin/false
$ sudo ftpasswd --passwd --name=ftp02 --uid=3002 --gid=3002 --home=/var/ftp/ftp02 --shell=/bin/false
~~~
> Nos pedirá que introduzcamos la contraseña de los usuarios virtuales.

![Crear primer usuario virtual](./img/Imagen_91.PNG)

![Crear segundo usuario virtual](./img/Imagen_92.PNG)

> 4. Para saber que estamos en nuestra carpeta, le vamos a crear un fichero dentro de cada una:
~~~
sudo touch /var/ftp/ftp01/soyusuario1.txt
sudo touch /var/ftp/ftp02/soyusuario2.txt
~~~

> Otra opción sería crear en dicha carpeta un archivo **message** tal como se indica en la directiva **DisplayChdir** del archivo de configuración.

![Crear fichero en cada carpeta](./img/Imagen_93.PNG)

> 5. A continuación necesitamos modificar el archivo de configuración:
~~~
sudo nano /etc/proftpd/proftpd.conf
~~~

> Descomentamos las líneas:
~~~
DefaultRoot ~
RequireValidShell off
~~~

> Revisad que haya un espacio entre la directiva y el valor. Si no os dará un error.

> Y al final del archivo incluimos la siguiente directiva:
~~~
AuthUserFile /etc/proftpd/ftpd.passwd
~~~

1. Editamos el archivo de configuración.

![Editar archivo de configuración](./img/Imagen_80.PNG)

2. Descomentamos el código.

![Descomentar código](./img/Imagen_94.PNG)

![Código modificado](./img/Imagen_95.PNG)

3. Incluimos el código AuthUserFile /etc/proftpd/ftpd.passwd.

![Incluir código](./img/Imagen_96.PNG)

> 6. Guardamos y reiniciamos el servicio **proftpd**:
~~~
sudo systemctl restart proftpd
~~~

1. Guardamos los cambios.

![Guardar cambios](./img/Imagen_97.PNG)

2. Reiniciamos el servicio.

![Reiniciar servicio proftpd](./img/Imagen_60.PNG)

> 7. Puedes probar que los archivos de configuración son correctos con el siguiente comando:
~~~
sudo /usr/sbin/protfpd --configtest -c /etc/proftpd/proftpd.conf
~~~

![Reiniciar servicio y comprobar archivos de configuración](./img/Imagen_98.PNG)

> 8. Vuelve a probar a acceder al servidor usando los usuarios virtuales, tanto desde el terminal como desde la aplicación gráfica filezilla. Debe aparecer el archivo correspondiente de su directorio de trabajo.

En este paso, no he podido acceder al servidor usando los usuarios virtuales, tanto desde el terminal como desde la
aplicación gráfica filezilla.


### Configuración de FTPS. FTP con TLS/SSL

> 1. Para utilizar una conexión segura primero debemos instalar en el servidor el paquete para hacer funcionar el módulo de criptografía en proftpd.
~~~
sudo apt install proftpd-mod-cryto
~~~

![Escribir comando](./img/Imagen_99.PNG)

![Paquete instalado](./img/Imagen_100.PNG)


> 2. Ahora debemos activar dicho paquete en el archivo de configuración: **/etc/proftpd/modules.conf**. Descomentar la directiva: **LoadModule mod_tls.c**.

1. Escribimos el siguiente comando:

![Escribir comando](./img/Imagen_101.PNG)

2. Descomentamos el código.

![Descomentar código](./img/Imagen_102.png)

![Código descomentado](./img/Imagen_103.png)

3. Guardamos los cambios.

![Guardar cambios](./img/Imagen_104.PNG)


> 3. A continuación debemos instalar el paquete openssl para generar un certificado autofirmado.
~~~
sudo apt install openssl
~~~

![Instalar paquete openssl](./img/Imagen_105.PNG)


> 4. Una vez instalado vamos a generar una **clave RSA** de **2048 bit**. Guardaremos la clave privada en el directorio **/etc/ssl/private/proftpd.key** y la clave pública en **/etc/ssl/certs/proftpd.crt**. Le daremos una caducidad de 365 días.
~~~
openssl req -x509 -newkey rsa:2048 -sha256 -keyout /etc/ssl/private/proftpd.key -out /etc/ssl/certs/proftpd.crt -nodes -days 365
~~~

> Al crear el certificado nos pedirá cierta información que tendremos que ir rellenando.

> Ponemos los permisos adecuados a estos certificados:
~~~
chmod 600 /etc/ssl/private/proftpd.key
chmod 600 /etc/ssl/certs/proftpd.crt
~~~

1. Generamos una clave RSA.

![Generar clave RSA](./img/Imagen_106.PNG)

2. Rellenamos la información.

![Rellenar información](./img/Imagen_107.PNG)

3. Ponemos los permisos.

![Poner permisos](./img/Imagen_108.PNG)


> 5. A continuación debemos incluir el archivo **tls.conf** en el archivo de configuración **proftpd.conf**. Editamos el archivo de configuración: **/etc/proftpd/proftpd.conf**. Descomentar la línea: **Include /etc/proftpd/tls.conf**.

1. Editamos el archivo de configuración.

![Editar archivo de configuración](./img/Imagen_52.PNG)

2. Descomentamos el código.

![Descomentar código](./img/Imagen_109.png)

![Código descomentado](./img/Imagen_110.png)

3. Guardamos los cambios.

![Guardar cambios](./img/Imagen_111.PNG)


> 6. Ahora abrimos el archivo de configuración de TLS **/etc/proftpd/tls.conf** y descomentamos las siguientes líneas:
~~~
TLSEngine                on
TLSLog                   /var/log/proftpd/tls.log
TLSProtocol              SSLv23
...
TLSRSACertificateFile    /etc/ssl/certs/proftpd.crt
TLSRSACertificateKeyFile /etc/ssl/private/proftpd.key
...
TLSRequired              on
~~~

> Con la directiva **TLSRequired on** establecemos que toda la comunicación con el servidor debe ser segura.

1. Editamos el archivo de configuración de TLS.

![Editar archivo de configuración de TLS](./img/Imagen_112.PNG)

2. Descomentamos los códigos.

![Descomentar código](./img/Imagen_113.png)

![Código descomentado](./img/Imagen_114.png)

![Descomentar código](./img/Imagen_115.png)

![Código descomentado](./img/Imagen_116.png)

![Descomentar código](./img/Imagen_117.png)

![Código descomentado](./img/Imagen_118.png)

3. Guardamos los cambios.

![Guardar cambios](./img/Imagen_119.PNG)


> 7. Reiniciamos el servicio **proftpd**:
~~~
sudo systemctl restart proftpd
~~~

![Reiniciar servicio proftpd](./img/Imagen_60.PNG)


> 8. Puedes probar que los archivos de configuración son correctos con el siguiente comando:
~~~
sudo /usr/sbin/proftpd --configtest -c /etc/proftpd/proftpd.conf
~~~

![Probar archivos de configuración](./img/Imagen_120.PNG)


> 9. Puedes probar que se establece la comunicación TLS entre el cliente **Casa** y el **Servidor** ejecutando el siguiente comando desde un terminal del equipo **Casa**:
~~~
openssl s_client -connect 10.0.2.4:21 -starttls ftp
~~~

![Escribir comando](./img/Imagen_121.PNG)

![Probar comunicación TLS](./img/Imagen_122.PNG)


> 10. Vuelve a probar a acceder al servidor usando la conexión segura. Primero con la aplicación gráfica filezilla. Crea una nueva conexión en el gestor de sitios con el protocolo FTP y el cifrado 'Requiere FTP explícito sobre TLS'. Recuerda activar el modo de transferencia activa en la pestaña 'Opciones de transferencia'.

1. Pulsamos 'Nuevo Sitio'.

![Pulsar 'Nuevo Sitio'](./img/Imagen_123.png)

2. Rellenamos la información.

![Rellenar información](./img/Imagen_124.PNG)

3. Pulsamos 'Conexión rápida'.

![Pulsar 'Conexión rápida'](./img/Imagen_72.PNG)

![Error](./img/Imagen_125.PNG)

Al intentar a acceder al servidor me da error.

> 11. Para probar con el terminal debemos instalarnos un nuevo paquete ya que el comando básico **ftp** no permite certificados. Instala en el equipo **Casa** el paquete **lftp**:
~~~
sudo apt install `lftp`
~~~

![Instalar lftp](./img/Imagen_126.PNG)


> 12. Ahora configura el comando **lftp** creando un archivo de configuración **nano ~/.lftprc** con el siguiente contenido:
~~~
set ftp:passive-mode off
set ftp:ssl-auth TLS
set ftp:ssl-force true
set ftp:ssl-protect-list yes
set ftp:ssl-protect-data yes
set ftp:ssl-protect-fxp yes
set ssl:verify-certificate no
~~~

> Configuramos: el modo transferencia en activo, forzamos el uso de ssl y la autentificación con TLS. Además como nuestro certificado en autofirmado establecemos que no se verifique el certificado.

1. Creamos el archivo.

![Crear archivo](./img/Imagen_127.PNG)

2. Escribimos los comandos.

![Escribir comandos](./img/Imagen_128.PNG)

3. Guardamos los cambios.

![Guardar cambios](./img/Imagen_129.PNG)


> 13. Prueba a acceder desde el terminal con:
~~~
lftp jose@10.0.2.4
~~~

![Acceder al servidor](./img/Imagen_130.PNG)

