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
### Servicios de computación - Compute
#### Nova
Lo que incialmente desarrolló la NASA. Proporciona máquinas virtuales. Con un driver de Nova también levanta LXC. Este es el componente original de OpenStack.

#### Zun
Levanta contenedores. 

#### Qinling
Funciones como servicios. Mandar acciones a una máquina para que se ejecute y devuelva el resultado. Este componente y el anterior son más nuevos, por lo tanto no son tan estables, y no son componentes cores, sino adicionales. 

### Hardware Lifecycle
#### Ironic
Hardware, físico. Instalador de servidores físicos como servicios.

### Almacenamiento
#### Swift
Desde el principio.

#### Cinder
Gestión de volúmenes.

#### Manila
Gestión de sistemas de ficheros.

