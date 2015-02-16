
![server](https://s3-eu-west-1.amazonaws.com/mny-fotos/webs/2K8.png)    

[Introduccion al Active Directory - qué es (51 min)](https://www.youtube.com/watch?v=ku-vfRTdmnU)   
[Windows Server 2008 - Instalación de Active Directory](https://www.youtube.com/watch?v=pD6l5iR9rDU)  
[Crear un dominio en server 2008 y acceso remoto](https://www.youtube.com/watch?v=F9pvhKNRGLA)   
[Active Directory - Qué es y para que sirve](https://www.youtube.com/watch?v=nxo98Dyw-2U)   
[Como crear usuarios y grupos - Windows Server 2008](https://www.youtube.com/watch?v=CT9iqnBmufI])   




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

#9.- Preparar W7 para conectarnos al dominio.   
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

  
Las **OU** sirven para organizar usuarios y ordenadores y de esta manera definir **lo que pueden y no pueden hacer** con sus usuarios, cambiar fondo escritorio, ocultar barra tareas, instalar software, etc.   
**Grupos** son objetos del directorio activo que **proveen o niegan acceso a recursos** como impresoras y carpetas de los diferentes departamentos, estos viven dentro de las OUs  


1. En Funciones > Servicios de dominio de Active Directory > Usuarios y equipos de Active Directory, boton derecho 
nuevo:  
Unidad Organizativa (tameOU), debajo de esta crear dos OU:TmUsers y TmComputers  

#13.- Crear y mover usuarios y ordenadores a sus UOs.

*Ir a la OU TmUsers boton derecho y crear un usuaior por cada alumno de la clase, estos podrán entrar entrar en el dominio desde windows cliente, poniendo el nombre del dominio en mayuscula\nombreusuario, por ejemplo si el dominio fuera manol.com, entrariamos mediante MANOL\usuario.
  
*En la OU TmComputers añadir nuestra máquina CLI-TM-W7, para ello lo arratraremos desde nuestro dominio > computers hasta TmComputers.  

En TmUsers crear 4 grupos, ManagerVentas, UsuarioVentas, ManagerProduccion, UsuarioProduccion.  
En TmComputers 2 grupos, ItComputers, StandardComputers  
Añadir usuarios y ordenadores a grupos haciendo clic derecho y Agregar a un grupo, escribiendo el grupo donde moverlo.   
Para comprobar si el grupo contiene a los miembros correctos, hacer clic y ver miembros en propiedades




#SOLUCIONES
###1. ¿Todas las versiones de Windows pueden trabajar en varias plataformas hardware?   
Solución:   
Solamente las versiones de Windows de la familia NT. En particular desde la versión NT
3.5, estos SO pueden trabajar en plataformas Intel, AMD, Alpha, Mips o Power-Pc, entre
otras, y en plataformas de 32 y 64 bits la mayoría de ellas.
###2. ¿Cuál es la característica que determina la portabilidad de estos SO?    
Solución:   
La abstracción del hardware, es decir, el SO hace a través de lo que se ha denominado
Capa de Abstracción de Hardware (HAL, Hardware Abstraction Layer), que la arquitectura
del hardware utilizada sea transparente al SO.
###3. ¿Se puede iniciar Windows Server en modo seguro?
Solución:  
Sí. Se hace igual que en las versiones de Windows de sobremesa, pulsando la tecla F8,
durante el proceso de arranque. De esta forma, solamente se cargarán los controladores y
servicios indispensables para que el sistema arranque. También se puede iniciar el equipo
en modo seguro con funciones de red mínimas.
###4. Si suspendemos el equipo y se corta el suministro de corriente eléctrica, ¿qué ocurre
en el sistema?
Solución:  
Las aplicaciones se cerrarán y, por lo tanto, todos los datos que no tuviéramos guardados,
se perderán.
###5. ¿Es lo mismo un dominio que el Directorio Activo o Active Directory?
Solución:  
No. El concepto de dominio se utiliza para la gestión de redes de gran tamaño,
independientemente del SO con el que se implemente el servidor que gestiona el dominio.
El Directorio Activo es la implementación de controladores de dominio exclusivamente
diseñado para equipos y redes con Windows Server.
###6. ¿Un grupo se considera unidad organizativa?
Solución:  
Sí. Siempre y cuando nos refiramos a un grupo de usuarios. En general, una Unidad
Organizativa (UO), es la Unidad jerárquica inferior al dominio, que puede estar compuesta
por una serie de objetos y/o por otras UO. En concreto, un grupo puede ser una UO ya que
se supone que contendrá usuarios.
###7. Pon un ejemplo de un nombre completo de dominio.
Solución:  
Por ejemplo, mcgrawhill.com. Siendo mcgrawhill el nombre del dominio y com el sufijo del
mismo. Este nombre es un nombre DNS. En la actualidad, la estructura de dominios de
Windows Server se basa en un sistema jerárquico de nombres, ampliamente aceptado,
probado, normalizado: el espacio de nombres de DNS. 
###8. ¿Es lo mismo un bosque que un árbol?
Solución:  
No. Un árbol es un conjunto de uno o más dominios que comparten un espacio de
nombres contiguo. Si existe más de un dominio, estos se disponen en estructuras de árbol
jerárquicas.  
El primer dominio creado es el dominio raíz del primer árbol. Cuando se agrega un dominio
a un árbol existente, este pasa a ser un dominio secundario (o hijo). Un dominio
inmediatamente por encima de otro dominio, en el mismo árbol de dominio, es su padre.
Todos los dominios que tengan un dominio raíz común se dice que forman un espacio de
nombres contiguo.  
Los dominios secundarios (hijos) pueden representar entidades geográficas (Valencia,
Madrid, Barcelona), entidades administrativas dentro de la organización (departamento de
ventas, departamento de desarrollo, etc.), u otras delimitaciones específicas de una
organización, según sus necesidades.  
En cambio, un bosque es un grupo de árboles que no comparten un espacio de nombres
contiguo, conectados a través de relaciones de confianza bidireccionales y transitivas.
Un dominio único constituye un árbol de un dominio, y un árbol único constituye un bosque
de un árbol. Es importante destacar que, aunque los diferentes árboles de un bosque no
comparten un espacio de nombres contiguo, el bosque tiene siempre un único dominio raíz,
llamado precisamente dominio raíz del bosque; dicho dominio raíz será siempre el primer
dominio creado por la organización.
###9. ¿Cuándo se crea el dominio raíz en una organización?
Solución:  
Cuando se instala el Directorio Activo en el primer equipo de la red. En este caso, se crea
no solamente la raíz sino el bosque del dominio.
###10. ¿Quién contiene la configuración del bosque?
Solución:  
La contiene el primer controlador de dominio en el que hemos instalado el Directorio Activo,
y que posteriormente se replicará al resto de controladores de la red que forman el dominio.
###11. Cuando agregamos nuevos dominios, ¿qué tipo de elemento añadimos a la
infraestructura de DA?
Solución:  
Podemos añadir nuevos bosques o nuevos árboles. Dentro de ellos podemos añadir
nuevos controladores de dominio principales, formando parte del bosque existente, o
controladores de reserva adicionales a los controladores principales existentes.
###12. Para crear una nueva estructura de dominio, ¿qué pasos tenemos que seguir?
Solución:  
Lo primero es crear un dominio nuevo en un bosque nuevo. Es lógico, ya que de momento
no disponemos de nada. Para ello, creamos la estructura del dominio raíz, el árbol al que
pertenecerá el dominio y el bosque en el que se integrará.
###13. ¿Se puede instalar un controlador de dominio adicional a un controlador de dominio
principal?
Solución:  
Sí. De hecho, más que poder, se debe instalar un controlador de reserva en cada dominio,
para descargar al controlador principal de trabajo y, sobre todo, por si este deja de
funcionar.
###14. ¿Y un controlador de dominio adicional para un bosque?
Solución:  
Sí. Por los mismos motivos que en la actividad anterior.
###15. ¿Qué tipos de objetos se manejan normalmente en DA?
Solución:  
Los objetos que pueden manejarse en un dominio Windows Server son:
A) Usuarios globales. Cuando tenemos instalado un controlador de dominio con Windows
Server, se crean en el sistema cuentas de usuario. Las cuentas de usuario globales
representan una cuenta única de usuario que se puede utilizar desde cualquier ordenador
integrado en el dominio. En este caso, cuando una persona se conecta a cualquiera de
dichos ordenadores, utilizando para ello su cuenta de usuario global, el ordenador en
cuestión realiza una consulta al Directorio Activo. Este valida las credenciales del usuario y
el resultado de la validación es enviado al ordenador cliente, concediendo o rechazando la
conexión.  
B) Grupos. De forma similar a los usuarios globales, existen grupos que son almacenados
en el Directorio Activo y que por lo tanto son visibles desde todos los ordenadores del
dominio. En el controlador de dominio pueden crearse diferentes tipos de grupos de
usuarios: grupos de distribución y grupos de seguridad.  
C) Equipos. La misma base de datos del Directorio Activo de un dominio que conserva
toda la información relativa a cuentas de usuarios y grupos globales, almacena asimismo
información relativa a los equipos que forman parte del dominio. Se almacena el nombre
del ordenador, así como un identificador único y privado que lo identifica unívocamente y
que solamente conoce el controlador de dominio. Gracias a esta identificación univoca, a
cada equipo (objeto en general) se le pueden asignar permisos y derechos.  
D) Unidades Organizativas. Las unidades organizativas son objetos del directorio que, a
su vez, pueden contener otros objetos. El uso fundamental de las UO es delegar la
administración de sus objetos a otros usuarios distintos del administrador del dominio, y
personalizar el comportamiento de los usuarios y/o equipos mediante la aplicación de
directivas. 
###16. ¿De qué forma se reconoce cada objeto en el sistema?
Solución:  
Objetos como usuarios, grupos y equipos quedan identificados, cada uno de ellos, por un
número o identificación en el equipo.
###17. ¿Qué tipos de usuarios podemos administrar desde la herramienta Administración
de equipos?
Solución:  
Con esta herramienta, administraremos y configuraremos opciones básicas del controlador
de dominio. Podemos administrar usuarios locales, grupos locales, espacio de
almacenamiento en disco, recursos compartidos, etc
###18. ¿Es necesario configurar el servicio DNS para dar de alta un controlador de dominio
en Windows Server?
Solución:  
Sí. Si no se configura este servicio, los clientes y el controlador de dominio no podrán
comunicarse entre ellos. Tampoco funcionarían las relaciones de confianza a establecer
entre diferentes dominios de la red. El servicio DNS nos permite asociar nombres de equipo
o host con direcciones IP. Durante el proceso de instalación de DA se instala y configura
automáticamente este servicio.

    
       
       
###1. Indica al menos dos desventajas de la utilización de equipos en una red con o sin
servidor.
Solución:  
Una de ellas puede ser la gestión centralizada de los recursos de red, ya que en redes
sin servidor no se puede centralizar este tipo de gestión, mientras que en redes con
servidor, desde el equipo principal se pueden administrar los recursos compartidos de
forma centralizada.
Otra puede ser la flexibilidad de la red a la hora de ampliaciones o cambios. Una red sin
servidor es menos flexible, ya que cualquier modificación de la infraestructura
informática afecta a todos los equipos. En redes con servidor, los cambios solamente se
reflejarán en el servidor, tomando esta configuración el resto de equipos de la red.
###2. ¿Cuantos servicios necesita un SO como mínimo para ser servidor?
Solución:  
En principio no hay un número concreto de servicios que se tengan que ejecutar para
que un SO en red sea servidor. Lo que sí tiene que incorporar es la funcionalidad de
poder validar el logueo que los clientes realicen al servidor, pero como tal, los servicios
se instalarán para dar más o menos funcionalidad al servidor.
###3. Indica al menos dos SO en entorno cliente y dos en entorno servidor.
Solución:  
Como sistemas operativos clientes, podemos nombrar a Windows XP, Windows 7/8 o
Linux Desktop. Como sistemas operativos servidores, los más importantes son Windows
2008 R2/2012 Server y Linux Server.  
###4. ¿Cuantos equipos puedo integrar en una red local, con o sin servidor?
Solución:   
En principio, todos los que quiera. Dependerá del tipo de sistema operativo y cliente
utilizados, pero en la actualidad, si elegimos un rango de direcciones IP lo
suficientemente grande, podremos integrar los equipos que queramos. Si la red dispone
de servidor, tendremos en cuenta las licencias que hemos adquirido para que los
equipos clientes puedan loguearse.
###5. ¿Se puede instalar Windows 2008 R2 Server en cualquier tipo de hardware? Indica
los requerimientos mínimos de instalación para este sistema operativo.
Solución:   
No, aunque todas las versiones de Windows 2008 R2 Server funcionan o son instalables
en plataformas Alpha, PowerPc, Mips, Intel o Amd. Los requisitos de hardware para estos sistemas operativos son bastante exigentes. 
###6. ¿Qué sistema de archivos utilizaremos normalmente para instalar equipos en
entorno Windows Server?
Solución:   
NTFS. El sistema de archivos de NT (NTFS) ha sido desarrollado especialmente para
Windows Server. NTFS ofrece medidas de seguridad ampliadas para el acceso a
archivos y directorios. De forma similar a los sistemas de archivos UNIX, se pueden
definir derechos sobre archivos y directorios de forma individual para cada grupo de
usuarios o para usuarios particulares.
###7. Si utilizamos el sistema de archivos FAT en servidores Windows, ¿qué ventajas y
desventajas encontraremos respecto del sistema de archivos NTFS?
Solución:  
Sobre todo, la asignación de permisos de seguridad a recursos del servidor. Este
sistema detalla a través de una tabla (la FAT) en qué sectores del disco duro se
encuentra cada archivo o parte de un archivo. Para ello, FAT divide el disco duro en
bloques. El número de bloques es limitado, y todos los bloques de un disco duro deben
tener siempre el mismo tamaño. Se utilizan en equipos de tipo cliente, y normalmente no
se montan en servidores ya que no permiten gestionar privilegios y permisos de acceso
a recursos de red de forma segura.  
Si utilizamos sistemas FAT, no podremos instalar casi ninguna de las funciones que lo
convierten en SO Servidor de Red. De hecho, no es posible instalar 2008 R2 en
particiones FAT32.  
En particular los sistemas de archivos NTFS tienen características tales como:  
 Notificación de eliminación para dispositivos de estado sólido (SSD) que admiten T10
Trim.
 Nueva semántica de bloqueos oportunistas e introducción de claves de bloqueo
oportunista.
 Compatibilidad con la desfragmentación de metadatos del sistema de archivos.
 Mejoras en la reducción del volumen.
 Posibilidad de deshabilitar nombres cortos por volumen.
 Simultaneidad mejorada de las solicitudes de lectura durante el vaciado.
 Compatibilidad nativa con VHD.
 Mejoras en el rendimiento de Chkdsk.
 Mejora del rendimiento de Robocopy.
 Mejoras en la copia de archivos locales.
###8. ¿De que tipo son las particiones que pueden gestionar los SO de la familia Server
de Windows?
Solución:  
Como en la mayoría de los sistemas, las particiones que se pueden gestionar son
primarias y extendidas. En Windows Server, existe además otro concepto de gestión del
espacio de almacenamiento: El volumen como conjunto de particiones, discos, o
cualquier dispositivo físico de almacenamiento unificados en una misma unidad lógica.
Este concepto no lo hemos visto, ya que normalmente este tipo de espacios de
almacenamiento es gestionado por administradores de estos SO en instalaciones de red
avanzadas.  
###9. ¿Puedo instalar un SO en una partición que no está activa?
Solución:  
No. Por defecto, el instalador copia los archivos de sistema, de arranque y de
configuración en el disco que tenga la partición activa. Puede ser en el primer disco
duro, en el segundo o sucesivos, o en la partición primaria, o en una unidad lógica de la
partición extendida, pero la instalación se realiza siempre sobre la partición activa.
###10. ¿Se pueden redimensionar particiones durante el proceso de instalación en
Windows Server? Indica si es el caso, en que versión se puede realizar esta
operación.
Solución:  
Sí, pero solamente en versiones superiores a Windows 2008 Server. Al igual que ocurre
con los SO equivalentes en entorno cliente, como Windows Vista, en Windows 2008
Server se pueden redimensionar particiones durante el proceso de instalación. Sin
embargo en Windows 2003 Server o inferiores, al igual que ocurre en Windows XP o
inferiores, este proceso no es posible.  
###13. ¿Puedo decidir, durante el proceso de instalación, las funciones que incorporará mi
SO Windows Server? Indica esta respuesta para las dos versiones.
Solución:  
Sí. Seleccionando la instalación personalizada podemos indicar que se instalen o no
algunos servicios, pero en realidad todo el proceso de añadir o quitar componentes de red
se realiza al terminar la instalación del SO. Es importante realizar una instalación limpia, y
posteriormente agregar roles y características según las necesidades de la infraestructura
que deseemos montar.
