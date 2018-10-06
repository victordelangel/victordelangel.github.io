---
layout: default
title: "CI/CD for SQL Server"
---
Contenido {#contenido .TOCHeading}
=========

[Pre-requisitos 1](#pre-requisitos)

[Crear infraestructura de despliegue
2](#crear-infraestructura-de-despliegue)

[Crear Grupo de Recursos 2](#crear-grupo-de-recursos)

[Crear Base de datos PaaS Azure SQL Database.
2](#crear-base-de-datos-paas-azure-sql-database.)

[Crear Base de datos IaaS con SQL Server Enterprise Edition.
4](#crear-base-de-datos-iaas-con-sql-server-enterprise-edition.)

[Configurar permisos de conexión de la base de datos
5](#configurar-permisos-de-conexión-de-la-base-de-datos)

[Crear Base de datos de prueba 5](#crear-base-de-datos-de-prueba)

[Crear catálogo de prueba para Integration Services
6](#crear-catálogo-de-prueba-para-integration-services)

[Agregar usuario para despliegue de Analysis Services
6](#agregar-usuario-para-despliegue-de-analysis-services)

[Crear proyecto de VSTS 7](#crear-proyecto-de-vsts)

[Creación de proyectos 7](#creación-de-proyectos)

[Alta de miembros del equipo 7](#alta-de-miembros-del-equipo)

[Repositorio de versiones de SQL Server, Integration Services y Análisis
Services.
8](#repositorio-de-versiones-de-sql-server-integration-services-y-análisis-services.)

[Configurar Service Point de Azure en Resource Group
8](#configurar-service-point-de-azure-en-resource-group)

[Configurar Deployment Group para SQL Server en IaaS
9](#configurar-deployment-group-para-sql-server-en-iaas)

[Crear Personal Access Token 9](#crear-personal-access-token)

[Ejecución de Agente de Team Services en VM con SQL Server
10](#ejecución-de-agente-de-team-services-en-vm-con-sql-server)

[Integración Continua 12](#integración-continua)

[Configurar Build de SQL Server 12](#configurar-build-de-sql-server)

[Configurar Build de Integration Services
14](#configurar-build-de-integration-services)

[Configurar Build de Analysis Services
16](#configurar-build-de-analysis-services)

[Despliegue Continuo 18](#despliegue-continuo)

[Configurar Release de Azure SQL Database
18](#configurar-release-de-azure-sql-database)

[Configurar Release de SQL Server en IaaS
20](#configurar-release-de-sql-server-en-iaas)

[Configurar Release de Integration Services en IaaS
22](#configurar-release-de-integration-services-en-iaas)

[Configurar Release de Analysis Services en IaaS
23](#configurar-release-de-analysis-services-en-iaas)

Pre-requisitos
==============

1.  Cuenta de VSTS

2.  Cuenta de Azure con rol de owner

3.  Crear Resource Group.

4.  Crear VM con SQL Server Enterprise y Azure SQL Server

Crear infraestructura de despliegue
===================================

Crear Grupo de Recursos
-----------------------

1.  Firmarse en la consola de Azure.

2.  Dentro del menú de la izquierda seleccionar el submenú:
    ![](media/image1.png){width="0.8432830271216099in"
    height="0.21286745406824148in"}

3.  Dar clic en la opción:
    ![](media/image2.png){width="1.0329899387576553in"
    height="0.1857895888013998in"}

4.  Clic en el botón superior
    ![](media/image3.png){width="0.4591622922134733in"
    height="0.20662292213473316in"}

5.  En el campo "Resource group name" colocar el texto
    "WorkshopDEVOPSBI", en el campo "Subscription" elegir la
    subscripción de Azure donde sean dueños, y en el campo "Resource
    group location" elegir la región "East US 2".

6.  Clic en el botón: ![](media/image4.png){width="0.6119411636045494in"
    height="0.1842399387576553in"}

Crear Base de datos PaaS Azure SQL Database.
--------------------------------------------

1.  Dentro del menú de la izquierda seleccionar el submenú:
    ![](media/image1.png){width="0.8432830271216099in"
    height="0.21286745406824148in"}

2.  Dar clic en la opción:
    ![](media/image5.png){width="0.9328357392825897in"
    height="0.2511482939632546in"}

3.  Clic en el botón superior
    ![](media/image3.png){width="0.4591622922134733in"
    height="0.20662292213473316in"}

4.  Llenar los campos como se muestra en la siguiente imagen, variar la
    suscripción por el nombre de la subscripción donde sean dueños.

> ![](media/image6.png){width="2.5802898075240597in" height="4.5625in"}

5.  Crear un nuevo Servidor al dar clic en la sección Server. (tendrá un
    signo de admiración en rojo)

6.  Llenar los datos como se muestra en la siguiente imagen: (importante
    guardar la contraseña que le pongan al servidor ya que será
    utilizada más adelante)

    ![](media/image7.png){width="6.5in" height="2.75in"}

7.  Dar clic en el botón "Select", a continuación, elegir el tamaño de
    la base de datos, un Standard S4 sería un tamaño recomendable.

8.  Al finalizar, el menú de creación de la base de datos debería verse
    como en la siguiente imagen:

    ![](media/image8.png){width="2.0849398512685915in"
    height="3.8656714785651793in"}

9.  Clic en el botón: ![](media/image4.png){width="0.6119411636045494in"
    height="0.1842399387576553in"}

10. A continuación, será necesario agregar la ip de nuestro equipo al
    firewall de la base de datos, para ello, debemos abrir la
    configuración de la base de datos una vez creada.

    a.  Dentro del menú de la izquierda seleccionar el submenú:
        ![](media/image1.png){width="0.8432830271216099in"
        height="0.21286745406824148in"}

    b.  Dar clic en la opción:
        ![](media/image5.png){width="0.9328357392825897in"
        height="0.2511482939632546in"}

    c.  Dar clic en la base de datos de plataforma que fue recientemente
        creada.

    d.  Clic en el menú:
        ![](media/image9.png){width="0.8582097550306211in"
        height="0.2080511811023622in"}

    e.  En la barra de menú superior, buscar el botón
        ![](media/image10.png){width="0.9179101049868766in"
        height="0.23823600174978127in"} y dar clic en el botón.

    f.  Dar clic al botón:
        ![](media/image11.png){width="0.7761187664041995in"
        height="0.20424212598425198in"}, esto agregará la dirección de
        ip pública de nuestro equipo.

    g.  Clic en el botón:
        ![](media/image12.png){width="0.4850754593175853in"
        height="0.25090113735783026in"}

Crear Base de datos IaaS con SQL Server Enterprise Edition.
-----------------------------------------------------------

1.  Dentro del menú de la izquierda seleccionar el submenú:
    ![](media/image1.png){width="0.8432830271216099in"
    height="0.21286745406824148in"}

2.  Dar clic en la opción:
    ![](media/image13.png){width="1.126865704286964in"
    height="0.28738845144356956in"}

3.  Clic en el botón superior:
    ![](media/image3.png){width="0.4591622922134733in"
    height="0.20662292213473316in"}

4.  Seleccionar la imagen SQL Server 2017 Enterprise.

5.  Clic en el botón: ![](media/image4.png){width="0.6119411636045494in"
    height="0.1842399387576553in"}

Llenar el formulario como se muestra a continuación y dar clic en el
botón "OK"(Guardar la contraseña seleccionada)

![](media/image14.png){width="2.128687664041995in"
height="4.947761373578302in"}

6.  Elegir el tamaño de la máquina virtual, un tamaño D2s\_v3 es
    aceptable para esta prueba y dar clic en "Select".

    ![](media/image15.png){width="6.5in" height="0.24097222222222223in"}

7.  En la siguiente pantalla, mantener todas las opciones en Default, en
    el submenú "Select public inbound ports" elegir RDP (3389) y dar
    clic en "OK".

8.  En la siguiente pantalla, será necesario cambiar el combo "SQL
    connectivity" a "Public (Internet)", mantener las siguientes
    opciones en Default y dar clic en "OK".

9.  Aceptar los términos y condiciones y dar clic en el botón "Create"

Configurar permisos de conexión de la base de datos
---------------------------------------------------

### Crear Base de datos de prueba

1.  Abrir la aplicación SQL Server Management Studio y conectarse al
    "Database Engine" del servidor local.

2.  En el Object Explorer, clic en Databases y a continuación en "New
    Database"

    ![](media/image16.png){width="2.109145888013998in"
    height="1.058613298337708in"}

3.  En el campo "Database name:" colocar el texto: "DEVOPS-DEV" y dar
    clic en OK.

4.  Seleccionar la carpeta Security.

5.  Dar clic con el botón secundario en Security y seleccionar New -\>
    User

6.  Agregar al usuario "NT AUTHORITY\\SYSTEM"

7.  Dar los privilegios de sysadmin para todo el servidor.

8.  Llenar el formulario como se muestra a continuación:

    ![](media/image17.png){width="2.7476979440069993in"
    height="2.3059700349956254in"}
    ![](media/image18.png){width="2.6199639107611548in"
    height="2.298351924759405in"}

### Crear catálogo de prueba para Integration Services

1.  En el Object Explorer, clic con botón secundario en "Integration
    Services Catalogs" y a continuación clic en "Create Catalog".

2.  En la nueva ventana, habilitar "Enable CLR Integration"

    ![](media/image19.png){width="4.462687007874016in"
    height="0.34532699037620296in"}

3.  Escribir el password utilizado para dar de alta la base de datos.

4.  Clic en OK.

5.  Clic en el catálogo creado y a continuación crear un nuevo folder
    llamado "DevOpsDemoFolder".

### Agregar usuario para despliegue de Analysis Services

1.  En el Object Explorer, clic con botón secundario en Security -\>
    Login -\> New Login...

    ![](media/image20.png){width="1.8880599300087488in"
    height="1.0505544619422573in"}

2.  En el campo "Login name:" agregar el usuario "NT
    SERVICE\\MSSQLServerOLAPService"

3.  En el submenú Server Roles, dar privilegios de "sysadmin"

Crear proyecto de VSTS
======================

Creación de proyectos
---------------------

La primera actividad consiste en crear un proyecto en una cuenta de
Visual Studio existente, para este workshop estaremos utilizando la
cuenta: <https://tfsvidela.visualstudio.com/>, para obtener información
de cómo crear una cuenta de Visual Studio Team Services, puede encontrar
información en la siguiente URL: TODO

1.  Abrir el browser y entrar a la url:
    <https://tfsvidela.visualstudio.com/>, a continuación dar clic en el
    botón ![](media/image21.png){width="1.1618482064741906in"
    height="0.2630599300087489in"}.

2.  El el campo "Project name" colocar el nombre "DEVOPS BI Workshop",
    Asegurarse que la opción "Private" esté seleccionada y dar clic en
    el botón inferior
    ![](media/image22.png){width="0.5927657480314961in"
    height="0.24813429571303586in"}.

Alta de miembros del equipo
---------------------------

A continuación, es necesario dar de alta a todos los miembros del equipo
que participarán en el proyecto. Para lograrlo, es necesario realizar el
siguiente procedimiento:

1.  Clic en el botón ![](media/image23.png){width="0.7215069991251094in"
    height="0.2654604111986002in"} de la esquina superior derecha de la
    pantalla.

2.  En el campo "Add users or groups" escribir los correos electrónicos
    o nombre de los usuarios uno a uno como se muestra en la siguiente
    imagen:

> ![](media/image24.png){width="2.8714326334208224in"
> height="1.596392169728784in"}

3.  Finalmente dar clic en el botón
    ![](media/image25.png){width="0.5298501749781277in"
    height="0.21462270341207348in"}.

4.  A cada uno de los usuarios les llegará un correo invitándolos a
    formar parte del equipo de trabajo, seguir las indicaciones del
    correo para completar el proceso.

 Repositorio de versiones de SQL Server, Integration Services y Análisis Services.
----------------------------------------------------------------------------------

Crearemos 3 repositorios de versiones, uno para cada solución del
proyecto.

1.  Clic en el Menú ![](media/image26.png){width="0.9328357392825897in"
    height="0.24918197725284338in"} de la barra de menú de la izquierda
    y a continuación, clic en el submenú
    ![](media/image27.png){width="1.0149256342957131in"
    height="0.22635061242344706in"}.

2.  En la barra superior verán una barra de direcciones similar a la
    siguiente:

> ![](media/image28.png){width="4.365671478565179in"
> height="0.2572626859142607in"}, Dar clic en el botón
> ![](media/image29.png){width="1.3805971128608925in"
> height="0.24456255468066493in"} y a continuación en el submenú
> ![](media/image30.png){width="1.0074628171478566in"
> height="0.21840113735783026in"}.
>
> ![](media/image31.png){width="1.5943143044619423in"
> height="1.18874343832021in"}

3.  En el campo "Type", dejar seleccionado el tipo de repositorio Git.

4.  En el campo "Repository Name" colocar el texto
    "database\_repository" y a continuación clic en el botón
    ![](media/image32.png){width="0.5671642607174103in"
    height="0.21441601049868766in"}.

5.  Repetir los pasos 2 al 4 para crear los repositorios
    "ssis\_repository" y "ssas\_repository" quienes serán utilizados
    para los proyectos de Integration Services y Análisis Services
    respectivamente.

Configurar Service Point de Azure en Resource Group
---------------------------------------------------

Para permitir que VSTS pueda desplegar aplicaciones en Azure, es
necesario configurar un Service Point y autorizar los permisos, debemos
seguir los siguientes pasos:

1.  En la barra de menú de la izquierda, y en la parte de inferior, dar
    clic en el botón:
    ![](media/image33.png){width="0.9776126421697288in"
    height="0.19430774278215224in"}

2.  Buscar el submenú "Build and release" y dar clic en la opción:
    ![](media/image34.png){width="1.1194028871391075in"
    height="0.1771205161854768in"}

3.  Dar clic en el combo
    ![](media/image35.png){width="1.3358213035870516in"
    height="0.19083114610673665in"}y clic en la opción "Azure Resource
    Manager". Se abrirá un formulario.

4.  En Connection name colocar el texto "Azure Default Connection", en
    el campo "Subscripcion" elegir la suscripción de Azure donde sean
    dueños, y finalmente en el campo Resource Group, elegir el grupo de
    recursos donde están los servicios de Azure que creamos previamente,
    finalmente dar clic en el botón OK.

> ![](media/image36.png){width="3.634367891513561in"
> height="2.9476060804899387in"}

Configurar Deployment Group para SQL Server en IaaS
---------------------------------------------------

A continuación, es necesario configurar el grupo de despliegue de la
máquina virtual que instalará de forma automatizada los cambios en el
repositorio de versiones, para ello es necesario realizar los siguientes
pasos.

### Crear Personal Access Token

1.  Dar clic en el ícono del usuario
    ![](media/image37.png){width="0.34375in"
    height="0.3020833333333333in"} que se encuentra en la parte superior
    derecha que se muestra en la siguiente imagen:

    ![](media/image38.png){width="1.4507141294838146in"
    height="0.9359448818897638in"}

2.  Dar clic en la opción
    ![](media/image39.png){width="0.8059700349956256in"
    height="0.23631999125109363in"}

    ![](media/image40.png){width="1.5028532370953631in"
    height="1.0897287839020122in"}

3.  En la sección "Security", elegir la opción
    ![](media/image41.png){width="1.241075021872266in"
    height="0.18128062117235347in"} y a continuación dar clic en "Add"

4.  Establecer una descripción como ejemplo: "PAT de grupo de despliegue
    de IaaS", establecer una expiración de 1 año y dar clic en el botón
    ![](media/image42.png){width="0.7695909886264217in"
    height="0.20522419072615922in"} de la parte inferior de la sección.

5.  A continuación, aparecerá un Token por única vez, asegúrese de
    guardarlo con cuidado ya que es la única ocasión en que se mostrará
    el token.

### Ejecución de Agente de Team Services en VM con SQL Server

Ahora es necesario que VSTS pueda ver la máquina virtual con SQL Server
Enterprise que desplegamos previamente, para esto es necesario ingresar
al servidor por medio de RDP.

1.  Dar clic en el ícono
    ![](media/image43.png){width="0.5899376640419948in"
    height="0.2571522309711286in"} de la esquina superior izquierda para
    volver al sitio principal de la cuenta de VSTS.

2.  Clic en el proyecto DEVOPS BI Workshop.

3.  Clic en el ícono ![](media/image44.png){width="1.2089545056867892in"
    height="0.26691163604549434in"} y a continuación clic en la opción
    ![](media/image45.png){width="1.3358202099737533in"
    height="0.23336614173228346in"}.

4.  Clic en el botón ![](media/image46.png){width="1.3732884951881015in"
    height="0.2431178915135608in"}

5.  Especificar un nombre para el grupo de despliegue, cómo, por
    ejemplo: "Grupo IaaS" y en la descripción "Grupo de despliegue de
    máquinas virtuales en IaaS"

6.  A continuación, aparecerá un script en la parte derecha de la
    pantalla. En la parte inferior dar clic en el botón
    ![](media/image47.png){width="1.6567169728783901in"
    height="0.21579068241469818in"}, este script deberá ser ejecutado en
    la máquina virtual con SQL Server Enterprise que instalamos
    previamente.

7.  Conectarse por medio de RDP a la máquina virtual en Azure.

8.  Abrir una consola de PowerShell como Administrador en la máquina
    virtual y ejecutar el script que tenemos copiado en el Clipboard.

    ![](media/image48.png){width="3.3108267716535433in"
    height="2.3955227471566056in"}

    ![](media/image49.png){width="6.5in" height="1.9729166666666667in"}

9.  Esta acción desgargará el agente de VSTS en la carpeta \\vstsagent
    del servidor, cuando la consola pregunte por el método de
    autenticación, escribir PAT

10. A continuación, escribir el token que guardamos previamente, en caso
    de que no se tenga se deberá crear un nuevo token.

11. Al preguntar si queremos agregar un TAG para el agente, escribir "N"

12. Al preguntar por la cuenta de usuario que utilizará el servicio,
    presionar la tecla ENTER.

13. Finalmente la ejecución mostrará una pantalla como la siguiente:

    ![](media/image50.png){width="4.1636297025371825in"
    height="3.3196511373578304in"}

14. Comprobar que el grupo de despliegue se haya publicado de forma
    exitosa siguiendo los siguientes pasos:

    a.  Volver al sitio de VSTS

    b.  Clic en "Build and Release" y posteriormente en "Deployment
        groups"

    c.  El grupo de recurso Grupo IaaS debería tener 1 recurso Online
        como se muestra en la siguiente imagen:

        ![](media/image51.png){width="5.134328521434821in"
        height="0.7405282152230971in"}

Integración Continua
====================

Antes de que exista una sola línea de código, podemos configurar cómo
será su compilación para lograr la Integración Contínua. Para lograrlo,
cada proyecto tendrá sus configuraciones especiales.

Configurar Build de SQL Server
------------------------------

Dentro del portal de VSTS realizar lo siguiente:

1.  Dar clic en el menú
    ![](media/image52.png){width="1.2611931321084864in"
    height="0.28983923884514434in"} y a continuación clic en el botón
    ![](media/image53.png){width="0.6703969816272966in"
    height="0.24378062117235347in"}.

2.  Dar clic en el botón central
    ![](media/image54.png){width="0.8116994750656168in"
    height="0.21999343832020998in"}.

3.  En el formulario, Seleccionar VSTS Git, y en el campo Repository
    seleccionar "database\_repository".

4.  Mantener las otras opciones en Default y dar clic en el botón
    ![](media/image55.png){width="0.7238812335958005in"
    height="0.21853018372703412in"}.

5.  En la parte superior dar clic en la opción
    ![](media/image56.png){width="0.8129943132108487in"
    height="0.27658573928258967in"}.

6.  Dar clic en el ícono "+" que aparecer junto a la sección de "Agent
    job 1":

    ![](media/image57.png){width="2.843284120734908in"
    height="0.32457567804024495in"}

7.  En el filtro de búsqueda, escribir el texto "MSBuild", elegir la
    tarea ![](media/image58.png){width="1.2313429571303587in"
    height="0.3588484251968504in"} y dar clic en el botón:
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"}

8.  Nuevamente, en el filtro de búsqueda, escribir el texto "Copy",
    elegir la tarea: ![](media/image60.png){width="1.5223884514435695in"
    height="0.3492924321959755in"} y dar clic en el botón
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"}.

9.  Nuevamente, en el filtro de búsqueda, escribir el texto "Publish",
    elegir la tarea ![](media/image61.png){width="1.3059700349956256in"
    height="0.37804352580927386in"} y dar clic en el botón:
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"}

10. En el menú de la izquierda se verán las 3 tareas agregadas, en esta
    ocasión vamos a mantener la tarea "Build solution" con los
    parámetros por default.

11. Clic en la tarea:
    ![](media/image62.png){width="1.5970155293088364in"
    height="0.35896216097987754in"} del menú de la izquierda.

12. Llenar el formulario como se muestra en la siguiente imagen:

    **Source Folder:** \$(system.defaultworkingdirectory)

    **Contents:** \*\*\\bin\\Debug\\\*\*

    **Target Folder:** \$(Build.ArtifactStagingDirectory)

    ![](media/image63.png){width="2.4743482064741906in"
    height="2.8208956692913385in"}

13. Dejar la tarea ![](media/image64.png){width="1.4328357392825897in"
    height="0.28656714785651793in"} con sus parámetros por default.

14. En la barra de menú superior, dar clic en el botón "Triggers":
    ![](media/image65.png){width="3.5074628171478563in"
    height="0.34652012248468944in"}

15. Habilitar la opción "Enable continuous integration".

16. En la parte superior, dar clic en el título del Build que dice
    ![](media/image66.png){width="1.8597550306211723in"
    height="0.2255916447944007in"}, y editar el nombre del Build por un
    nombre más descriptivo como por ejemplo "Database\_Build"

17. Clic en el combo superior:
    ![](media/image67.png){width="1.1044772528433946in"
    height="0.2162609361329834in"} y dar clic en la opción:
    ![](media/image68.png){width="0.7373184601924759in"
    height="0.23196522309711287in"}

18. Escribir un comentario para el cambio que acabamos de realizar, por
    ejemplo: "Primera versión de build para base de datos" y dar clic en
    el botón "Save".

> ![](media/image69.png){width="3.6302865266841646in"
> height="2.0800142169728786in"}

Configurar Build de Integration Services
----------------------------------------

1.  Dar clic en el menú
    ![](media/image52.png){width="1.2611931321084864in"
    height="0.28983923884514434in"} y a continuación clic en el botón
    ![](media/image53.png){width="0.6703969816272966in"
    height="0.24378062117235347in"}.

2.  Como ya existe un Build previo, será necesario dar clic en el botón
    ![](media/image70.png){width="0.701492782152231in"
    height="0.233830927384077in"} y a continuación en "New build
    pipeline"

3.  En el formulario, Seleccionar VSTS Git, y en el campo Repository
    seleccionar "ssis\_repository".

4.  Mantener las otras opciones en Default y dar clic en el botón
    ![](media/image55.png){width="0.7238812335958005in"
    height="0.21853018372703412in"}.

5.  En la parte superior dar clic en la opción
    ![](media/image56.png){width="0.8129943132108487in"
    height="0.27658573928258967in"}.

6.  Dar clic en el ícono "+" que aparecer junto a la sección de "Agent
    job 1":

    ![](media/image57.png){width="2.843284120734908in"
    height="0.32457567804024495in"}

7.  En el filtro de búsqueda, escribir el texto "SSIS", elegir la tarea
    ![](media/image71.png){width="1.5373129921259843in"
    height="0.4469717847769029in"}y dar clic en el botón:
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"} (Si esta tarea no se tiene
    instalada, se deberá agregar desde el Marketplace de Visual Studio).

8.  Nuevamente, en el filtro de búsqueda, escribir el texto "Copy",
    elegir la tarea: ![](media/image60.png){width="1.5223884514435695in"
    height="0.3492924321959755in"} y dar clic en el botón:
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"}

9.  Nuevamente, en el filtro de búsqueda, escribir el texto "Publish",
    elegir la tarea ![](media/image61.png){width="1.3059700349956256in"
    height="0.37804352580927386in"} y dar clic en el botón:
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"}

10. En el menú de la izquierda se verán las 3 tareas agregadas, demos
    clic en la tarea ![](media/image72.png){width="1.5475043744531933in"
    height="0.2910444006999125in"}

11. Llenar el formulario como se muestra en la siguiente imagen:

    ![](media/image73.png){width="3.080491032370954in"
    height="5.611162510936133in"}

12. Clic en la tarea:
    ![](media/image62.png){width="1.5970155293088364in"
    height="0.35896216097987754in"} del menú de la izquierda.

13. Llenar el formulario como se muestra en la siguiente imagen:

    **Source Folder:** \$(system.defaultworkingdirectory)

    **Contents:** \*\*

    **Target Folder:** \$(Build.ArtifactStagingDirectory)

    ![](media/image74.png){width="2.2921905074365703in"
    height="2.738805774278215in"}

14. Dejar la tarea ![](media/image64.png){width="1.4328357392825897in"
    height="0.28656714785651793in"} con sus parámetros por default.

15. En la barra de menú superior, dar clic en el botón "Triggers":
    ![](media/image65.png){width="3.5074628171478563in"
    height="0.34652012248468944in"}

16. Habilitar la opción "Enable continuous integration".

17. En la parte superior, dar clic en el título del Build que dice
    ![](media/image66.png){width="1.8597550306211723in"
    height="0.2255916447944007in"}, y editar el nombre del Build por un
    nombre más descriptivo como por ejemplo "SSIS\_Build"

18. Clic en el combo superior:
    ![](media/image67.png){width="1.1044772528433946in"
    height="0.2162609361329834in"} y dar clic en la opción:
    ![](media/image68.png){width="0.7373184601924759in"
    height="0.23196522309711287in"}

19. Escribir un comentario para el cambio que acabamos de realizar, por
    ejemplo: "Primera versión de build para SSIS" y dar clic en el botón
    "Save".

> ![](media/image75.png){width="3.547093175853018in"
> height="2.033270997375328in"}

Configurar Build de Analysis Services
-------------------------------------

1.  Dar clic en el menú
    ![](media/image52.png){width="1.2611931321084864in"
    height="0.28983923884514434in"} y a continuación clic en el botón
    ![](media/image53.png){width="0.6703969816272966in"
    height="0.24378062117235347in"}.

2.  Como ya existe un Build previo, será necesario dar clic en el botón
    ![](media/image70.png){width="0.701492782152231in"
    height="0.233830927384077in"} y a continuación en "New build
    pipeline".

3.  En el formulario, Seleccionar VSTS Git, y en el campo Repository
    seleccionar "ssas\_repository".

4.  Mantener las otras opciones en Default y dar clic en el botón
    ![](media/image55.png){width="0.7238812335958005in"
    height="0.21853018372703412in"}.

5.  En la parte superior dar clic en la opción
    ![](media/image56.png){width="0.8129943132108487in"
    height="0.27658573928258967in"}.

6.  Dar clic en el ícono "+" que aparecer junto a la sección de "Agent
    job 1":

    ![](media/image57.png){width="2.843284120734908in"
    height="0.32457567804024495in"}

7.  En el filtro de búsqueda, escribir el texto "MSBuild", elegir la
    tarea ![](media/image58.png){width="1.2313429571303587in"
    height="0.3588484251968504in"} y dar clic en el botón:
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"}

8.  Nuevamente, en el filtro de búsqueda, escribir el texto "Copy",
    elegir la tarea: ![](media/image60.png){width="1.5223884514435695in"
    height="0.3492924321959755in"} y dar clic en el botón
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"}.

9.  Nuevamente, en el filtro de búsqueda, escribir el texto "Publish",
    elegir la tarea ![](media/image61.png){width="1.3059700349956256in"
    height="0.37804352580927386in"} y dar clic en el botón:
    ![](media/image59.png){width="0.7795406824146982in"
    height="0.22885608048993875in"}

10. En el menú de la izquierda se verán las 3 tareas agregadas, en esta
    ocasión vamos a mantener la tarea "Build solution" con los
    parámetros por default.

11. Clic en la tarea:
    ![](media/image62.png){width="1.5970155293088364in"
    height="0.35896216097987754in"} del menú de la izquierda.

12. Llenar el formulario como se muestra en la siguiente imagen:

    **Source Folder:** \$(system.defaultworkingdirectory)

    **Contents:** \*\*

    **Target Folder:** \$(Build.ArtifactStagingDirectory)

> ![](media/image74.png){width="2.2921905074365703in"
> height="2.738805774278215in"}

13. Dejar la tarea ![](media/image64.png){width="1.4328357392825897in"
    height="0.28656714785651793in"} con sus parámetros por default.

14. En la barra de menú superior, dar clic en el botón "Triggers":
    ![](media/image65.png){width="3.5074628171478563in"
    height="0.34652012248468944in"}

15. Habilitar la opción "Enable continuous integration".

16. En la parte superior, dar clic en el título del Build que dice
    ![](media/image66.png){width="1.8597550306211723in"
    height="0.2255916447944007in"}, y editar el nombre del Build por un
    nombre más descriptivo como por ejemplo "SSAS\_Build"

17. Clic en el combo superior:
    ![](media/image67.png){width="1.1044772528433946in"
    height="0.2162609361329834in"} y dar clic en la opción:
    ![](media/image68.png){width="0.7373184601924759in"
    height="0.23196522309711287in"}

18. Escribir un comentario para el cambio que acabamos de realizar, por
    ejemplo: "Primera versión de build para SSAS" y dar clic en el botón
    "Save".

> ![](media/image76.png){width="3.4104483814523183in"
> height="1.9960094050743658in"}

Despliegue Continuo
===================

A continuación, automatizaremos la instalación de los proyectos de Azure
SQL Server, SQL Server 2017 en IaaS, SQL Server Integration Services en
IaaS y SQL Analysis Services en IaaS.

Configurar Release de Azure SQL Database
----------------------------------------

1.  Clic en el menú ![](media/image77.png){width="1.2388068678915136in"
    height="0.24460520559930007in"} y a continuación clic en la opción
    ![](media/image78.png){width="0.813432852143482in"
    height="0.2475667104111986in"}.

2.  Clic en el botón ![](media/image79.png){width="1.0in"
    height="0.24in"}.

3.  Seleccionar ![](media/image80.png){width="0.7686570428696413in"
    height="0.20727799650043743in"}.

4.  En el campo Stage name colocar el texto "DEV\_DB" que indicará que
    es un ambiente de desarrollo y dar clic en cerrar en la "X" de la
    esquina superior derecha ![](media/image81.png){width="0.25in"
    height="0.22916666666666666in"}.

5.  Clic en ![](media/image82.png){width="0.8872364391951006in"
    height="0.5781342957130359in"}, en el campo Source elegir
    "Database\_Build" y dar clic en el botón:
    ![](media/image83.png){width="0.43283464566929136in"
    height="0.1442782152230971in"}

6.  Dar clic en el ícono en forma de rayo que se encuentra en la esquina
    superior derecha del artifact que acabamos de agregar
    ![](media/image84.png){width="0.5447758092738407in"
    height="0.43735564304461944in"}y habilitar el trigger de Continuous
    deployment como se muestra en la siguiente figura:

    ![](media/image85.png){width="2.2916765091863516in"
    height="0.9762128171478566in"}

7.  Dar clic en la X superior para cerrar la sección.

8.  Dar clic en el apartado "Variables" que se encuentra en el menú
    superior:

    ![](media/image86.png){width="3.238805774278215in"
    height="0.29047572178477693in"}

9.  Dar clic en el ícono
    ![](media/image87.png){width="0.5982895888013998in"
    height="0.2527985564304462in"}.

10. Vamos a crear una variable para guardar la contraseña de la base de
    datos de forma segura.

    a.  En el campo Name colocar el nombre de la variable "passdb"

    b.  En el campo Value colocar la contraseña de la base de datos con
        la que se construyó previamente.

    c.  Dar clic en ícono en forma de candado que se encuentra en el
        extremo derecho del campo Value.

    d.  Verificar que la variable se haya guardado de forma exitosa y
        enmascarada.

        ![](media/image88.png){width="5.649254155730533in"
        height="0.5456113298337708in"}

11. Regresar a la configuración de Pipeline del release al dar clic en
    el apartado "Pipeline" del menú superior:

    ![](media/image89.png){width="3.544775809273841in"
    height="0.3099693788276465in"}

12. Dar clic en el pequeño link que aparece en el Stage DEV\_DB donde
    está escrito "1 job, 0 task"

    ![](media/image90.png){width="2.6770833333333335in" height="1.0in"}

13. Dar clic en el símbolo "+" que aparece en la sección "Agent job".

14. En el filtro de búsqueda de la derecha, escribir el texto "SQL".

15. Buscar la tarea ![](media/image91.png){width="1.5in"
    height="0.39189195100612423in"} y dar clic en el botón
    ![](media/image92.png){width="0.783581583552056in"
    height="0.20620516185476814in"}

16. Dar clic en la tarea recién agregada "Azure SQL Publish", se abrirá
    un formulario del lado derecho.

17. En el combo "Azure Subscription" seleccionar el Service Connection
    que creamos previamente llamado "Azure Default Connection".

18. En el campo "Azure SQL Server Name" colocar el nombre del servidor
    SQL en plataforma que creamos previamente, para obtenerlo, debemos
    entrar al portal de Azure, seleccionar la base de datos que creamos,
    seleccionar la opción "overview" y copiar el texto debajo de la
    sección "Server name", en este ejemplo se trata del texto
    "db-devops.database.windows.net"

    ![](media/image93.png){width="6.5in" height="2.061111111111111in"}

19. En el campo "Database Name", colocar el nombre de la base de datos
    que creamos previamente, en este ejemplo se trata de "DEVOPS-DEV"

20. En el campo "Server Admin Login", colocar el nombre de usuario que
    le dieron al servidor de la base de datos cuando fue creada, en este
    ejemplo se trata del texto "workshopAdmin".

21. En el campo "Password" colocar el texto (sin comillas) "\$(passdb)"
    que hace referencia a la variable con la contraseña de la base de
    datos que creamos previamente.

22. En el campo DACPAC File, colocar el siguiente texto: \*\*\\\*.dacpac

23. Dar clic en el título de la parte superior que dice "New Release
    pipeline" y modificar el título con un texto descriptivo que diga
    "Database Release PaaS"

24. Dar clic en el botón
    ![](media/image94.png){width="0.5597014435695538in"
    height="0.25440944881889765in"} que se encuentra en la esquina
    superior derecha.

25. Colocar un comentario descriptivo del cambio realizado y dar clic en
    el botón "OK".

Configurar Release de SQL Server en IaaS
----------------------------------------

1.  Clic en el menú ![](media/image77.png){width="1.2388068678915136in"
    height="0.24460520559930007in"} y a continuación clic en la opción
    ![](media/image78.png){width="0.813432852143482in"
    height="0.2475667104111986in"}.

2.  Clic en el botón ![](media/image95.png){width="0.3854166666666667in"
    height="0.3020833333333333in"} y a continuación en la opción "Create
    a reléase pipeline"

3.  Seleccionar ![](media/image80.png){width="0.7686570428696413in"
    height="0.20727799650043743in"}.

4.  En el campo Stage name colocar el texto "DEV\_DB" que indicará que
    es un ambiente de desarrollo y dar clic en cerrar en la "X" de la
    esquina superior derecha ![](media/image81.png){width="0.25in"
    height="0.22916666666666666in"}.

5.  Clic en ![](media/image82.png){width="0.8872364391951006in"
    height="0.5781342957130359in"}, en el campo Source elegir
    "Database\_Build" y dar clic en el botón:
    ![](media/image83.png){width="0.43283464566929136in"
    height="0.1442782152230971in"}

6.  Dar clic en el ícono en forma de rayo que se encuentra en la esquina
    superior derecha del artifact que acabamos de agregar
    ![](media/image84.png){width="0.5447758092738407in"
    height="0.43735564304461944in"}y habilitar el trigger de Continuous
    deployment como se muestra en la siguiente figura:

    ![](media/image85.png){width="2.2916765091863516in"
    height="0.9762128171478566in"}

7.  Dar clic en la X superior para cerrar la sección.

8.  Dar clic en el apartado "Variables" que se encuentra en el menú
    superior:

    ![](media/image86.png){width="3.238805774278215in"
    height="0.29047572178477693in"}

9.  Dar clic en el ícono
    ![](media/image87.png){width="0.5982895888013998in"
    height="0.2527985564304462in"}.

10. Vamos a crear una variable para guardar la contraseña de la base de
    datos de forma segura.

    a.  En el campo Name colocar el nombre de la variable "passdb"

    b.  En el campo Value colocar la contraseña de la base de datos con
        la que se construyó previamente.

    c.  Dar clic en ícono en forma de candado que se encuentra en el
        extremo derecho del campo Value.

    d.  Verificar que la variable se haya guardado de forma exitosa y
        enmascarada.

        ![](media/image88.png){width="5.649254155730533in"
        height="0.5456113298337708in"}

11. Regresar a la configuración de Pipeline del release al dar clic en
    el apartado "Pipeline" del menú superior:

    ![](media/image89.png){width="3.544775809273841in"
    height="0.3099693788276465in"}

12. Dar clic en el pequeño link que aparece en el Stage DEV\_DB donde
    está escrito "1 job, 0 task"

    ![](media/image90.png){width="2.6770833333333335in" height="1.0in"}

13. Dar clic en la tarea
    ![](media/image96.png){width="0.8061734470691163in"
    height="0.24185258092738407in"} y en la sección que se abre en la
    derecha clic en ![](media/image97.png){width="0.6716415135608049in"
    height="0.1883869203849519in"}. Debido a que esta es una instalación
    en una máquina virtual en IaaS, daremos de alta una sección para
    agregar el grupo de despliegue configurado previamente.

14. En la sección ![](media/image98.png){width="1.7537314085739282in"
    height="0.28113298337707787in"}, dar clic en los 3 puntos que
    aparecen a la derecha
    ![](media/image99.png){width="0.2916666666666667in"
    height="0.20833333333333334in"} y a continuación "Add a deployment
    Group job".

15. En la derecha se abrirá un formulario, buscar el combo "Deployment
    group" y elegir el grupo de despliegue "Grupo IaaS", dejar las
    siguientes opciones con los valores por default.

16. Dar clic en el símbolo "+" que aparece en la sección "Deployment
    Group job".

17. En el filtro de búsqueda de la derecha, escribir el texto "SQL".

18. Buscar la tarea ![](media/image100.png){width="1.5951465441819772in"
    height="0.3358202099737533in"} y dar clic en el botón
    ![](media/image92.png){width="0.783581583552056in"
    height="0.20620516185476814in"}

19. Dar clic en la tarea recién agregada "Deploy using : dacpac", se
    abrirá un formulario del lado derecho.

20. En el campo "DACPAC File" colocar el texto: \*\*\\\*.dacpac

21. En el campo "Sever Name" colocar el texto: localhost

22. En el campo "Database Name" colocar el texto: DEVOPS-DEV

23. En el campo "Authentication" colocar la opción "Windows
    Authentication"

24. Dar clic en el título de la parte superior que dice "New Release
    pipeline" y modificar el título con un texto descriptivo que diga
    "Database Release IaaS"

25. Dar clic en el botón
    ![](media/image94.png){width="0.5597014435695538in"
    height="0.25440944881889765in"} que se encuentra en la esquina
    superior derecha.

26. Colocar un comentario descriptivo del cambio realizado y dar clic en
    el botón "OK".

Configurar Release de Integration Services en IaaS
--------------------------------------------------

1.  Clic en el menú ![](media/image77.png){width="1.2388068678915136in"
    height="0.24460520559930007in"} y a continuación clic en la opción
    ![](media/image78.png){width="0.813432852143482in"
    height="0.2475667104111986in"}.

2.  Clic en el botón ![](media/image95.png){width="0.3854166666666667in"
    height="0.3020833333333333in"} y a continuación en la opción "Create
    a reléase pipeline"

3.  Seleccionar ![](media/image80.png){width="0.7686570428696413in"
    height="0.20727799650043743in"}.

4.  En el campo Stage name colocar el texto "DEV\_DB" que indicará que
    es un ambiente de desarrollo y dar clic en cerrar en la "X" de la
    esquina superior derecha ![](media/image81.png){width="0.25in"
    height="0.22916666666666666in"}.

5.  Clic en ![](media/image82.png){width="0.8872364391951006in"
    height="0.5781342957130359in"}, en el campo Source elegir
    "SSIS\_Build" y dar clic en el botón:
    ![](media/image83.png){width="0.43283464566929136in"
    height="0.1442782152230971in"}

6.  Dar clic en el ícono en forma de rayo que se encuentra en la esquina
    superior derecha del artifact que acabamos de agregar
    ![](media/image84.png){width="0.5447758092738407in"
    height="0.43735564304461944in"}y habilitar el trigger de Continuous
    deployment como se muestra en la siguiente figura:

    ![](media/image85.png){width="2.2916765091863516in"
    height="0.9762128171478566in"}

7.  Dar clic en la X superior para cerrar la sección.

8.  Dar clic en el apartado "Variables" que se encuentra en el menú
    superior:

    ![](media/image86.png){width="3.238805774278215in"
    height="0.29047572178477693in"}

9.  Dar clic en el ícono
    ![](media/image87.png){width="0.5982895888013998in"
    height="0.2527985564304462in"}.

10. Vamos a crear una variable para guardar la contraseña de la base de
    datos de forma segura.

    a.  En el campo Name colocar el nombre de la variable "passdb"

    b.  En el campo Value colocar la contraseña de la base de datos con
        la que se construyó previamente.

    c.  Dar clic en ícono en forma de candado que se encuentra en el
        extremo derecho del campo Value.

    d.  Verificar que la variable se haya guardado de forma exitosa y
        enmascarada.

        ![](media/image88.png){width="5.649254155730533in"
        height="0.5456113298337708in"}

11. Regresar a la configuración de Pipeline del release al dar clic en
    el apartado "Pipeline" del menú superior:

    ![](media/image89.png){width="3.544775809273841in"
    height="0.3099693788276465in"}

12. Dar clic en el pequeño link que aparece en el Stage DEV\_DB donde
    está escrito "1 job, 0 task"

    ![](media/image90.png){width="2.6770833333333335in" height="1.0in"}

13. Dar clic en la tarea
    ![](media/image96.png){width="0.8061734470691163in"
    height="0.24185258092738407in"} y en la sección que se abre en la
    derecha clic en ![](media/image97.png){width="0.6716415135608049in"
    height="0.1883869203849519in"}. Debido a que esta es una instalación
    en una máquina virtual en IaaS, daremos de alta una sección para
    agregar el grupo de despliegue configurado previamente.

14. En la sección ![](media/image98.png){width="1.7537314085739282in"
    height="0.28113298337707787in"}, dar clic en los 3 puntos que
    aparecen a la derecha
    ![](media/image99.png){width="0.2916666666666667in"
    height="0.20833333333333334in"} y a continuación "Add a deployment
    Group job".

15. En la derecha se abrirá un formulario, buscar el combo "Deployment
    group" y elegir el grupo de despliegue "Grupo IaaS", dejar las
    siguientes opciones con los valores por default.

16. Dar clic en el símbolo "+" que aparece en la sección "Deployment
    Group job".

17. En el filtro de búsqueda de la derecha, escribir el texto "SSIS".

18. Buscar la tarea ![](media/image101.png){width="1.8582086614173228in"
    height="0.3271008311461067in"} y dar clic en el botón
    ![](media/image92.png){width="0.783581583552056in"
    height="0.20620516185476814in"}

19. Dar clic en la tarea recién agregada "Deploy SSIS", se abrirá un
    formulario del lado derecho.

20. En el campo "Path to .ispac file" colocar el texto:
    \$(System.DefaultWorkingDirectory)\\\*\*\\\*.ispac

21. En el campo "Name of the SSIS catalog" colocar el texto: SSISDB

22. Seleccionar la opción "Create new Catalog if doesn't exist"

23. En el campo "New Catalog Password" colocar el texto: \$(passdb)

24. En el campo "Name of the SSIS catalog folder" colocar el texto:
    DevOpsDemoFolder

25. En el campo "Name of the SQL Server hosting the SSIS catalog
    database" colocar el texto: localhost

26. En el campo "Integrated Security" seleccionar la opción "SSPI
    (Windows Authentication)"

    e.  *El instalador tiene un pequeño bug al utilizar la opción SSPI.
        Es necesario elegir elegir la opción "Custom Connection String",
        colocar cualquier texto y nuevamente seleccionar la opción "SSPI
        (Windows Authentication)"*

27. Dar clic en el título de la parte superior que dice "New Release
    pipeline" y modificar el título con un texto descriptivo que diga
    "SSIS Release IaaS"

28. Dar clic en el botón
    ![](media/image94.png){width="0.5597014435695538in"
    height="0.25440944881889765in"} que se encuentra en la esquina
    superior derecha.

29. Colocar un comentario descriptivo del cambio realizado y dar clic en
    el botón "OK".

Configurar Release de Analysis Services en IaaS
-----------------------------------------------

1.  Clic en el menú ![](media/image77.png){width="1.2388068678915136in"
    height="0.24460520559930007in"} y a continuación clic en la opción
    ![](media/image78.png){width="0.813432852143482in"
    height="0.2475667104111986in"}.

2.  Clic en el botón ![](media/image95.png){width="0.3854166666666667in"
    height="0.3020833333333333in"} y a continuación en la opción "Create
    a release pipeline"

3.  Seleccionar ![](media/image80.png){width="0.7686570428696413in"
    height="0.20727799650043743in"}.

4.  En el campo Stage name colocar el texto "DEV\_DB" que indicará que
    es un ambiente de desarrollo y dar clic en cerrar en la "X" de la
    esquina superior derecha ![](media/image81.png){width="0.25in"
    height="0.22916666666666666in"}.

5.  Clic en ![](media/image82.png){width="0.8872364391951006in"
    height="0.5781342957130359in"}, en el campo Source elegir
    "SSAS\_Build" y dar clic en el botón:
    ![](media/image83.png){width="0.43283464566929136in"
    height="0.1442782152230971in"}

6.  Dar clic en el ícono en forma de rayo que se encuentra en la esquina
    superior derecha del artifact que acabamos de agregar
    ![](media/image84.png){width="0.5447758092738407in"
    height="0.43735564304461944in"}y habilitar el trigger de Continuous
    deployment como se muestra en la siguiente figura:

    ![](media/image85.png){width="2.2916765091863516in"
    height="0.9762128171478566in"}

7.  Dar clic en la X superior para cerrar la sección.

8.  Dar clic en el apartado "Variables" que se encuentra en el menú
    superior:

    ![](media/image86.png){width="3.238805774278215in"
    height="0.29047572178477693in"}

9.  Dar clic en el ícono
    ![](media/image87.png){width="0.5982895888013998in"
    height="0.2527985564304462in"}.

10. Vamos a crear una variable para guardar la contraseña de la base de
    datos de forma segura.

    a.  En el campo Name colocar el nombre de la variable "passdb"

    b.  En el campo Value colocar la contraseña de la base de datos con
        la que se construyó previamente.

    c.  Dar clic en ícono en forma de candado que se encuentra en el
        extremo derecho del campo Value.

    d.  Verificar que la variable se haya guardado de forma exitosa y
        enmascarada.

        ![](media/image88.png){width="5.649254155730533in"
        height="0.5456113298337708in"}

11. Regresar a la configuración de Pipeline del release al dar clic en
    el apartado "Pipeline" del menú superior:

    ![](media/image89.png){width="3.544775809273841in"
    height="0.3099693788276465in"}

12. Dar clic en el pequeño link que aparece en el Stage DEV\_DB donde
    está escrito "1 job, 0 task"

    ![](media/image90.png){width="2.6770833333333335in" height="1.0in"}

13. Dar clic en la tarea
    ![](media/image96.png){width="0.8061734470691163in"
    height="0.24185258092738407in"} y en la sección que se abre en la
    derecha clic en ![](media/image97.png){width="0.6716415135608049in"
    height="0.1883869203849519in"}. Debido a que esta es una instalación
    en una máquina virtual en IaaS, daremos de alta una sección para
    agregar el grupo de despliegue configurado previamente.

14. En la sección ![](media/image98.png){width="1.7537314085739282in"
    height="0.28113298337707787in"}, dar clic en los 3 puntos que
    aparecen a la derecha
    ![](media/image99.png){width="0.2916666666666667in"
    height="0.20833333333333334in"} y a continuación "Add a deployment
    Group job".

15. En la derecha se abrirá un formulario, buscar el combo "Deployment
    group" y elegir el grupo de despliegue "Grupo IaaS", dejar las
    siguientes opciones con los valores por default.

16. Dar clic en el símbolo "+" que aparece en la sección "Deployment
    Group job".

17. En el filtro de búsqueda de la derecha, escribir el texto "Copy".

18. Buscar la tarea ![](media/image102.png){width="1.470148731408574in"
    height="0.31803258967629044in"} y dar clic en el botón
    ![](media/image92.png){width="0.783581583552056in"
    height="0.20620516185476814in"}

19. Nuevamente en el filtro de búsqueda escribir el texto "Powershell",
    buscar la tarea ![](media/image103.png){width="1.4925371828521434in"
    height="0.38250984251968506in"} y dar clic en el botón
    ![](media/image92.png){width="0.783581583552056in"
    height="0.20620516185476814in"}

20. Dar clic en la tarea recién agregada "Copy Files to:", se abrirá un
    formulario del lado derecho.

21. En el campo "Source Folder" colocar el texto:
    \$(System.DefaultWorkingDirectory)\\\*\*\\bin

22. En el campo "Contents" colocar el texto: \*\*

23. El el campo "Target Folder" colocar el texto: c:\\SSASReleases\\

24. En el menú de la izquierda dar clic en la tarea:
    ![](media/image104.png){width="1.2313429571303587in"
    height="0.2682819335083115in"}

25. En la sección Type, seleccionar la opción "Inline"

26. En el área de texto "Script" colocar el siguiente texto:

    Write-Host \"Implementando base de datos tabular\"

    Microsoft.AnalysisServices.Deployment.exe
    C:\\SSASReleases\\Model.asdatabase /s

27. Dar clic en el título de la parte superior que dice "New Release
    pipeline" y modificar el título con un texto descriptivo que diga
    "SSAS Release IaaS"

28. Dar clic en el botón
    ![](media/image94.png){width="0.5597014435695538in"
    height="0.25440944881889765in"} que se encuentra en la esquina
    superior derecha.

29. Colocar un comentario descriptivo del cambio realizado y dar clic en
    el botón "OK".
