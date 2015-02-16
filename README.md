
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
## Actividades 1
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

    
       
## Actividades 2       
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

## Actividades 3
###1. ¿Con que herramienta crearemos usuarios en un equipo Windows Server que ha sido
promovido a controlador de dominio?
Solución:
Cuando un equipo Windows Server que no ha sido promovido a controlador de dominio,
sus grupos y usuarios se administran desde Administración de equipos > Usuarios y grupos
locales. Cuando se promueve a controlador de dominio, los grupos se tienen que dar de
alta con la herramienta administrativa Usuarios y equipos de Active Directory. Esta
herramienta la encontramos dentro del conjunto de Herramientas Administrativas del
equipo.
###2. ¿En que equipo de la red podrán iniciar sesión estos usuarios?
Solución:
En cualquier equipo cliente o servidor, que previamente haya sido unido al dominio, tal y
como veremos en la siguiente unidad.
###. Cuándo tenemos promovido un equipo Windows Server a controlador de dominio,
¿podemos crear usuarios locales para iniciar sesión en el equipo?
Solución:
No. Aunque podamos iniciar sesión localmente en el equipo que tiene instalado Directorio
Activo, los usuarios y grupos que se gestionan a partir de este momento, son usuarios y
grupos globales del dominio. Por lo tanto, la administración de grupos y usuarios locales
desde la herramienta Administración de equipos no estará activa.
###4. ¿Desde donde podremos crear grupos en el Directorio Activo?
Solución:
Los grupos se tienen que dar de alta, cuando tenemos instalado DA, con la herramienta
administrativa Usuarios y equipos de Active Directory. Esta herramienta la encontramos
dentro del conjunto de Herramientas Administrativas del equipo.
###5. ¿Es necesario crear los grupos en la unidad organizativa en la que vienen creados
los grupos integrados en el sistema?
Solución:
No. En general, los diferentes objetos que maneja el Directorio Activo, se pueden agrupar
en nuevas unidades organizativas creadas por nosotros, sin que esto afecte al
funcionamiento de los objetos creados.
###6. ¿Cuántos grupos podemos crear en un equipo con Directorio Activo?
Solución:
Los que queramos. La limitación seguro que existe, pero ni el propio fabricante del sistema
operativo indica que haya límite alguno a la creación de objetos en un dominio con
Directorio Activo.
###7. Si eliminamos un grupo de usuarios, ¿eliminamos los usuarios que pertenecen al
mismo?
Solución:
No. Los usuarios seguirán existiendo. Lo que ocurre es que los privilegios que pudieran
haber heredado del grupo que hemos borrado, desaparecerán.
###8. ¿Podemos añadir grupos a otros grupos de usuarios?
Solución:
Sí. Para ello tendremos en cuenta el ámbito de los grupos:
a) Grupos de ámbito global. Los permisos concedidos a este grupo tienen validez en
cualquier dominio. Sus miembros solo pueden actuar en el dominio donde está dado de
alta el grupo global. Pueden ser miembros de grupos universales o locales de dominio
en cualquier dominio y pueden tener como miembros a otros grupos globales y cuentas
de usuario del mismo dominio.  
b) Grupos de ámbito local de dominio. Es lo contrario de un grupo local ya que sus
miembros actúan sobre cualquier dominio, pero sus permisos sólo son efectivos para
recursos del dominio en el que se crea el grupo. Pueden tener como miembros a otros
grupos locales de dominio en el mismo dominio, grupos globales de cualquier dominio,
grupos universales de cualquier dominio y usuarios de cualquier dominio.  
c) Grupos de ámbito universal. Estos grupos, pueden tener miembros procedentes de
cualquier dominio y se les puede asignar permisos para recursos de cualquier dominio.
Solo se pueden gestionar en servidores en modo nativo (servidores y equipos del mismo
tipo de sistema operativo 2000 o 2003). Pueden tener como miembros a otros grupos
universales, grupos globales de cualquier dominio y cuentas individuales de usuarios de
cualquier dominio.
###9. ¿Podemos tener creados dos grupos con el mismo nombre en dos unidades
organizativas diferentes?
Solución:
No. Cuando creamos un objeto, indicamos el tipo de objeto que es. En este caso, si
creamos un objeto grupo de usuarios en una UO, no podremos crear otro objeto grupo de
usuarios con el mismo nombre aunque esté contenido en otra UO diferente.
###10. ¿Es obligatorio asignar contraseña cuando se crea un usuario de Directorio Activo?
Solución:
No. Con lo que sabemos hacer nosotros, sí que tendremos que asignar obligatoriamente
una contraseña, pero se pueden diseñar políticas de cuentas específicas, que permitan que
un usuario se pueda validar al dominio sin introducir ninguna contraseña. Esta operación se
considera administración avanzada, y por ello nosotros no la veremos en este curso.
###11. ¿Para que sirve el nombre NetBios de un usuario de Directorio Activo?
Solución:
Sobre todo y principalmente, para realizar validaciones desde clientes que no utilicen
nombres DNS, como pueden ser clientes Windows 98 o algunos clientes Linux, aunque la
utilidad de este nombre es mayor, sobre todo, cuando trabajemos con herramientas de
administración avanzada de dominios Windows Server.
###12. ¿Qué ocurre si modificamos el login de un usuario de Directorio Activo?
Solución:
Si se pudiera, que no se puede, estaríamos eliminando el usuario y dando de alta otro
nuevo, ya que el login de usuario en el sistema ha de ser único y no se puede modificar.
###13. ¿Se pueden crear dos usuarios con el mismo nombre pero con distinto login?
Solución:
Sí. El nombre completo del usuario es un dato, como puede ser la dirección, teléfono, etc,
que evidentemente puede ser el mismo para varios usuarios. Lo único que debe ser
diferente es el login de usuario. El resto de datos, incluyendo la contraseña, se pueden
repetir.
###14. Si una cuenta de usuario está deshabilitada, ¿este podrá iniciar sesión en el
sistema?
Solución:
No. Hasta que el administrador o un usuario con privilegios active la cuenta, a pesar de
existir en el dominio, el usuario que tenga la cuenta deshabilitada no podrá iniciar sesión en
el equipo.
###15. ¿Es obligatorio configurar el servicio DNS para que los usuarios se puedan validar
en el dominio?
Solución:
Sí. Pero, más que para los usuarios, es necesario para que el controlador de dominio sea
capaz de reconocer los clientes o máquinas desde las que los usuarios se validan al
dominio. Si no se asocia el nombre de máquina a una IP y el servidor no reconoce esta
asociación, aunque tengamos usuarios dados de alta correctamente en el sistema, estos no
se podrán validar ya que no se reconocerán los equipos desde los que se quieren conectar
###16. ¿Es necesario configurar el servicio DNS para establecer conectividad entre
dominios sin relaciones de confianza?
Solución:
Obligatoriamente. Si no se configuran los servicios DNS, no se podrán establecer
relaciones de confianza entre dominios, ya que no se podrán poner en contacto los equipos
principales en los que se ha instalado el Directorio Activo.
###17. ¿Quiénes pueden establecer relaciones de confianza entre dominios?
Solución:
Los administradores de cada dominio, o algún usuario al que se le hayan concedido
permisos o privilegios para administrar el sistema.
###18. Si montamos una relación de confianza bidireccional del dominio1 con el dominio2 y
del dominio2 con el dominio3, ¿qué implicaciones tendrá este proceso?
Solución:
Si las relaciones de confianza son de confianza total, técnicamente el dominio1 y el
dominio3 tendrán establecidas indirectamente relaciones de confianza a través del
dominio2
###19. ¿Cuántas plantillas de usuario se pueden crear en Directorio Activo?
Solución:
Tantas como queramos. Las plantillas de usuario en realidad son usuarios que no se
validarán al sistema, pero contienen las mismas propiedades que cualquier usuario del
equipo. Teniendo en cuenta esto, podemos dar de alta las plantillas que queramos, con el
mismo límite que el sistema nos podría imponer a la hora de crear usuarios normales.
###20. ¿Si eliminamos la plantilla de usuario con la que hemos creado otros usuarios,
¿estos se eliminarán también?
Solución:   
No. Como acabamos de decir, las plantillas de usuario son realmente usuarios, que no
tienen nada que ver unos con otros, a pesar que nosotros los utilicemos para crear nuevos
usuarios.
###21. ¿Se puede utilizar el nombre paco\user como nombre de usuario de Directorio
Activo?
Solución:   
No. El nombre de usuario o login no puede incluir caracteres especiales como / \ + * , etc.
Tampoco puede incluir espacios en blanco.
###22. ¿Quiénes pueden establecer delegaciones de uso y administración de dominios?
Solución:   
Solamente el administrador del dominio, o algún usuario con privilegios de administración
del mismo.
###23. Una vez realizada la delegación de control, ¿se podrá administrar un dominio desde
un cliente?
Solución:  
Sí. Aunque no se puedan realizar todas las operaciones de administración, sí que se
podrán hacer algunas cosas. No se podrán administrar usuarios, grupos, etc, puesto que lo
que hemos visto en esta unidad no es suficiente para ello. Realmente nos harían falta
algunas herramientas extra para poder administrar completamente el equipo servidor.

