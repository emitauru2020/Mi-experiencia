# Guia para el desarrollo en Odoo
por [Emanuel da Silva](https://www.linkedin.com/in/emanuel-da-silva-5487a72a9/)

## Intro
La idea es registrar las diferentes experiencias afrontadas en Odoo:

## Objetivo
Tratar de unificar en un solo lugar los diferentes documentos que se realizaron para el desarrollo en Odoo en los cuales estuve involucrado:

  1. Mejores prácticas para el desarrollo en Odoo.
  2. Ejemplos de como resolver casos especificos.
  3. Novedades del lenguaje.

## Tabla de Contenidos

  1. [Armado de ambiente](#Armado-de-ambiente)
  2. [Creación de nombres](#Creación-de-nombres)
  3. [Herencia desde Modulos diferentes](#Herencia-desde-Modulos-diferentes)

## Armado de ambiente

  <a name="campo"></a><a name="1.1"></a>

  - [Estructura de Carpetas](#campo) Se recomienda que estas tengan la siguiente estructura:
La contenedora con el nombre de la versión de Odoo por ejemplo "17.0" y dentro de ella otras tres carpetas con la siguiente nomenclatura, la primera "Odoo Server" con el código fuente de Odoo, la segunda "projects" donde se alojan los proyectos en esta versión y por último "Venv" donde estarán los diferentes entornos virtuales de los proyectos.

  - [Clonar Odoo Server](#campo) Primero accedemos al git donde se alojan las versiones de Odoo https://github.com/odoo/odoo y copiamos el enlace para descargarlo por consola en nuestro equipo. Abriendo una consola en la carpeta que va a contener el código fuente de Odoo podemos usar el siguiente código para descargar la última versión actualizada, sustituimos "url" por la que copiamos del git de Odoo y 17.0 por la version que desamos obtener.
      ```javascript
        git clone url --depth 1 --branch 17.0
      ```

  - [Pycharm](#campo) Herramientas para complementar el desarrollo


**[Volver al inicio](#tabla-de-contenidos)**

## Creación de nombres

  <a name="campo"></a><a name="1.1"></a>
  - [1.1](#campo) Se debe ser descriptivo con los nombres de los campos. El nombre debe ser autodescriptivo.

    ```javascript
    // mal
    Campo: licenf

    // bien
    Campo: licencia_por_enfermedad
    ```

  <a name="segundo"></a><a name="1.2"></a>
  - [1.2](#segundo) Utilizar PascalCase al nombrar objetos, atributos y variables.

    ```javascript
    // mal
    insertcustomer

    // bien
    InsertCustomer
    ```

  <a name="naming--leading-underscore"></a><a name="1.3"></a>
  - [1.3](#naming--leading-underscore) No utilizar underscore al inicio o final en ningún tipo de objeto, atributo o variable.
    > Esto puede hacer suponer a un programador proveniente de otros lenguajes que tiene algún significado de privacidad.

    ```javascript
    // mal
    &_CusName = "John Doe"
    &CusName_ = "John Doe"
    Proc: _InsertCustomer

    // bien
    &CustomerName = "John Doe"
    ```

  <a name="naming-enums"></a><a name="1.4"></a>
  - [1.4](#naming-enums) Nombrar los dominios enumerados sin abreviar, comenzando con la entidad en singular y siguiendo con el calificador enumerado también en singular. Los valores enumerados también se deben especificar en singular. Es importante que no tenga el mismo nombre que un atributo, por lo tanto si queremos capturar tipos de documento, necesitamos especificar un poco mas la categorizacion para no tener un dominio DocumentType y un atributo DocumentType. Se podria por ejemplo llamar al dominio OrderType (sale order, purchase order) y al atributo DocumentOrderType
	> Esto es para facilitar la definición de atributos y variables basadas en un dominio (GeneXus lo hará automáticamente).

    ```javascript
    // mal
    TypeOrder
    OrderTypes
    DocumentType si tenemos un atributo DocumentType 

    // bien
    OrderType { Sale, Purchase, etc}   - y el atributo seria DocumentOrderType
    ```

  <a name="naming-procs"></a><a name="1.5"></a>
  - [1.5](#naming-procs) Nombrar procedimientos relacionados mediante Acción Entidad + Atributo(depende el caso) + Complemento.
	> Algunas acciones típicas son Get, Set, Load (para SDT), Insert, Update, Delete, etc. La diferencia entre Set y Update es que Set refiere a un atributo y Update a una entidad.

    ```javascript
    // mal
      CustomerInsert
      Customerpsert
    

    // bien
      InsertCustomer
      UpsertCustomer
      DeleteCustomer
      GetCustomerName
      SetCustomerName
      RecalcDiscount
      DoCheckout
	  ```

  <a name="naming-gik"></a><a name="1.6"></a>
  - [1.6](#naming-gik) Utilizar [nomenclatura GIK](http://wiki.genexus.com/commwiki/servlet/wiki?1872,GIK) para nombrar atributos. Se pueden crear atributos sin el límite de los 3 caracteres si el nombre no supera los 20 caracteres y mejora la comprensión.
	> Estandard desde los inicios de GeneXus.

    ```javascript
    // mal
      InsCusDte
      DateInsertCustomer

    // bien
      CusInsDte

    // mejor
      CustomerInsertDate
	  ```

  <a name="naming-trns"></a><a name="1.7"></a>
  - [1.7](#naming-trns) Las transacciones deben tener el nombre de la entidad en singular.
	> Se define así porque en la comunidad GeneXus está claro que queda mejor a la hora de trabaja por ejmeplo con [Business Component](http://wiki.genexus.com/commwiki/servlet/wiki?5846,Toc%3ABusiness+Component). También es requerimiento de algunos patterns GeneXus para su correcta visualización (ej.: K2BTools).

    ```javascript
    // mal
      Trn:Products
      Trn:Customers

    // bien
      Trn:Product
      Trn:Customer
	  ```
