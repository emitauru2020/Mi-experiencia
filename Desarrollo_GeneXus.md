# Estandares de desarrollo GeneXus

por [Emanuel da Silva](https://www.linkedin.com/in/emanuel-da-silva-5487a72a9/)

## Intro

En este documento, se presentará una serie de estándares y prácticas recomendadas que el equipo debe seguir en el
desarrollo de software. Al seguir estos estándares, se logrará una mayor coherencia en el código, se facilitará la
revisión y el mantenimiento del software.

Además, este documento servirá como una guía útil para los miembros del equipo de desarrollo, proporcionándoles un
conjunto claro de directrices que deben seguir al escribir código y trabajar en proyectos de software.

## Indice

1. [Definición de nombres para elementos de la KB](#Definicion-nombres)
2. [Código en la KB](#Código-en-la-KB)
3. [Probar tu código](#Probar-tu-código)
4. [Control de versiones](#Control-de-versiones)

## Definición de nombres para elementos de la KB

- [1.1](#campo) Atributos
    - Los nombres de los objetos deben ser descriptivos y abreviados en caso de ser necesario, siempre manteniendo una
      clara interpretación de su función.
    - Tomar como referencia [nomenclatura GIK](http://wiki.genexus.com/commwiki/servlet/wiki?1872,GIK), es un estándar
      definido por GeneXus para definir los nombres de atributos.
      > Nota: El orden de la sintaxis aplica para lenguas en las que los adjetivos se colocan después del sustantivo. Si
      el nombre del atributo se define en inglés, eso es al revés, por lo que la Categoría (el sustantivo) va al final.
  ```
  //Mal
    Numbersupplierinvoice
    Invsupnum
    PhoneCustomerNumber
    NumPhoCus

  //Bien
    SupInvNum
    CusPhoNum
  
  //Mejor
    SupplierInvoiceNumber
    CustomerPhoneNumber
  ```
    - Estilo de nomenclatura CamelCase para los nombres de objetos, el cual consiste en escribir las palabras juntas con
      la primera letra de cada una en mayúscula.
  ```
  //Mal
    supinvnum
    supplierinvoicenumber

  //Bien
    SupInvNum
    SupplierInvoiceNumber
  ```

- [1.2](#campo) Transacciones
    - Los objetos de tipo Transacción deben ser creados con el nombre de la entidad en singular, agregando una
      descripción clara y concisa para su identificación.
    - Se deben definir en singular ya que es mejor para trabajar
      con [Business Component](http://wiki.genexus.com/commwiki/servlet/wiki?5846,Toc%3ABusiness+Component). También es
      requerimiento de algunos patterns GeneXus para su correcta visualización como por ejemplo K2BTools.
  ```
  //Mal
    trn: Invoices
    trn: Countries
    trn: Suppliers

  //Bien
    trn: Invoice
    trn: Country
    trn: Supplier
  ```

- [1.3](#campo) Procedimientos
    - Para los procedimientos, se recomienda utilizar una nomenclatura que incluya la acción, la entidad y el atributo
      en su nombre. Los nombres más comunes son Get, Insert, Update, Load (SDT), Delete y Set.
  ```
  //Inserta
    proc: InsertCountry
    proc: InsCountry

  //Obtiene
    proc: GetCountryName
  
  //Setea
    proc: SetCountryName
  
  //Actualiza
    proc: UpdateCountry
  
  //Proc que borra
    proc: DeleteCountry
    proc: DelCountry
  ```  

- [1.4](#campo) Variables
    - Las Variables deben comenzar con minúscula después del & y ser lo más descriptivas posibles, lo deseable es que el
      nombre este formado por la referencia y descripcion.
  ```
  //Mal
    &CustName
    &CountryName

  //Bien
    &custName
    &countryName
  ```

- [1.5](#campo) Underscore
    - Es importante evitar el uso de guiones bajos (_) al inicio de los nombres de objetos, ya que esto puede causar
      conflictos o errores en la aplicación.
  ```
  //Mal
    &invNum_
    &_invNum
    trn: _Invoice
    proc: _GetCountryName

  //Bien
    &invNum
    trn: Invoice
    proc: GetCountryName
  ```

- [1.6](#campo) MasterPage
    - Para los elementos Master page, se debe usar las letras MP al inicio del nombre, seguidas del nombre del objeto
      correspondiente.
  > Ejemplo de nombre para Master page: MPHome


- [1.7](#campo) WebPanel
    - Para los Web Panels, se debe usar el nombre del objeto correspondiente y al final se deben añadir las letras WP
      como referencia.
  > Ejemplo de nombre para WebPanel: ContactWP


- [1.8](#campo) Tipos de datos estructurados (SDT)
    - Los Structured Data Type deben nombrarse con las primeras tres letras SDT en mayúscula y la referencia del objeto
      del cual va a manejar.
  ```
  //Ejemplo
    SDTUser
    SDTSeller
    SDTCustomer
  ```

- [1.9](#campo) Data Providers (DP)
    - Los Data Providers deben ser nombrados con las primeras dos letras DP y la referencia del objeto.
  ```
  //Ejemplo
    DPSlideMenu
    DPInvoiceComposition
  ```

- [1.10](#campo) Data Selector (DS)
    - Los Data Selector deben nombrarse con las primeras dos letras DS y la referencia del objeto al cual va manejar.

**[Volver al inicio](#Indice)**

## Código en la KB

- [1.1](#campo) Tabulador
    - En lugar de usar espacios antes de la comparación, es recomendable utilizar el tabulador (tab) para mejorar la
      legibilidad del código y facilitar su desarrollo y mantenimiento. En Genexus, se puede configurar la cantidad de
      espacios que corresponden a un tabulador.
  ```
  //Ejemplo
    TextblockNombre.Class                   = ThemeClass:TexBlockTabReqOFF
    TextblockApellido.Class                 = ThemeClass:TexBlockTabReqOFF
    TextblockObservaciones.Class            = ThemeClass:TexBlockTabReqOFF
    TextblockControlCalidadrimario.Class    = ThemeClass:TexBlockTabReqOFF
    TextblockTipoDocumento.Class            = ThemeClass:TexBlockTabReqOFF
    TextblockGradoPersona.Class             = ThemeClass:TexBlockTabReqOFF
    TextblockFecha.Class                    = ThemeClass:TexBlockTabReqOFF
  ```

- [1.2](#campo) Espacios
    - Para una mayor claridad en la lectura del código, se debe dejar un espacio después de la coma que separa los
      parámetros.
  ```
    Parm(InOut: &Name, InOut: &UserId, in: &InvoiceId);
  ```

- [1.3](#campo) Comillas
    - Es recomendable utilizar comillas simples ('...') por defecto para estandarizar el código y mejorar su
      legibilidad.
    -

- [1.4](#campo) Comentarios
    - Los comentarios son de gran ayuda para entender el código y facilitar su mantenimiento. Es importante incluir
      comentarios en el código para que otros desarrolladores puedan comprenderlo más fácilmente.
    - En las líneas que corresponda se puede comentar el funcionamiento explicando que hace ese fragmento de código, a
      qué procesos llama, etc.
  ```
    // Recorro la tabla con numeradores [InvNumeradores]
        For Each
          Where InvoiceId   = &InvoiceId
          Where UserId      = &UserId
          Defined By NumUltInv
  
          NumUltInv   = NumUltInv + 1
          &NumUltInv  = NumUltInv
  
    // Se crea nuevo número de invoice 
  ```

- [1.5](#campo) Subrutinas
    - En ocasiones, podemos encontrarnos con fragmentos de código que se repiten en distintas partes de nuestra
      aplicación. Cuando esto sucede, es importante evitar duplicar el código y en su lugar, utilizar subrutinas o
      procedimientos para generalizar la funcionalidad. Es importante que los nombres de estas subrutinas y
      procedimientos sean descriptivos y permitan entender rápidamente su funcionalidad. Las subrutinas deben ser
      utilizadas para código que es específico de un objeto, mientras que los procedimientos son adecuados para código
      que es utilizado por varios objetos.

- [1.6](#campo) Referenciar desarrollo correctivo
    - Cuando realizamos cambios en el código, es importante dejar un registro que permita entender el motivo de dicha
      modificación. Una buena práctica es incluir comentarios en la línea correspondiente, indicando la Iniciales, fecha
      y el número de incidente asociado del sistema de seguimiento de incidencias. De esta manera, cualquier persona que
      trabaje en el código en el futuro podrá entender la razón detrás de la modificación.
  ```
    // eds - 28/12/2022 - incidente 48523 - Obtengo el próximo número de invoice 
  ```
- [1.7](#campo) Tablas Base en Grillas
    - Siempre que sea posible, debemos utilizar grillas de tabla base. Esto nos permite aprovechar al máximo la
      funcionalidad de Genexus y reducir el tiempo de desarrollo y mantenimiento de nuestra aplicación.

- [1.8](#campo) Cerrar Sesiones
    - Es importante tener en cuenta que cada vez que se utiliza una web sesión con una clave, se debe cerrar
      adecuadamente la instancia correspondiente utilizando el método session.remove() de la clave. De lo contrario, se
      pueden generar problemas de seguridad y/o recursos.
  ```
    //Obtener codigo de Invoice
    &SDTInvoice.FromJson(&ws.Get('&SDTInvoice'))

    //Cierro sesión
    &ws.Remove('&SDTInvoice')
  ```
- [1.9](#campo) Parametrizar
    - Siempre se debe tratar de parametrizar el código en la medida de lo posible. De lo contrario, es probable que en
      el futuro se requieran cambios en el código. Esto puede generar tiempos de inactividad innecesarios del sistema en
      producción.

- [1.10](#campo) Warnings
    - Es recomendable revisar periódicamente los warnings generados por el código para minimizar las probabilidades de
      fallos y mejorar el rendimiento de la aplicación. Los warnings pueden indicar desde errores de sintaxis hasta
      problemas de rendimiento, por lo que su corrección es fundamental.

**[Volver al inicio](#Indice)**

## Probar tu código

- Es fundamental probar el código que escribimos antes de enviarlo al repositorio de código, incluso después de hacer
  pequeños cambios. Esto nos permite detectar errores y corregirlos antes de que el código sea utilizado por los
  usuarios o modificado por otros desarrolladores. Una buena práctica es escribir casos de prueba y ejecutarlos de
  manera automática para asegurarnos de que el código funciona correctamente en distintos escenarios.

## Commit on exit

- Genexus por defecto utiliza la propiedad commit on exit al finalizar los procedimientos, lo cual puede ocultar errores
  en el código que escribimos y se pierde el control de los impactos a la base de datos. Para evitar esto, es una buena
  práctica deshabilitar esta propiedad en las preferencias de Genexus y agregar validaciones específicas que nos
  permitan detectar cualquier error en el código antes de hacer el commit. En las Preferences se debe modificar esta
  propiedad en No.

## Control de versiones

- Es fundamental utilizar un sistema de control de versiones para gestionar el código y las modificaciones realizadas
  sobre él. Esto permite tener un registro histórico de los cambios y revertirlos en caso de ser necesario. Además, es
  recomendable utilizar una estrategia de branching adecuada para separar el código en diferentes ramas de desarrollo,
  de pruebas y de producción.