## Actividades 4  
###1. Una vez que tenemos integrado un equipo en un dominio, ¿podemos configurarlo
para que forme solamente parte de un grupo de trabajo?
Solución:
Sí. En este caso sacaremos el equipo cliente del dominio y lo dejaremos como un equipo
independiente de la red perteneciendo al grupo de trabajo que queramos.
###2. ¿Qué hay que tener en cuenta antes de integrar un cliente en un dominio?
Solución:
De cada cliente tendremos que comprobar lo siguiente:  
 El equipo tiene un nombre de equipo único en la red.  
 Tiene una dirección IP dentro del mismo rango de direcciones IP que el servidor.  
 La máscara de red tiene que ser la misma que la del servidor u otra que permita verle en
la red.  
 El grupo de trabajo al que pertenece el equipo, no es importante en este punto.  
 Comprobar y verificar que el cliente tiene conectividad con otros equipos de la red. Esto
lo podemos comprobar ejecutando una consola desde Inicio > Ejecutar e introduciendo
cmd. Pulsamos Aceptar, y en la ventana de la consola introducimos un comando que
comprobará si el equipo tiene conexión o no con otros de la red, y en particular con el
servidor: ping 192.168.1.1 siendo 192.168.1.1 la dirección IP del servidor o controlador
de dominio. En nuestro caso, introduciremos la IP que tenga el controlador de dominio.  
 Introducir como DNS principal del equipo cliente, la dirección IP del servidor. La DNS
