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

## Introducción
### Integrated Release
Modelo de desarrollo hasta OpenStack Kilo:
- Proyectos oficiales. Los que se incluían.
- Proyectos incubados. No se incluían pero tu podías meterlos.
- Publicación conjunta: 201X.Y
- Nivel de madurez varaible.
- Resto de Ecosistemas. Cosas que no estaban dentro del proyecto.

### Componentes de OpenStack
##### Nova
Lo que incialmente desarrolló la NASA. Proporciona máquinas virtuales. Con un driver de Nova también levanta LXC. Este es el componente original de OpenStack.

Tiene varios subcomponenetes a su vez:
**Nova API**
**nava-conductor**
**nova-console**: gestiona las consolas de texto que salen en cada máquina que se ejecuta. Porque el usuario no tiene acceso directo a la consola de las máquinas.
**Nova-scheduler**: reparte el trabajo.
**nova-computer**: se ejecuta en un hipervisor.

Importante: definir CPU y RAM allocation-ratio que es la relación entre cores virtuales y reales. 
Importantes: con KVM es convenietne configurar KSM, la paginación de la RAM. 

##### Keystone (servicio de identidad)
Proyectos, usuarios, roles, etc. 
- Implementa RBAC (Role Based Access Control), los permisos no son por usuarios sino por roles. 
- Autenticación por tokens,
- Gestión de los Endpoints. Define la URL de acceso para la API web de cada componente (https://172.22.200.1:5000/keystone_v3/)
- Autenticación básica, SQL o LDAP.
- Se puede implementar otros sistemas: SAML, OpenID...
- No hay roles por defecto: Admin y _member_

##### Glance (servicio de imágenes)
- Gestió de imágenes e instantáneas.
- Formato qcow2 es el nativo, entre comillas, porque usa 'nativamente ' KVM.
- Acepta otros formatos: raw, vhd, vdi, porque puede usar un hipervisor diferente a KVM.
- Se pueden almacenar como objetos en Swift las imágenes e instantáneas.

##### Zun
Levanta contenedores. 

##### Qinling
Funciones como servicios. Mandar acciones a una máquina para que se ejecute y devuelva el resultado. Este componente y el anterior son más nuevos, por lo tanto no son tan estables, y no son componentes cores, sino adicionales. 

#### Hardware Lifecycle
##### Ironic
Hardware, físico. Instalador de servidores físicos como servicios.

#### Almacenamiento
##### Swift
Desde el principio. Almacena de objetos. Es independiente al resto de los componentes. Almacenamiento distribuido y en alta disponibilidad. Utilización muy sencilla. 

##### Cinder
Gestión de volúmenes, las instantáneas de los volúmenes y la conección de estos. Tiene múltiples "backends". Como nova tiene varios componentes: cinder-console, 

Cinder no se ocupa de redundancia, de raid ni nada de eso, sino que se pone en contacto con otro software (o hardware) para crear los volúmenes.

##### Manila
Gestión de sistemas de ficheros.

##### Neutron
Encargado de las redes, las subredes, router. Es la más complicada porque se dedica a la virtualización de redes.


Todos estos componentes podrían agruparse en dos:
- Servicios de infraestructuras: como Nova, Cinder, Neuton, Swift, Zun que proporcionan algo en bruto. Etnre ellos, uno da las máquinas, otro volúmenes, otro redes, etc. 
- Una capa de software sobre las infractuturas. Solo sirve para mejorar la experiencia, somo servicios adicionales, como configurar el dns, alertas. Ejemplos: manila, octavia, designate, aodh, freezer.
- También habría un tercer grupo de componenetes que serían core, pero son adicionales. Por ejmplo Glance o Keystone. 

### Características generales
- Módulos independiente.
- Es un software de alto nivel, es decir, que no hace las cosas desde abajo sino que por ejemplo Nova tira de libvirt y Cinder tira de iSCI, ZFS, Ceph, etc. Es por eso por lo que está escrito en Python. 
- Oslo: bibliotecas comunes.
- Base de datos por componente. Por esto afecta al rendimiento. 
- Comunicación vía API web.
- AMQP: es una cola de mensajes para que se comuniquen los procesos entre sí aunque estén en diferentes máquinas.
- Cada compoenente tiene una herramienta desde la línea de comando, CLI, independiente desde el espacio de usuario. 

### Formas de instalación
#### Elegir una versión de OpenStack
- Las versiones tiene 18 meses de soporte directo. Esto está cambiando porque es un periodo muy corto. Es por eso que aparece un nuevo término "Extended Maintenance" que es como una extensión en el soporte. 
- Ya no hay grandes diferencias en los componentes esenciales.
- Elegir qué componentes son necesarios y descartar las versiones de OpenStack que no los incluyan.
- No siempre la última versión es recomendable. Se recomienda, en estos momentos, Stein o Train.

#### Instalación manual
- Elegir una distribución GNU/Linux que incluya en sus repositorios los paquetes de OpenStack. Las más utilizadas son Ubuntu y CentOS. 
- Realiza una instalación manual siguiendo la documentación oficial.
- Es recomendable hacer al menos una vez una isntalación manual apra familiarizarse con los componentes de OpenStack.


#### Instalación mediante CMS
- Existen "recetas" para la configuración a través de Chef, Puppet, Openstack-chef y OpenStack-Ansible (estos dos oficiales). En el caso de Ansible hay otros no oficiales como por ejemplo [la recera del IES Gonzalo Nazareno](https://github.com/iesgn/openstack-ubuntu-ansible).

#### Openstack Fuel
Esta se ha quedado obsoleto. Es de la empresa Mirantis. Incluye un instalador web e incluye soporte en alta disponibilidad. 

#### RDO
Despliegue sencillo en Red Hat y derivadas. Es para despliegue, no para producción, pero para tener un pequeño OpenStack.

#### Openstack on Openstack (tripleo)
- Principalmente pensado para grandes despliegues en centros de datos.
- Muy relacionado con Ironic.
- Utiliza templates de heat.
- Undercloud/Overcloud.

#### Ubuntu
Además de empaquetar cada versión de OpenStack para Ubuntu LTS, está desarrollando su propia pila de aplicaciones.

#### Red Hat
Red Hat OpenStack Platform es el soporte que ofrece Red Hat.

#### Openstack Kolla
- Proporciona contenedores docker y libros de jugadas de ansible para gestionar nubes de OpenStack. Proyecto oficial de OpenStack.
También está: kolla-ansible (utiliza openstack kolla para aplicarle las recetas) y kolla-kubernetes.


## Arquitectura de OpenStack
### Arquitectura
- ¿Qué máquinas necesitamos?
- ¿Qué redes físicas/lógicas?
- ¿Qué componentes se instalan en cada nodo?
- Rodo esto conforme a: requisitos, costes, rendimmiento, seguridad y escalabilidad. 

**nodo controlador** se define la gestión de recursos. Tiene dos componentes:
- api
- scheduler
**nodo computación**: hipervisores.
**nodo de red**: se refiere al equipo donde se va a gestionar los router, la ip flotante, etc.
**nodo de almancenamiento**: depende mucho del sistema de almacenamiento que se va a usar. 
> Otros para servicios genéricos:
**nodos para las bases de datos relacionales** normalmente se monta galera, versión de mariaDB que viene configurada como un cluster en modo activo activo. 
**AMQP** para las colas de mensajes. 

