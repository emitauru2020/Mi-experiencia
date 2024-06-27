# Guia para el desarrollo en Odoo
por [Emanuel da Silva](https://www.linkedin.com/in/emanuel-da-silva-5487a72a9/)

## Intro
La idea es registrar las diferentes experiencias afrontadas en Odoo:

## Objetivo
Prueba 1Tratar de unificar en un solo lugar los diferentes documentos que se realizaron para el desarrollo en Odoo en los cuales estuve involucrado:

  1. Mejores prácticas para el desarrollo en Odoo.
  1. Ejemplos de como resolver casos especificos.
  1. Novedades del lenguaje.

## Tabla de Contenidos

  1. [Creación de nombres](#Creación-de-nombres)
  1. [Segundo](#Segundo)
  1. [Tercero](#Tercero)
  1. [Cuarto](#Cuarto)
  1. [Herencia desde Modulos diferentes](#Herencia-desde-Modulos-diferentes)


## Creación de nombres

  <a name="campo"></a><a name="1.1"></a>
  - [1.1](#campo) Se debe ser descriptivo con los nombres de los campos.
	> El nombre debe ser autodescriptivo.

    ```javascript
    mal
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
  - [2.4](#whitespace-parms) Algo algo algo.

	> Algo.

	 ```javascript
    // mal
    parm();

    // bien
    parm();

    ```

**[Volver al inicio](#tabla-de-contenidos)**

## Herencia desde Modulos diferentes
  <a name="herencia-modelos-diferentes"></a><a name="3.1"></a>
  - [3.1](#herencia-modelos-diferentes) Para este ejemplo tenemos dos modelos donde el Modelo_a ya esta creado y se desea en el Modelo_b poder heredar las vistas agregando un campo.
    > Primero debemos identificar el xml que posee la vista que deseamos obtener, para este caso heredamos la vista tree y form agregandole un campo en ambas.
    > Estas dos vistas pertencen al modelo hr_attendance_pro.time_record_event.

   	 ```javascript
	<record id="hr_attendance_pro_time_record_event_tree_view" model="ir.ui.view">
		<field name="name">hr_attendance_pro.time_record_event.tree</field>
		<field name="model">hr_attendance_pro.time_record_event</field>
                   <field name="arch" type="xml">
                       <tree editable="bottom" >
                           <field name="record_id" readonly="1"/>
                           <field name="date" attrs="{'readonly':[('can_edit','=',False)]}"/>
                           <field name="year" readonly="1"/>
                       </tree>
                    </field>
        </record>

	<record id="hr_attendance_pro_time_record_event_form_view" model="ir.ui.view">
		<field name="name">hr_attendance_pro.time_record_event.form</field>
		<field name="model">hr_attendance_pro.time_record_event</field>
                   <field name="arch" type="xml">
                      <form string="Event" version="7.0">
                        <sheet>
                          <group>
                             <group>
                                <field name="value" required="1" attrs="{'readonly':[('origin','=','rule')]}"/>
                                <field name="val_unit" readonly="1" options="{'save_readonly':True}"/>
                             </group>
                             <group>
                                 <field name="month"  required="1" attrs="{'readonly':[('origin','=','rule')]}"/>
                                 <field name="year"  required="1" widget="char" attrs="{'readonly':[('origin','=','rule')]}"/>
                                 <field name="retroactivo"  readonly="1"/>
                                 <field name="rule_id" readonly="1"/>
                             </group>
                        </group>
                      </sheet>
                    </form>
               </field>
             </record>
     
    ```
     > Para este ejemplo ya existia time_record.py el cual hereda hr_attendance_pro.time_record_event.
     > Se agrega 'observaciones' en el modelo time_record.py.
   	 ```javascript
	class BPSTimeRecordEvent(models.Model):
   	 _inherit = "hr_attendance_pro.time_record_event"

    	nombre_completo = fields.Char(comodel_name='hr.employee',related='employee_id.name_related',string=u'Nombre de funcionario')
    	numero_funcionario = fields.Char(comodel_name='hr.employee',related='employee_id.code',string=u'Número de funcionario')
    	documento = fields.Char(comodel_name='hr.employee', related='employee_id.identification_id', string=u'Nro. Documento')
     	//Nuevo campo
   		observaciones = fields.Text(string=u'Observaciones', required=True)
     
    ```
     > El nombre del id se mantiene lo el codigo que este creado para este caso ya que esta en un modulo bps se le pone ese prefijo y al final 'inherit' haciendo referencia a que es una vista heredada la misma logica para el "name"
     > El "model" sera el del cual estamos heredando.
     > En "inherit_id" se ingresa el modelo.id_tree en este caso: hr_attendance_pro.hr_attendance_pro_time_record_event_form_view.
     > Por ultimo ubicamos el nuevo campo despues del campo "description".
    ```javascript
        <record id="bps_hr_attendance_pro_time_record_event_tree_view_inherit" model="ir.ui.view">
            <field name="name">hr_attendance_pro.time_record.tree.inherit</field>
            <field name="model">hr_attendance_pro.time_record_event</field>
            <field name="inherit_id" ref="hr_attendance_pro.hr_attendance_pro_time_record_event_tree_view"/>
            <field name="arch" type="xml">
                <field name="description" position="after">
                    <field name="observaciones" readonly="1"/>
                </field>
            </field>
        </record>
    ```
    > Para el form lo ubicamos antes del primer grupo que exista.
    ```javascript
        <record id="bps_hr_attendance_pro_time_record_event_form_view_inherit" model="ir.ui.view">
            <field name="name">hr_attendance_pro.time_record.form.inherit</field>
            <field name="model">hr_attendance_pro.time_record_event</field>
            <field name="inherit_id" ref="hr_attendance_pro.hr_attendance_pro_time_record_event_form_view"/>
            <field name="arch" type="xml">
                <xpath expr="//group" position="before">
                    <group>
                        <field name="observaciones"/>
                    </group>
                </xpath>
            </field>
        </record>
    ```
