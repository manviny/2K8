En este caso   
Dominio: manol.com   
Nombre servidor TM-DC1-2K8  
# Tareas de configuración inicial
![](https://s3-eu-west-1.amazonaws.com/mnybackups/2k8/16-17.JPG)


# 1.- Proporcionar información del equipo
####Establecer zona horaria
![](https://s3-eu-west-1.amazonaws.com/mnybackups/2k8/Captura+de+pantalla+2015-01-29+a+las+9.00.45.png)
####Configurar funciones de red 
Debemos poner IP's estáticas  
**IP**   
192.168.1.2  
255.255.255.0   
192.168.1.1  
**DNS**  
192.168.1.2  # El mismo que nuestra IP estática, ya que disponemos de servidor de DNS's   
(si no usamos IPV6, desconectarlo)  
![](https://s3-eu-west-1.amazonaws.com/mnybackups/2k8/Captura+de+pantalla+2015-01-29+a+las+8.59.50.png)

####Proporcionar nombre del equipo y dominio.
Cambiar el nombre del servidor  
TM-DC1-2K8, (el dominio no podemos cambiarlo todavía)  
restart  

#4.- Habilitar comentarios y actulizacions automáticas.
Actualizaciones automaticas, preguntar antes si quiero descargarlas e instalarlas e incluir actualizaciones recomendadas.   
Preguntarme antes de enviar errores a microsoft.   

#5.- Descargar e instalar actualizaciones.
restart  

#6.- Habilitar escritorio remoto.
Permitir solo equipos a nivel de red  

Ahora podemos hacer check para que no muestre esta ventana al inicio, pie de ventana.  

#7.- Instalar Directorio Activo
Pulsar start y escribir en la caja de texto inferior dcpromo, esperar hasta que aparezca en la parte superior, entonces hacer clic.  
En la ventana que aparece clic en siguiente, no es necesario opciones avanzadas.  
Siguiente en compatibilidad de sistema operativo.  
Crear dominio nuevo en nuevo bosque, y dar un nombre unico en internet (midomino.com), manol.com  
Si no usamos un servidor antiguo en la red, seleccionamos Windows 2008  
Servidor DNS, es necesario (ip dinamicas si nos lo pregunta)  
Ubicacion de la base de datos -> siguiente  
Escribir contraseña en caso de que el servidor caiga podemos hacer una restauración, si exportamos configuracion nos puede servir para crear otros dominios.  

#8.- Revisar que todo esta instalado.
En la ventana del Administrador de Servidor ir a:  
Funciones > Servicios de dominio de Active Directory > Usuarios y equipos de Active Directory > midominio.com  
nuestro dominio debe existir y en su interior entre otras veremos carpetas de computers y usuarios.

En Funciones > Servicios de dominio de Active Directory > Sitios y Servicios de Active Directory  
Renombramos Default-First-Site-Name A TAME  

#9.- Conectar por control remoto W7 -> 2K8  
En W7 inicio > equipo, boton derecho y cambiar nombre a CLI-TM-W7 y reiniciar  
En W7 inicio > equipo, boton derecho y poner nombre del dominio, manol.com.  
**PROBLEMAS:**    
si nos da error en servicio DNS preferido ponemos la direccion del servidor 192.168.1.2.  
Si aún así no funcionase reiniciamos el servidor.  
Si sigue sin funcionar y estamos emulando los SO en vmware o virtualbox, poner modo bridge o modo puente en ambas maquinas.  


#10.- Conectarse al dominio
AL reinicar la maquina intentara conectarse a tu ordenador local antecedido con el nuevo nombre de la maquina:   
CLI-TM-2K8\manol  
Debemos cambiar de usuario para entrar en el dominio  
y entramos como MANOL\Administrador (manol.com)  

#11.- conectarse al servidor de forma remota  
inicio > escribimos remoto y hacemos clic en escritorio remoto y en opciones:   
En la pestaña general decimos que nos conectamos al equipo: TM-DC1-2K8.manol.com y usuario:Administrador  
Guardar conexión en el escritorio para  tenerla siempre a mano  

#12.- Crear unidad organizativa
En Funciones > Servicios de dominio de Active Directory > Sitios y Servicios de Active Directory, boton derecho 
nuevo: Unidad Organizativa (tameOU), debajo de esta crear dos OU:TmUsers y TmComputers  

#13.- Crear usuarios
Ir a la OU adecuada boton derecho y crear usuario   
