# Guia para el desarrollo en Odoo

por [Emanuel da Silva](https://www.linkedin.com/in/emanuel-da-silva-5487a72a9/)

## Intro

La idea es registrar las diferentes experiencias afrontadas en Odoo:

## Objetivo

Tratar de unificar en un solo lugar los diferentes documentos que se realizaron para el desarrollo en Odoo en los cuales
estuve involucrado:

1. Programas para el desarrollo.
2. Mejores prácticas al escribir código.
3. Ejemplos de como resolver casos especificos.

## Tabla de Contenidos

- [Guia para el desarrollo en Odoo](#guia-para-el-desarrollo-en-odoo)
  - [Armado de ambiente](#armado-de-ambiente)
  - [Estructura de Carpetas](#estructura-de-carpetas)
  - [Clonar Odoo Server](#clonar-odoo-server)
  - [Pycharm](#pycharm)
  - [PostgreSQL](#postgresql)
  - [Instalar Python](#instalar-python)
  - [Entornos Virtuales](#entornos-virtuales)
  - [Configurar Run Debug](#configurar-run-debug)
- [Creación de nombres](#creación-de-nombres)
- [Herencia desde Modulos diferentes](#herencia-desde-modulos-diferentes)
- [Ciclos para Migración](#ciclos-para-migración)

## Armado de ambiente

## Estructura de Carpetas

- **1.1** Se recomienda que estas tengan la siguiente estructura:
  La contenedora con el nombre de la versión de Odoo por ejemplo "17.0" y dentro de ella otras tres carpetas con la siguiente nomenclatura, la primera "Odoo Server" con el código fuente de Odoo, la segunda "projects" donde se alojan los proyectos en esta versión y por último "Venv" donde estarán los diferentes entornos virtuales de los proyectos.

## Clonar Odoo Server

- **1.2** Primero accedemos al git donde se alojan las versiones de [Odoo](https://github.com/odoo)odoo y copiamos el enlace para descargarlo por consola en nuestro equipo. Abriendo una consola en la carpeta que va a contener el código fuente de Odoo podemos usar el siguiente código para descargar la última versión actualizada, sustituimos "url" por la que copiamos del git de Odoo y 17.0 por la version que desamos obtener.

    ```text
      git clone url --depth 1 --branch 17.0

      //En caso de necesitar certificado ssl se puede saltear con el sguiente comando -c http.sslVerify=false
      git clone -c http.sslVerify=false url --depth 1 --branch 17.0
    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Pycharm

- **1.3** Descarga e instalación de Pycharm
  - Accedemos a la página y descargamos el [Professional](https://www.jetbrains.com/pycharm).
  - Instalamos el programa en nuestro equipo sin ejecutarlo.
  - Creamos una carpeta en nuestro equipo la cual no se podra mover y descargamos los archivos de este sitio [jetbra](https://3.jetbra.in), no ceramos la url ya que vamos a volver por el codigo de activación.
  - En la carpeta que creamos abrimos el archivo readme.txt y copiamos la siguiente linea -javaagent: /path/to/ja-netfilter.jar=jetbrains.
  - Donde esta instlalado Pycharm en la carpeta bin abrimos el documento pycharm64.vmoptions y agregamos esta linea que copiamos.
  - Pegamos la línea: -javaagent:/path/to/ja-netfilter.jar=jetbrains al final.
  - Volvemos a la carpeta donde donde alojamos los archivos y copiamos la direccion del archivo
      ja-netfilter.jar
  - De nuevo en el documento pycharm64.vmoptions sustituimos path/to por por la dirección del archivo que copiamos en el paso anterior.

      ```text
        //Linux
        -javaagent:/home/user/Escritorio/pch/ja-netfilter.jar=jetbrains
        //Windows
        -javaagent:"C:\Users\emanu\Documents\PycharmCrack\jetbra\ja-netfilter.jar"=jetbrains
      ```

  - **Linux**
    - En la carpeta donde estan los archivos descargados, ingresamos a la carpeta scripts y abrimos un terminal, ingresamos sudo su para tener los permisos y ejecutamos el install.sh con el comando: sh install.sh y luego reiniciamos.

  - **Windows**
    - En la carpeta donde estan los archivos descargados ejecutamos Install-all-user.vbs
    - Ejecutamos el Pycharm y seleccinamos la prueba por 30 días.
    - Vamos a la url donde se descargaron los archivos y buscamos con Ctrl+f pycharm y copiamos el texto de la licencia.
    - En el Pycharm vamos a help, register, seleccionamos activar por código y pegamos.
    - Por último seleccionamos Activar.
    - Se puede aumentar RAM dedicada al ide, para ello abrimos el archivo pycharm64.vmoptions y modificamos la memoria min y max.

      ```text
        -Xms512m
        -Xmx4096m
      ```

**[Volver al inicio](#tabla-de-contenidos)**

## PostgreSQL

- **1.4** Descarga e instalación de PostgreSQL
- Accedemos a la página y descargamos el [PostgreSQL](https://www.postgresql.org/download) y lo instalamos.
- En la istalacion nos pide que ingresemos un password para el usuario "postgres" que sera el master de la DB y por defecto asigna el numero de puerto 5432, en caso de modificar este recordar asignar ese puerto en el archivo .conf.
- Al finalizar la instalación debemos detener el servicio ya que necesitamos que se ejecute a través del ide.
  - **Linux**
    - Detener el servicio para después quitar de la lista de ejecutados automáticos.

    ```text
        sudo service odoo stop
        sudo systemctl disable odoo
    ```

  - **Windows** Accedemos a Servicios, buscamos "PostgreSQL" y deshabilitamos el inicio automático.
    - Luego creamos un usuario con los permisos a la base iguales al usuario postgres que se crea por defecto al instalar, esto es necesario ya que en el archivo .conf de odoo debemos configurar el db_user y este no puede ser el usuario postgres.

      ```text
        //Crear usuario
        CREATE USER username WITH PASSWORD 'MYPASS';
        //Darle permisos de super
        ALTER USER odoo WITH CREATEDB CREATEROLE REPLICATION BYPASSRLS;
      ```

**[Volver al inicio](#tabla-de-contenidos)**

## Instalar Python

- **1.5** Descarga e instalación de Python
  - Se descarga e instala Python en la versión que se necesite.
  - En el caso de Windows se debe agregar a las variables de entorno.
  - Luego de instalada verificar que este instalada por medio de una consulta en cosola.

      ```text
        //Python 3.10
        https://www.python.org/downloads/release/python-3100/
      ```

**[Volver al inicio](#tabla-de-contenidos)**

## Entornos Virtuales

- **1.6** Creación de los entornos virtuales
  - Es recomendable siempre crear el entorno virtual separado del python que se esté usando ya que las librerías quedan instaladas solo para este ambiente de desarrollo en un python virtual y no para el python instalado.
  - Luego de abrir o crear el proyecto en Pycharm debemos crear un interprete de Python.
  - En Pycharm vamos a Settings > Project > Python Interpreter.
  - Se considera buena practica crear una carpeta que contendrá el entorno virtual de este proyecto en la carpeta "Venv" de la estructura que se creó en **(#Estructura-de-Carpetas)**.
  - En la pantalla de Python Interrpreter seleccionamos Add interpreter > Add Local Interrpreter.
  - Se abre una nueva ventana en la cual agregamos la carpeta que se crea para este proyecto dentro de "Venv" y seleccionamos el python.exe del Python que deseamos sea el interprete del proyecto.
  - Luego de esto abrimos una consola en el Pycharm y ubicamos en la carpeta creada para luego. ejecutar el comando que instala las librerías con la direccion del archvo requirements.txt.
  - Este archivo está ubicado donde se encuentra el código Odoo fuente en la carpeta odoo.

      ```text
        //Ejecutamos el siguiente comando en la cosola
        pip install -r "C:\Develop\17.0\Odoo_server\odoo\requirements.txt"
      ```

  - En un momento de la instalación puede salir un mensaje para que nos pide que actualicemos el pip en ese momento ejecutamos la línea que nos sugiere y continuamos.

      ```text
        //Ejecutamos el siguiente comando en la cosola
        python.exe -m pip install --upgrade pip
      ```

    - **Windows**
      - Puede dar el siguiente error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools.
      - Se puede descargar de esta [url](https://visualstudio.microsoft.com/es/visual-cpp-build-tools).
      - Es importante que seleccionemos en el instalador el Desktop development with C++ y en el menu que nos muestra en el lateral agregamos " Windows 11 SDK " o  " Windows 10 SDK " segun corresponda.
      - Podemos agregar a la librería del proyecto el código de Odoo ya que nos sirve de ayuda para buscar código de ejemplo y ver los diferentes módulos que existen en el código fuente de Odoo.
      - Seleccionamos nuestro intérprete, seleccionamos el icono de las carpetas llamado Interpreter Path, luego el signo de + y agregamos la carpeta Odoo

**[Volver al inicio](#tabla-de-contenidos)**

## Configurar Run Debug

- **1.7** En Pycharm podemos correr localmente nuestro ambiente para ver los cambios o hacer debug
  - Primero debemos disponer de un archivo .conf donde se encuentran el codigo de configuracion de nuestro ambiente local.
  - Este se puede alojar en la carpeta de nuestro proyecto, en esta url podemos encontrar ejemplos de que comandos agregar [conf](https://gist.github.com/Guidoom/d5db0a76ce669b139271a528a8a2a27f)
  - Luego de que tenemos nuestro archivo .conf desde la parte superior del IDE en la flechita que se
      encuentra en Current File > Edit Configurations accedemos a el y se abrira una ventana.
  - Seleccionamos + y nos muestra una lista de la cual seleccionamos Python.
  - Se abre una ventana y en ella debe quedar seleccionado las siguientes opciones:
    - Creamos un nombre.
    - Debe quedar seleccionado el Python del venv que creamos.
    - En script seleccionamos el odoo.bin que se encuentra en la carpeta odoo del código fuente.
    - En script parameters debe quedar -c y la dirección de acceso a nuestro archivo conf.

      ```text
            -c "C:\Proyectos\bps\server-config.conf"
      ```

    - Por último seleccionamos Run para correr el ambiente local.
    - En caso de no haber errores, nos dirigimos al navegador y accedemos a la url localhost:8080.
    - Seguimos los pasos que nos muestra para crear la DB.
    - En el archivo .conf en el addons_path agregamos las carpetas de nuestro proyecto para que estos puedan ser instalados como aplicaciones, tambien agregamos las carpetas addons y odoo del codigo fuente de odoo.

**[Volver al inicio](#tabla-de-contenidos)**

## Creación de nombres

**1.1** Se debe ser descriptivo con los nombres de los campos. El nombre debe ser autodescriptivo.

  ```text
  mal
  Campo: licenf

  // bien
  Campo: licencia_por_enfermedad
  ```

## Herencia desde Modulos diferentes

## Ciclos para Migración

**1.1** Hay momentos donde se necesita migrar el código de versiones viejas a nuevas, la organización del equipo
  de desarrollo y un claro procedimiento es fundamentalmente para hacer un seguimiento ordenado de todo el proceso de
  migración
**1.2** En este ejemplo se aborda lo implementado en Redmine para la migración de BPS realizada en el 2024
    **** Al asignar una petición a un desarrollador se hace en estado "En Construcción" y subestado "CT -
      Asignado para desarrollo"
    **** Cuando se comienza el desarrollo el desarrollador pasa a subestado "CT - En desarrollo" y agrega el tag
      “Fase 1”. En “Fase 1” se migra todo el código que no genere conflictos en el siguiente orden:
        **** Migrar manifiesto y .py
        **** Migrar grupos, roles, reglas y access
        **** Migrar vistas y menús
        **** Deseable pero no requerido que se pueda instalar el addon
    **** Finalizado el paso anterior se quita el tag “Fase 1”, se agrega el tag “Fase 2”, se agrega una nota con
      los conflictos encontrados o elementos que hayan quedado pendientes y se mueve la petición al grupo “Gestión
      técnica”
    **** Se vuelve a asignar la petición a un desarrollador para completar la “Fase 2”. Esta fase consiste en
      resolver todos los conflictos encontrados en la “Fase 1”. Requerido que se pueda instalar el addon
    **** Finalizado el paso anterior se quita el tag “Fase 2”, se agrega el tag “Fase 3”, se agrega una nota con
      los conflictos que puedan permanecer y se mueve la petición al grupo “Gestión técnica”
    **** Se vuelve a asignar la petición a un desarrollador para completar la “Fase 3”. Esta fase consiste en
      comparar odoo9 vs odoo17, resultado y vistas. Este paso es acompañado por la documentación funcional
    **** Finalizado el paso anterior se quita el tag “Fase 3”, se agrega una nota con las decisiones tomadas,
      testeos realizados, en caso de ser necesario se hace el diseño técnico y se cambia el subestado a “CT – Desarrollo
      Finalizado” y se mueve la petición al grupo “Gestión técnica”