secundaria, podemos poner cualquier cosa, normalmente la que utilizamos para el
acceso a Internet.  
 Asegurarnos que el usuario administrador del equipo cliente tiene contraseña,
preferiblemente la misma que el administrador del controlador de dominio.  
 Aunque no es estrictamente necesario, sí que es conveniente tener las cuentas de
usuario globales creadas en el controlador de dominio, para realizar la integración del
cliente de forma correcta.  
 Tener iniciada sesión en el dominio y en el cliente como administrador.  
###3. ¿De qué formas se pueden unir clientes Windows a un dominio con Windows
Server?
Solución:
Desde los clientes, sean del tipo que sean, tendremos que ir a la pantalla de Propiedades
del equipo o Sistema y pulsar en la pestaña Nombre de equipo.
En Windows XP seleccionaremos Propiedades de Mi PC. En Windows Vista/7/8 iremos a
Propiedades de Equipo > Cambiar la configuración. Y pulsaremos en la pestaña Nombre de
equipo. En cualquiera de los dos casos, pulsaremos el botón Cambiar. Aparecerá una
pantalla en la que introduciremos el nombre del dominio al que queremos unir este equipo,
habiendo seleccionado previamente el botón de radio del cuadro Miembro de > Dominio.Otra forma, es pulsando Id de red cuando estemos en la pestaña Nombre de equipo del
cuadro de diálogo Propiedades del sistema. De esta forma veremos qué pasos son los
necesarios para realizar el proceso, matizando lo que en realidad pasa.
###4. ¿Es necesario que el cliente a integrar en un dominio cuente con una dirección IP
válida?
Solución:
Sí. Esta dirección puede ser estática o estar asignada por un servidor DHCP de la red, pero
en cualquier caso el cliente deberá tener una IP en el mismo rango que el servidor del
dominio al que lo queremos unir.
###5. ¿Podemos integrar un equipo en dos dominios diferentes?
Solución:
No simultáneamente. Solamente podemos tenerlo integrado en un dominio. Otra cosa es
que, posteriormente, establezcamos relaciones de confianza de este dominio con otro, en
cuyo caso el cliente podrá iniciar sesión en ambos dominios, pero realmente pertenecerá a
uno de ellos solamente. También podemos hacer que un cliente pertenezca primero a un
dominio y luego a otro, pero a ambos a la vez no.
###6. ¿Se pude iniciar sesión en modo local si el equipo cliente está unido a un dominio?
Solución:
Sí. Para ello nos validaremos con un usuario local del equipo, indicando en la pantalla de
entrada Conectar a… el equipo local si es XP, y tecleando el nombre del equipo local o
nombre de usuario para loguearse en el equipo.
###7. El inicio de sesión yaiza@midominio.es, ¿Es un nombre de inicio de sesión válido?
¿En qué tipo de cliente Windows?
Solución:
Sí. Es un nombre correcto de inicio de sesión, para dominios Windows Server a partir de la
versión 2000, ya que el nombre de inicio de sesión es un nombre DNS. En concreto este
nombre indica:  
 Yaiza. Nombre de ususario del dominio.  
 @. Validación DNS.  
 midominio.es: nombre DNS del dominio.  
