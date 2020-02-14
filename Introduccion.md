# OpenStack
Plataforma de IaaS (sofware, pplataforma e infraestructura como servicio) en software libre. Puede utilizarse en nube pública, privada e híbrida. Aunque tiene más nicho de mercado en privada, porque en pública compite con AWS, Azure, Google, Alibaba, etc.

OpenStack es porque:
- Se quiere instalar nuestro propio software para proporcionar IaaS.
- Tenemos necesidad de infraestructura variable.
- Software libre.
- Proyecto estable, con muchos apoyos y muy buenas perspectivas de futuro.
- Tiene muchas funcionalidades.
- Se está convirtiendo en el SO cloud ¿o era el kérnel?
- Podemos utilizar hardware convencional.
- Cada vez es más fácil de instalar.

## Integrated Release
Modelo de desarrollo hasta OpenStack Kilo:
- Proyectos oficiales. Los que se incluían.
- Proyectos incubados. No se incluían pero tu podías meterlos.
- Publicación conjunta: 201X.Y
- Nivel de madurez varaible.
- Resto de Ecosistemas. Cosas que no estaban dentro del proyecto.

## Componentes de OpenStack
#### Nova
Lo que incialmente desarrolló la NASA. Proporciona máquinas virtuales. Con un driver de Nova también levanta LXC. Este es el componente original de OpenStack.

Tiene varios subcomponenetes a su vez:
**Nova API**
**nava-conductor**
**nova-console**: gestiona las consolas de texto que salen en cada máquina que se ejecuta. Porque el usuario no tiene acceso directo a la consola de las máquinas.
**Nova-scheduler**: reparte el trabajo.
**nova-computer**: se ejecuta en un hipervisor.

Importante: definir CPU y RAM allocation-ratio que es la relación entre cores virtuales y reales. 
Importantes: con KVM es convenietne configurar KSM, la paginación de la RAM. 

#### Keystone (servicio de identidad)
Proyectos, usuarios, roles, etc. 
- Implementa RBAC (Role Based Access Control), los permisos no son por usuarios sino por roles. 
- Autenticación por tokens,
- Gestión de los Endpoints. Define la URL de acceso para la API web de cada componente (https://172.22.200.1:5000/keystone_v3/)
- Autenticación básica, SQL o LDAP.
- Se puede implementar otros sistemas: SAML, OpenID...
- No hay roles por defecto: Admin y _member_

#### Glance (servicio de imágenes)
- Gestió de imágenes e instantáneas.
- Formato qcow2 es el nativo, entre comillas, porque usa 'nativamente ' KVM.
- Acepta otros formatos: raw, vhd, vdi, porque puede usar un hipervisor diferente a KVM.
- Se pueden almacenar como objetos en Swift las imágenes e instantáneas.

#### Zun
Levanta contenedores. 

#### Qinling
Funciones como servicios. Mandar acciones a una máquina para que se ejecute y devuelva el resultado. Este componente y el anterior son más nuevos, por lo tanto no son tan estables, y no son componentes cores, sino adicionales. 

### Hardware Lifecycle
#### Ironic
Hardware, físico. Instalador de servidores físicos como servicios.

### Almacenamiento
#### Swift
Desde el principio. Almacena de objetos. Es independiente al resto de los componentes. Almacenamiento distribuido y en alta disponibilidad. Utilización muy sencilla. 

#### Cinder
Gestión de volúmenes, las instantáneas de los volúmenes y la conección de estos. Tiene múltiples "backends". Como nova tiene varios componentes: cinder-console, 

Cinder no se ocupa de redundancia, de raid ni nada de eso, sino que se pone en contacto con otro software (o hardware) para crear los volúmenes.

#### Manila
Gestión de sistemas de ficheros.

#### Neutron
Encargado de las redes, las subredes, router. Es la más complicada porque se dedica a la virtualización de redes.


Todos estos componentes podrían agruparse en dos:
- Servicios de infraestructuras: como Nova, Cinder, Neuton, Swift, Zun que proporcionan algo en bruto. Etnre ellos, uno da las máquinas, otro volúmenes, otro redes, etc. 
- Una capa de software sobre las infractuturas. Solo sirve para mejorar la experiencia, somo servicios adicionales, como configurar el dns, alertas. Ejemplos: manila, octavia, designate, aodh, freezer.
- También habría un tercer grupo de componenetes que serían core, pero son adicionales. Por ejmplo Glance o Keystone. 

## Características generales
- Módulos independiente.
- Es un software de alto nivel, es decir, que no hace las cosas desde abajo sino que por ejemplo Nova tira de libvirt y Cinder tira de iSCI, ZFS, Ceph, etc. Es por eso por lo que está escrito en Python. 
- Oslo: bibliotecas comunes.
- Base de datos por componente. Por esto afecta al rendimiento. 
- Comunicación vía API web.
- AMQP: es una cola de mensajes para que se comuniquen los procesos entre sí aunque estén en diferentes máquinas.
- Cada compoenente tiene una herramienta desde la línea de comando, CLI, independiente desde el espacio de usuario. 



