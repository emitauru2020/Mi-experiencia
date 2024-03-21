# Guia de estilo para el desarrollo
por [Emanuel da Silva](https://www.linkedin.com/in/emanuel-da-silva-5487a72a9/)

## Intro
La presente guía se realizó buscando los siguientes objetivos:

## Objetivos
La presente guía se realizó buscando los siguientes objetivos:

  1. Transmitir las mejores prácticas a la hora de desarrollar en GeneXus.
  1. Estandarizar el código escrito. Ya que hay tantas formas de programar como programadores, se intenta simplificar la lectura del código fuente.
  1. Divulgar buenas prácticas de codificación y las novedades del lenguaje.

## Tabla de Contenidos

  1. [Creación de nombres](#Creación-de-nombres)
  1. [Segundo](#dominios-enumerados)
  1. [Tercero](#identación-y-espaciado)
  1. [Cuarto](#structured-data-types)
  1. [Buenas practicas](#structured-data-types)


## Creación de nombres

  <a name="naming--descriptive"></a><a name="1.1"></a>
  - [1.1](#naming--descriptive) Se debe ser descriptivo con los nombres.
	> Se intenta que el nombre sea autodescriptivo.

    ```javascript
    // mal
    Proc: InsCus

    // bien
    Proc: InsertCustomer
    ```

  <a name="naming--PascalCase"></a><a name="1.2"></a>
  - [1.2](#naming--PascalCase) Utilizar PascalCase al nombrar objetos, atributos y variables.

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

**[Volver al inicio](#tabla-de-contenidos)**

## Segundo
  <a name="whitespace-tab"></a><a name="2.1"></a>
  - [2.1](#whitespace-tab) Utilizar tabuladores (tab) en lugar de "espacios". De esta forma, cada uno puede visializar la cantidad de espacioes que prefiera, ya que se configura en GeneXus.
    > La identación ofrece a los desarrolladores una mejor lectura del código fuente. Si tomamos una identación estandard, facilitará al resto entednder el código fuente.

	 ```javascript
    // mal
    if &DocumentOrderType = OrderType.Sale
    msg( "Sale")
    endif

    // mal
    if &DocumentOrderType = OrderType.Sale
    		msg( "Sale")
    endif

    // bien
    if &DocumentOrderType = OrderType.Sale
        msg( "Sale")
    endif
    ```

  <a name="whitespace-where"></a><a name="2.2"></a>
  - [2.2](#whitespace-where) Se deben identar las condiciónes y comandos dentro de un for each.

	 ```javascript
    // mal
    for each Document
    where DocumentOrderType = OrderType.Sale
    ...
    endfor

    // mal
    for each Document
    defined by DocumentOrderType
    ...
    endfor

    // bien
    for each Document
        where DocumentOrderType = OrderType.Sale

        ...
    endfor
    ```
 
  <a name="whitespace-parms"></a><a name="2.4"></a>
  - [2.4](#whitespace-parms) Dejar un espacio entre parámetros.

	> Hace a la sentencia más sencilla de leer.

	 ```javascript
    // mal
    parm(in:CountryId,out:&CountryName);

    // bien
    parm(in:CountryId, out:&CountryName);

    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Tercero
  <a name="enums-use"></a><a name="3.1"></a>
  - [3.1](#enums-use) Evitar la utilización de textos/números fijos cuando pueden existir multiples valores.
    > Simplificar la lectura y no necesitar recordar el texto específico de cada opción.

    ```javascript