Este login de usuario no puede utilizarse en clientes Linux antiguos ni en Windows 9X/Me.
###8. Si hemos iniciado sesión en un dominio, ¿podemos iniciar una segunda sesión con
otro usuario en modo local?
Solución:
Sí. En el equipo cliente, se pueden iniciar de forma simultánea al menos hasta 10 sesiones
de trabajo, locales o en dominio.
###9. ¿Y con otro usuario del dominio?
Solución:
Sí. Ocurre igual que en la actividad anterior.
###10. ¿Se puede iniciar sesión en un dominio con un usuario local del equipo?
Solución:  
No. Los usuarios locales permiten la validación en el equipo local, y los usuarios del
dominio permiten la validación en el controlador de dominio, pero en ningún caso a la
inversa.
###11. Cuando compartimos un recurso de un controlador de dominio, ¿estamos asignando
permisos sobre el mismo?
Solución:
Sí. Por defecto se generan unos permisos de seguridad al recurso compartido, permitiendo
solamente el acceso de lectura a todos los usuarios del dominio.
###12. ¿Es cierto que los permisos sobre un recurso solamente se pueden aplicar cuando
está compartido?
Solución:
No. Los permisos de seguridad sobre un recurso del controlador de dominio, se pueden
asignar tanto si el recurso está compartido como si no lo está.
###13. ¿Qué implica que un usuario tenga concedido el permiso de Control total sobre un
recurso del controlador de dominio?
Solución:
Implica que puede leer, escribir, crear carpetas, borrar carpetas y archivos, etc, en
definitiva, puede hacer lo que quiera sobre el mismo.
###14. Si un usuario tiene concedido el permiso de lectura sobre una carpeta compartida en
el dominio, ¿puede crear nuevas carpetas dentro del mismo?
Solución:
No. Sería necesario que se le hubiera concedido el permiso de escritura o de control total
para poder crear nuevas carpetas o archivos dentro del recurso compartido.
Con el permiso de lectura, como su nombre indica, solamente puede leer o explorar los
archivos o carpetas del recurso.  
###15. ¿Qué grupo por defecto tiene concedidos permisos sobre cualquier recurso del
dominio?
Solución:
Usuarios o users.
###16. ¿Puede un usuario local de un equipo cliente, administrar una impresora del
dominio?
Solución:
Sí. Siempre y cuando el administrador del dominio le haya concedido permisos de
seguridad que permitan esta acción.
###17. ¿Qué particularidad tienen los recursos compartidos cuyo nombre termine en $?
Solución:
No se muestran en el entorno de red de ningún equipo de la red. Son recursos
compartidos, pero ocultos.
Solamente podremos acceder a ellos desde Inicio > Ejecutar, indicando algo similar a esto:
\\nombre_servidor\\nombre_recurso_compartido$.  
###18. ¿Qué recursos se dan a compartir inmediatamente después de la instalación y
promoción a controlador de dominio de un equipo con Windows Server?
Solución:
Suelen ser estos:
 ADMIN$. Recurso que utiliza el sistema durante la administración remota del equipo.  
 IPC$. Recurso en dónde se comparte con canalizaciones especiales para la  
comunicación entre procesos y programas, de tal forma que permite al equipo cliente
enviar diferentes comandos al servidor como, por ejemplo, la acción de mostrar recursos
compartidos.  
 NETLOGON. Se usa para el inicio de sesión.  
 PRINT$. Recurso utilizado para la administración remota de carpetas.  
 FAX$. Igual que el anterior, pero para fax.  
 Unidad_logica_$. Para poder acceder en modo remoto al recurso en cuestión, que al
tener el dólar no será visible para ningún equipo de la red. Solamente lo podrá utilizar
quien sepa cómo se llama. Si por ejemplo compartimos la unidad de CD-ROM como D$
en vez de cómo D, el acceso remoto será prácticamente igual, a diferencia de que el
recurso no se verá desde el entorno de red.  
 SYSVOL. Contiene el catálogo global de las bases de datos de los controladores de
dominio del Active Directory.  
###19. Cuando compartimos un recurso de un cliente, ¿qué usuarios podrán acceder a el:
locales o usuarios del dominio?
Solución:
Todos. Podemos otorgar permisos de acceso a usuarios locales, desde la pestaña
Seguridad, o a usuarios del dominio. 

## Actividades 5
###1. ¿Se pueden asignar perfiles móviles a todos los usuarios del dominio?
Solución:
Sí. No existe ninguna restricción para este tipo de acciones. Si no asignamos perfiles
móviles, localmente en cada equipo en el que el usuario inicie sesión se creará un perfil
local.
###2. ¿Se pueden asignar perfiles móviles a usuarios locales de un cliente Windows
integrado en un dominio?
Solución:
No. Los perfiles móviles solamente se pueden asignar a usuarios de Directorio Activo, es
decir, a usuarios globales del dominio. Los usuarios locales solamente pueden gestionar
y utilizar perfiles locales de la máquina en la que están dados de alta.
###3. ¿Dónde se guardan los perfiles móviles de usuario, en el equipo cliente o en el
servidor?
Solución:
En el Servidor. Localmente se almacena una copia del perfil del dominio, pero la
configuración real de los elementos que ha personalizado el usuario se almacenará
siempre en la carpeta que en el controlador de dominio hayamos designado para este
fin.
###4. ¿Qué indica la variable de entorno %username%?
Solución:
El nombre de usuario que se ha validado al controlador de dominio, o usuario que ha
iniciado sesión localmente en un equipo. %username% se sustituye por el login del
usuario que ha iniciado sesión localmente o en el dominio.
###5. ¿Qué tipo de memoria es la que conviene optimizar en un sistema con Windows
Server?
Solución:
La memoria de intercambio, para que el sistema pueda ir algo más ligero y se produzcan
menos accesos a disco para realizar la paginación de archivos abiertos.
###6. ¿Se puede modificar la ubicación del archivo de paginación en Windows Server?
Solución:
Sí. No es muy recomendable, pero podemos elegir otra unidad de disco o partición de
nuestro sistema para realizar el intercambio de archivos. Lo ideal es disponer de un
segundo disco duro, que sea rápido y en el que se realice exclusivamente la
paginación, y que no se utilice para almacenar archivos de datos de usuario o del
propio SO.
###7. ¿En qué disco es conveniente ubicar el archivo de paginación de Windows
Server?
Solución:
Como acabamos de indicar, lo correcto es dejar el archivo de paginación en el disco
duro principal o, en todo caso, en un segundo disco duro independiente, siempre y
cuando este sea al menos igual de rápido que el principal y tenga espacio suficiente
para realizar el intercambio.
###8. ¿Qué usuarios pueden administrar la gestión de cuotas de disco?
Solución:
El administrador del sistema y todos aquellos usuarios globales del dominio que tengan
privilegios de súper usuarios del sistema.
###9. ¿Cuánto espacio de disco puede utilizar cualquier usuario del dominio sobre un
recurso compartido que está en el controlador de dominio?
Solución:
A priori, todo el espacio disponible del disco, a menos que asignemos cuotas de disco
a dicho usuario sobre este recurso compartido.
###10. ¿De qué forma podemos saber cuándo un usuario ha excedido la cuota de disco
que tiene asignada?
Solución:
Mediante los avisos que se generan en el visor de sucesos del sistema. Gracias a esta
herramienta, y si hemos configurado las cuotas de disco con un tamaño mínimo,
estableciendo los Limites de advertencia adecuadamente.
###11. Si un usuario tiene acceso a dos carpetas del controlador de dominio y tiene una
cuota de disco de 1 MB, ¿cuánta información podrá almacenar en cada carpeta?
Solución:
En total, en todos sus recursos, el usuario no podrá utilizar un espacio superior a 1 MB,
aunque tuviera acceso a varias carpetas compartidas en las que pudiera grabar
información. Las cuotas de disco se asignan por usuario, pero no se asignan por
recurso.
###12. ¿De qué formas podemos ejecutar el administrador de tareas en Windows Server?
Solución:
De forma similar a como se hace en las versiones de sobremesa de Windows, pulsando
las teclas, Ctrl+Alt+Supr, o desde la barra de tareas, pulsando con el botón derecho del
ratón en Administrador de Tareas.
###13. ¿Cómo se puede detener un proceso en Windows Server?
Solución:
Desde el Administrador de Tareas, en la pestaña Procesos, seleccionando el proceso y
pulsando con el botón derecho del ratón en Terminar proceso o Terminar el árbol de
procesos (esta segunda opción, solamente para procesos multihebra o multihilo).
###14. ¿Se pueden detener todos los servicios que están en ejecución en Windows
Server?
Solución:
No. Hay servicios que si los detenemos el sistema no arrancaría. Por eso el propio
sistema impide que paremos algunos de los servicios considerados como “esenciales”
para el funcionamiento del equipo.
###15. Si un servicio no se puede detener, ¿a qué puede ser debido?
Solución:
A que es un servicio que tiene dependencias, y por lo tanto su parada implica la
detención de otros servicios que dependen de él, o también porque la aplicación de la
que depende este servicio está en ejecución y hasta que no la finalicemos no podremos
detener el servicio.
###16. ¿En qué ubicaciones se pueden hacer copias de seguridad completas de Windows
Server?
Solución:  
En cualquier lugar que no incluya los archivos de los que queremos realizar la copia de
seguridad. Se pueden hacer copias de seguridad en otros discos duros locales, discos
en red, dispositivos de almacenamiento externo, como USB, e incluso en DVD-RW.
###17. ¿Para qué se etiqueta un disco de copia de seguridad en Windows Server?
Solución:
Para poder identificarlo posteriormente y tener claro que tipo de copia se hizo en este
disco, cuando, y con que configuración. Es simplemente una identificación de la copia de
seguridad.
###18. ¿Podemos restaurar una copia de seguridad completa en otro equipo distinto pero
de idénticas carecterísticas a aquel en el que se realizó una copia de seguridad
completa de Windows Server?
Solución:
Sí. Seguramente tengamos algún problema de hardware, pero técnicamente esta
operación es posible y tiene que funcionar correctamente, ya que el equipo en el que se
restaura la copia de seguridad es idéntico al equipo original.
###19. ¿Es conveniente programar tareas en un equipo que utilizamos muy de vez en
cuando?
Solución:
No. Normalmente las tareas se programan para evitar tener que estar pendiente de
realizar operaciones repetitivas habituales. Si queremos, por ejemplo, chequear el disco
una vez al año, el programar una tarea para ello tal vez esté de más, aunque pueda ser
la única forma de acordarnos de hacerlo, pero en general no se recomienda programar
tareas poco habituales.
###20. ¿Se pueden asociar las tareas programadas a eventos del sistema?
Solución:
Sí. En Windows 2003 o 2008 Server se puede realizar esta asociación, teniendo en
cuenta que las opciones que ofrece 2008 Server, en cuanto a eventos a los que asociar
tareas programadas, es mucho mayor que las que ofrece 2003 Server.
###21. ¿Podemos hacer que una tarea programada apague el sistema?
Solución:
Sí. Siempre y cuando conozcamos el comando que se utiliza para apagar el sistema,
que en este caso es el comando Shutdown con los parámetros requeridos.  
