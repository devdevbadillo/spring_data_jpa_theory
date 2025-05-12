# Spring Data JPA

## Tabla de Contenido

- [Fundamentos de JPA](#fundamentos-jpa)
  - [¿Qué es JPA?](#que-es-jpa)
    - [Interfaces centrales en JPA](#interfaces-centrales-en-jpa)
        - [EntityManagerFactory](#entity-manager-factory)
        - [EntityManager](#entity-manager)
    - [Proveedores de JPA](#provedores-de-jpa)
        - [Hibernate](#hibernate)
        - [OpenJPA](#open-jpa)
  - [Mapeo Objeto-Relacional (ORM)](#orm)
    - [Anotaciones básicas](#anotaciones-basicas)
        - [La anotación @Entity](#la-anotacion-entity)
        - [La anotación @Table](#la-anotacion-table)
        - [Las anotaciones @Id y @GeneratedValued](#id-y-generated-value)
        - [La anotación @Basic](#la-anotacion-basic)
        - [La anotación @Temporal](#la-anotacion-temporal)
        - [La anotación @Enumerated](#la-anotacion-enumerated)
        - [La anotación @Lob](#la-anotacion-lob)
        - [La anotación @Transient](#la-anotacion-transient)
    - [Claves Primarias](#claves-primarias)
        - [Claves simples](#claves-simples)
        - [Claves embebidas](#claves-embebidas)
    - [Mapeo de tipos de datos](#mapeo-de-tipo-de-datos)
    - [Columnas](#columns)
    - [Mapeo de Tipos Embebidos](#mapeo-de-tipos-embebidos)
        - [La anotación @Embeddable](#anotacion-embeddable)
        - [La anotación @Embedded](#anotacion-embedded)
    - [Mapeo de colecciones](#mapeo-de-colecciones)
        - [La anotación @ElementCollection](#anotacion-element-collection)
        - [La anotación @CollectionTable](#anotacion-collection-table)
    - [Relaciones entre entidades](#relacion-entre-entidades)
        - [Relaciones unidireccionales y bidireccionales](#relaciones-unidireccionales-y-bidireccionales)
        - [Cardinalidad](#cardinalidad)
            - [Uno-a-Uno](#uno-a-uno)
            - [Uno-a-Muchos](#uno-a-muchos)
            - [Muchos-a-Uno](#muchos-a-uno)
            - [Muchos-a-Muchos](#muchos-a-muchos)
        - [Estrategias de Fetching](#estrategia-de-fetching)
            - [Carga Perezosa (Lazy)](#carga-perezosa)
            - [Carga Ansiosa (Eager)](#carga-ansiosa)
        - [Operaciones en cascada](#operaciones-en-casacada)
- [Consultas y recuperación de datos](#consultas-y-recuperacion-de-datos)
    - [Java Persistence Query Language (JPQL)](#jpql)
        - [Sintaxis Básica](#sintaxis-de-jpql)
            - [Sentencias básicas SELECT, FROM, WHERE, ORDER BY, GROUP BY, HAVING](#sentencias-basicas)
        - [Consultas con Parámetros](#consultas-con-parametros)
        - [Funciones y Operadores](#funciones-y-operadores)
            - [Funciones agregadas](#funciones-agregadas)
            - [Funciones de cadena](#funciones-de-cadena)
            - [Operadores lógicos y de comparación](#operadores-logicos-y-de-comparacion)
        - [Relaciones en JPQL (JOIN, FETCH JOIN)](#relaciones-en-jpql)
        - [Consultas dinámicas](#consultas-dinamicas)
    - [La API de Criterios (Criteria API)](#criteria-api)
        - [Construcción de consultas programáticamente](#construccion-de-consultas-programaticamente)
        - [Predicados, ordenamientos y agrupaciones](#predicados-ordenamiento-y-agrupacion)
        - [Consultas con relaciones](#consultas-con-relaciones)
    - [Consultas Nativas (Native Queries)](#consultas-nativas)
        - [Ejecución de SQL Directo](#sql-directo)
        - [Mapeo de resultados a entidades o escalares](#mapeo-de-resultado-en-consulta-nativa)
- [Transacciones y concurrencia](#transacciones-y-concurrencia)
    - [Gestión de transacciones](#gestion-de-transacciones)
        - [Transacciones locales (Resource-Local)](#transacciones-locales)
        - [Transacciones JTA](#transacciones-jta)
    - [Delimitación de transacciones](#delimitacion-de-transacciones)
        - [Inicio, commit y rollback](#inicio-commit-rollback)
    - [Aislamiento de transacciones](#aislamiento-de-transacciones)
        - [Lectura sucia](#lectura-sucia)
        - [Lectura no repetible](#lectura-no-repetible)
        - [Lectura fantasma](#lectura-fantasma)
    - [Bloqueos](#bloqueos)
        - [Optimistic](#optimistic)
        - [Pessimistic Locking](#pessimistic-locking)
    - [Contexto de persistencia](#contexto-de-persistencia)
        - [Cache de primer nivel](#cache-de-primer-nivel)
        - [Ciclo de vida de las entidades](#ciclo-de-vida-de-las-entidadeS)
            - [El estado New](#el-estado-new)
            - [El estado Managed](#el-estado-managed)
            - [El estado Detached](#el-estado-detached)
            - [El estado Removed](#el-estado-removed)
        - [Operaciones del EntityManager](#operaciones-de-entity-manager)
            - [Métodos persist(), merge(), remove(), find(), refresh(), detach(), clear() y flush()](#metodos-comunes-del-entity-manager)
         

<a id="fundamentos-jpa"></a>
# Fundamentos de JPA

<a id="que-es-jpa"></a>
## Qué es JPA?
JPA, o Java Persistence API, es una especificación del API de Java para acceder, persistir y gestionar datos entre objetos Java (nuestra aplicación) y una base de datos relacional. En esencia, **JPA define un conjunto de interfaces y anotaciones que los desarrolladores utilizan para interactuar con las bases de datos** de una manera orientada a objetos

> [!IMPORTANT]
> Es crucial entender que JPA en sí misma no es una pieza de software que se descarga y ejecuta. Más bien, **define un estándar que las implementaciones (como Hibernate, EclipseLink, OpenJPA) deben seguir**.

JPA se basa en el concepto de **Mapeo Objeto-Relacional**. Su objetivo principal es mapear las clases Java (entidades) a las tablas de una base de datos relacional, y las instancias de esas clases a las filas de esas tablas. Esto permite a los desarrolladores trabajar con objetos en su código Java sin tener que preocuparse directamente por el lenguaje SQL subyacente.

<a id="interfaces-centrales-en-jpa"></a>
### Interfaces centrales en JPA
Dentro de la especificación de JPA, existen algunas interfaces que son fundamentales para interactuar con la persistencia de datos. Dos de las más importantes son EntityManagerFactory y EntityManager

<a id="entity-manager-factory"></a>
#### EntityManagerFactory
La interfaz EntityManagerFactory es responsable de crear instancias de EntityManager. 

La creación de un EntityManagerFactory suele ser una operación costosa en términos de recursos, ya que implica la lectura de la configuración, la inicialización de la conexión a la base de datos y la configuración del proveedor de JPA. Por esta razón, debería ser creada una sola vez (por unidad de persistencia) y compartida a lo largo de la vida de la aplicación

<a id="entity-manager"></a>
#### EntityManager
La interfaz EntityManager es la interfaz principal para **interactuar con el contexto de persistencia**. Es a través de una instancia de EntityManager que se realizan las operaciones de persistencia sobre las entidades definidas en la aplicación.


> [!NOTE]
> 1. Un EntityManager generalmente se asocia con una unidad de trabajo o una transacción. Es una instancia de corta duración que se crea cuando se necesita realizar operaciones de base de datos y se cierra una vez que la unidad de trabajo se completa
> 2. El EntityManager es responsable de gestionar el ciclo de vida de las entidades en el contexto de persistencia. Este contexto actúa como una caché de primer nivel, rastreando los cambios realizados en las entidades gestionadas.


<a id="provedores-de-jpa"></a>
### Proveedores de JPA
Como se ha mencionado, JPA es una especificación. Para que JPA funcione en tu aplicación, necesitas una **implementación concreta de esta especificación, lo que se conoce como un proveedor de JPA**. Estos proveedores son los que realmente se encargan de traducir las operaciones de JPA a las operaciones específicas del motor de base de datos.

<a id="hibernate"></a>
#### Hibernate
Hibernate es uno de los proveedores de JPA más populares y maduros. Aunque existía antes de la especificación de JPA, adoptó JPA y ahora es una implementación completa y ampliamente utilizada.

> [!NOTE]
> - En el ecosistema de Spring, Hibernate suele ser el proveedor de JPA predeterminado y se integra muy bien con Spring Data JPA.
> - Hibernate ofrece una gran cantidad de características más allá de lo que especifica JPA, como su propio lenguaje de consultas (HQL), potentes mecanismos de caching (primer y segundo nivel), optimización de consultas, interceptores y listeners específicos de Hibernate, y soporte para mapeos avanzados

<a id="open-jpa"></a>
#### OpenJPA
Apache OpenJPA es otro proveedor de JPA de código abierto desarrollado bajo la sombrilla de la Apache Software Foundation. OpenJPA implementa completamente la especificación de JPA y está diseñado para ser flexible y extensible, permitiendo a los desarrolladores personalizar su comportamiento a través de plugins y extensiones.

<a id="orm"></a>
## Mapeo Objeto-Relacional (ORM)

El Mapeo Objeto-Relacional (ORM) es el corazón de JPA. Es la técnica que permite **traducir los datos entre el modelo de objetos de la aplicación Java y el modelo relacional de una base de datos**. JPA facilita este mapeo a través de una serie de anotaciones y configuraciones.

<a id="anotaciones-basicas"></a>
### Anotaciones básicas
Las anotaciones básicas son las que **se utilizan directamente en las clases Java (entidades)** para definir cómo se mapean a las tablas de la base de datos y cómo se gestionan sus propiedades.

<a id="la-anotacion-entity"></a>
#### La anotación @Entity
La anotación @Entity es fundamental. Se aplica a una clase Java para marcarla como una entidad, lo que **significa que representa una tabla en la base de datos**.

> [!IMPORTANT]
> - Cualquier clase que se quiera persistir utilizando JPA debe estar anotada con @Entity
> - Por defecto, el nombre de la entidad es el mismo que el nombre de la clase (sin el nombre del paquete). Es posible especificar un nombre diferente utilizando el atributo name de la anotación, por ejemplo: @Entity(name = "UsuarioSistema"). **Este nombre se utiliza en las consultas JPQL**.
> - La clase anotada con @Entity debe ser una clase de nivel superior y no puede ser abstracta
> - La entidad debe tener un constructor sin argumentos (puede ser público o protegido). Esto es necesario para que el proveedor de JPA pueda instanciar objetos de la entidad.

```
@Entity
public class User{
  // Metodos y atributos...
  // Getter y setters...
}
```

<a id="la-anotacion-table"></a>
#### La anotación @Table
La anotación @Table se utiliza a nivel de clase (junto con @Entity) para **especificar los detalles de la tabla de la base de datos a la que se mapea la entidad**.

> Atributos
1. name: Especifica el nombre de la tabla en la base de datos. Es obligatorio si quieres usar un nombre diferente al de la entidad. Ejemplo: @Table(name = "TB_USUARIOS")
2. schema: Especifica el nombre del esquema de la base de datos al que pertenece la tabla. Ejemplo: @Table(name = "TB_USUARIOS", schema = "public")
3. catalog: Especifica el nombre del catálogo de la base de datos al que pertenece la tabla
4. uniqueConstraints: Permite definir restricciones de unicidad a nivel de tabla, especificando las columnas que deben ser únicas (pueden ser combinaciones de columnas)
   
```
@Table(name = "TB_USUARIOS",
       uniqueConstraints = {@UniqueConstraint(columnNames = {"email"}),
                            @UniqueConstraint(columnNames = {"nombreUsuario"})})
```

<a id="id-y-generated-value"></a>
#### Las anotaciones @Id y @GeneratedValued
1. La anotación @Id se aplica a un atributo de la entidad para **marcarlo como la clave primaria de la tabla correspondiente**. Cada entidad debe tener al menos un atributo anotado con @Id.

> [!IMPORTANT]
> El tipo de datos del atributo @Id debe ser un tipo primitivo, su correspondiente wrapper, java.lang.String, java.util.Date, java.sql.Date, java.math.BigDecimal, etc., o un tipo serializable.

2. La anotación @GeneratedValue se utiliza junto con @Id para especificar la estrategia de generación de valores para la clave primaria.

> Estrategias de Generación
> 
> - GenerationType.AUTO: El proveedor de JPA elige una estrategia de generación apropiada basándose en las capacidades de la base de datos subyacente. Es la opción por defecto.
> - GenerationType.IDENTITY: La base de datos genera el valor automáticamente (por ejemplo, a través de una columna autoincremental en MySQL o PostgreSQL). JPA recupera el valor generado después de la inserción.
> - GenerationType.SEQUENCE: La base de datos genera el valor a partir de una secuencia (común en Oracle y PostgreSQL). Se puede especificar el nombre de la secuencia mediante el atributo generator y la anotación @SequenceGenerator
> - GenerationType.TABLE: JPA utiliza una tabla separada en la base de datos para simular la generación de secuencias. Se puede especificar el nombre de la tabla y otros detalles mediante el atributo generator y la anotación @TableGenerator

> Ejemplo
```
@Entity
@Table(name = "productos")
public class Producto {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // ... otros atributos ...
}
```

<a id="la-anotacion-basic"></a>
#### La anotación @Basic
La anotación @Basic se utiliza para **mapear un atributo de la entidad a una columna de la tabla de la base de datos**. Es la anotación por defecto para los atributos que no están anotados con otras anotaciones de mapeo (como @Id, @OneToMany, etc).

> Atributos
> 
> - fetch: Especifica la estrategia de fetching para este atributo. Puede ser FetchType.LAZY (cargar el valor solo cuando se accede a él) o FetchType.EAGER (cargar el valor inmediatamente al cargar la entidad). Por defecto es FetchType.EAGER para los tipos básicos.
> - optional: Indica si el valor de este atributo puede ser nulo en la base de datos. Por defecto es true (puede ser nulo). Si se establece en false, se intentará crear una restricción NOT NULL en la columna de la base de datos (dependiendo del proveedor y la configuración).

> Ejemplo

```
@Entity
public class Usuario {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Basic
    private String nombre;

    @Basic(fetch = FetchType.LAZY)
    private String detallesAdicionales;

    @Basic(optional = false)
    private String email;

    // ... otros atributos ...
}
```

<a id="la-anotacion-temporal"></a>
#### La anotación @Temporal
La anotación @Temporal se utiliza para **especificar la precisión del tipo de datos java.util.Date o java.util.Calendar cuando se mapea a una columna de fecha/hora en la base de datos**.

El atributo value de @Temporal puede tomar uno de los siguientes valores:
 - TemporalType.DATE: Mapea solo la parte de la fecha (año, mes, día)
 - TemporalType.TIME: Mapea solo la parte de la hora (horas, minutos, segundos)
 - TemporalType.TIMESTAMP: Mapea tanto la fecha como la hora, con precisión de milisegundos (dependiendo de la base de datos)

> Ejemplo
```
@Entity
public class Evento {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Temporal(TemporalType.DATE)
    private java.util.Date fechaEvento;

    @Temporal(TemporalType.TIMESTAMP)
    private java.util.Date horaInicio;

    // ... otros atributos ...
}
```

<a id="la-anotacion-enumerated"></a>
#### La anotación @Enumerated
La anotación @Enumerated se utiliza para **mapear un atributo de tipo enumerado (enum) a una columna de la base de datos**.

Estrategias de mapeo: 
- EnumType.ORDINAL:El enum se mapea a su valor ordinal (la posición del enum en su declaración, comenzando desde 0). No se recomienda generalmente ya que **si se cambia el orden de los enums, los datos existentes en la base de datos se volverán incorrectos**.
- EnumType.STRING: El enum se mapea a su nombre como una cadena de texto. Se recomienda generalmente ya que es más robusto a los cambios en la declaración del enum.

> Ejemplo
```
public enum EstadoPedido {
    PENDIENTE, PROCESANDO, ENVIADO, ENTREGADO, CANCELADO
}

@Entity
public class Pedido {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Enumerated(EnumType.STRING)
    private EstadoPedido estado;

    // ... otros atributos ...
}
```

<a id="la-anotacion-lob"></a>
#### La anotación @Lob
La anotación @Lob se utiliza para mapear atributos de gran tamaño (Large Objects) a los tipos de datos BLOB (Binary Large Object) o CLOB (Character Large Object) de la base de datos.

@Lob se puede aplicar a los siguientes tipos de datos Java:
1. byte[] o java.sql.Blob para mapear a BLOB.
2. char[], java.lang.String o java.sql.Clob para mapear a CLOB.

> Ejemplo
```
@Entity
public class Documento {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Lob
    private byte[] contenidoBinario;

    @Lob
    private String textoLargo;

    // ... otros atributos ...
}
```

<a id="la-anotacion-transient"></a>
#### La anotación @Transient

La anotación @Transient se utiliza para marcar **un atributo de una entidad que no debe ser persistido en la base de datos**. Es decir, este atributo existirá en la instancia del objeto Java, pero JPA ignorará este campo durante las operaciones de persistencia

> Ejemplo
```
@Entity
public class Usuario {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;
    private String apellido;

    @Transient
    private String nombreCompleto; // No se persistirá

    // ... otros métodos ...
}
```

<a id="claves-primarias"></a>
### Claves Primarias
La clave primaria es un atributo (o un conjunto de atributos) que identifica de forma única cada fila en una tabla de la base de datos. En JPA, esto se mapea a través de la anotación @Id.

<a id="claves-simples"></a>
#### Claves simples

### Generación de claves
Una clave primaria simple es aquella que **está compuesta por una sola columna en la tabla de la base de datos** y se mapea a un único atributo en la entidad Java anotado con @Id.

> [!NOTE]
> Ya se han visto ejemplos de claves primarias simples con la anotación @GeneratedValue.

<a id="claves-embebidas"></a>
#### Claves embebidas
Una clave primaria embebida se utiliza cuando **la clave primaria de la tabla de la base de datos está compuesta por múltiples columnas**. En JPA, esto se logra utilizando la anotación @EmbeddedId en la entidad, junto con una clase separada anotada con @Embeddable que representa la clave primaria compuesta.

> Pasos para crear una clave embebida

1. Se crea una clase separada que contiene los atributos que forman la clave primaria compuesta. Esta clase debe estar anotada con @Embeddable.
  - Debe ser pública.
  - Debe tener un constructor sin argumentos (público o protegido).
  - Debe implementar la interfaz java.io.Serializable.
  - Debe sobrescribir los métodos equals() y hashCode() basados en los valores de sus atributos.

2. En la entidad, se declara un atributo del tipo de la clase @Embeddable y se anota con @EmbeddedId

> Ejemplo
```
import java.io.Serializable;
import javax.persistence.Embeddable;

@Embeddable
public class CuentaProyectoId implements Serializable {
    private Long proyectoId;
    private Long usuarioId;

    // Constructores, getters, setters, equals(), hashCode() ...
}

@Entity
public class CuentaProyecto {
    @EmbeddedId
    private CuentaProyectoId id;

    // ... otros atributos ...
}
```

<a id="mapeo-de-tipo-de-datos"></a>
### Mapeo de tipos de datos
JPA se encarga de mapear los tipos de datos de los atributos de tus entidades Java a los tipos de datos correspondientes en las columnas de la base de datos.

> [!NOTE]
> 
> Los tipos primitivos (como int, boolean, double) se mapean a columnas **NOT NULL** por defecto, mientras que sus **wrappers** (como Integer, Boolean, Double) permiten valores **NULL**.

JPA permite definir convertidores personalizados (@Converter) para mapear tipos de datos Java que no tienen un mapeo estándar a los tipos de datos de la base de datos.

<a id="columns"></a>
### Columnas
La anotación @Column se utiliza para **personalizar el mapeo de un atributo de la entidad a una columna específica de la tabla** de la base de datos.

> Atributos Principales
- name: Especifica el nombre de la columna en la base de datos. Si no se especifica, se utiliza el nombre del atributo de la entidad. Ejemplo: @Column(name = "nombre_usuario")
- nullable: Especifica si la columna puede contener valores nulos. Por defecto es true. Ejemplo: @Column(nullable = false)
- unique: Especifica si la columna debe tener una restricción de unicidad. Por defecto es false. Ejemplo: @Column(unique = true)
- length: Especifica la longitud máxima de la columna para tipos String. Ejemplo: @Column(length = 255)
- precision y scale: Se utilizan para tipos numéricos de precisión arbitraria (como BigDecimal) para especificar el número total de dígitos y el número de dígitos después del punto decimal, respectivamente
- columnDefinition: Permite especificar la definición SQL completa de la columna. Esto proporciona la máxima flexibilidad pero reduce la portabilidad entre bases de datos. Ejemplo: @Column(columnDefinition = "VARCHAR(100) COLLATE utf8_bin")
- insertable: Indica si la columna debe incluirse en las operaciones de inserción. Por defecto es true. Se puede usar para columnas calculadas o gestionadas por la base de datos
- updatable: Indica si la columna debe incluirse en las operaciones de actualización. Por defecto es true. Se puede usar para columnas de solo lectura después de la inserción

<a id="mapeo-de-tipos-embebidos"></a>
### Mapeo de Tipos Embebidos
El mapeo de tipos embebidos permite **incrustar los atributos de una clase Java dentro de otra entidad**, de tal manera que las columnas correspondientes se crean en la tabla de la entidad que contiene el tipo embebido. Esto es útil para agrupar lógicamente atributos relacionados

<a id="anotacion-embeddable"></a>
#### La anotación @Embeddable
La anotación @Embeddable se aplica a una clase Java para marcarla como un tipo embebible. Una clase anotada con @Embeddable no representa una entidad por sí misma y no tiene su propia tabla en la base de datos. **Sus atributos se mapean a las columnas de la tabla de la entidad que la referencia**.

<a id="anotacion-embedded"></a>
#### La anotación @Embedded
La anotación @Embedded se utiliza en un atributo de una entidad para indicar que este **atributo es una instancia de una clase anotada con @Embeddable**. Los atributos de la clase embebible se mapearán a columnas en la tabla de la entidad contenedora.

Por defecto, los nombres de las columnas generadas para los atributos de la clase embebible serán los mismos que los nombres de los atributos de la clase embebible. Sin embargo, se puede utilizar la anotación @AttributeOverrides (y su anotación interna @AttributeOverride) para especificar nombres de columna diferentes o para sobrescribir la configuración de las columnas heredadas del @Embeddable

```
import javax.persistence.Embeddable;
import javax.persistence.Column;

@Embeddable
public class Direccion {
    @Column(name = "calle")
    private String calle;
    private String ciudad;
    private String codigoPostal;

    // Constructores, getters, setters ...
}

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Embedded;
import javax.persistence.AttributeOverrides;
import javax.persistence.AttributeOverride;

@Entity
public class Cliente {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;

    @Embedded
    @AttributeOverrides({
        @AttributeOverride(name = "calle", column = @Column(name = "direccion_calle", length = 100)),
        @AttributeOverride(name = "codigoPostal", column = @Column(name = "zip_code"))
    })
    private Direccion direccion;

    // ... otros atributos ...
}
```

En este ejemplo, la clase **Direccion** está marcada como @Embeddable. En la entidad **Cliente**, el atributo direccion está anotado con @Embedded, lo que significa que los atributos calle, ciudad y codigoPostal de Direccion se mapearán a las columnas direccion_calle, ciudad y zip_code en la tabla Cliente, respectivamente (notar cómo @AttributeOverrides se usa para personalizar los nombres de las columnas de calle y codigoPostal).

<a id="mapeo-de-colecciones"></a>
### Mapeo de colecciones

JPA permite mapear colecciones de tipos básicos (como List<String>, Set<Integer>) o colecciones de tipos embebidos. Esto se logra con la anotación @ElementCollection

<a id="anotacion-element-collection"></a>
#### La anotación @ElementCollection
La anotación @ElementCollection se utiliza para mapear una colección de instancias de un tipo embebible o de un tipo básico (como String, Integer, Date). Por defecto, los elementos de la colección se persistirán en una tabla separada.

> [!IMPORTANT]
> - @ElementCollection generalmente requiere una tabla join para almacenar los elementos de la colección, ya que no forman parte directamente de la tabla de la entidad propietaria.
> - La tabla join tendrá una clave foránea que referencia la clave primaria de la entidad propietaria.


<a id="anotacion-collection-table"></a>
#### La anotación @CollectionTable
La anotación @CollectionTable **se utiliza junto con @ElementCollection** para especificar los detalles de la tabla join que se utilizará para almacenar la colección de elementos.

> Atributos de la anotación
- name: Especifica el nombre de la tabla join.
- schema: Especifica el esquema de la tabla join.
- catalog: Especifica el catálogo de la tabla join.
- joinColumns: Permite especificar las columnas de la tabla join que se utilizarán como clave foránea para referenciar la entidad propietaria. Se utiliza la anotación @JoinColumn dentro de @JoinColumns

> Ejemplo(colección de tipos básicos)
```
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.ElementCollection;
import javax.persistence.CollectionTable;
import javax.persistence.Column;
import javax.persistence.JoinColumn;
import java.util.List;

@Entity
public class Usuario {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;

    @ElementCollection
    @CollectionTable(name = "telefonos_usuario", joinColumns = @JoinColumn(name = "usuario_id"))
    @Column(name = "telefono")
    private List<String> telefonos;

    // ... otros atributos ...
}
```
En este ejemplo, la lista de teléfonos del usuario se mapea a una tabla llamada telefonos_usuario. Esta tabla tiene una columna usuario_id que es una clave foránea a la tabla Usuario, y una columna telefono para almacenar cada número de teléfono.

> Ejemplo (colección de tipos embebidos)

```
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.ElementCollection;
import javax.persistence.CollectionTable;
import javax.persistence.JoinColumn;
import javax.persistence.Embedded;
import java.util.List;

@Embeddable
public class DireccionEntrega {
    private String calle;
    private String ciudad;
    private String codigoPostal;

    // Constructores, getters, setters ...
}

@Entity
public class Pedido {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ElementCollection
    @CollectionTable(name = "direcciones_entrega_pedido", joinColumns = @JoinColumn(name = "pedido_id"))
    @Embedded
    private List<DireccionEntrega> direccionesEntrega;

    // ... otros atributos ...
}
```
Aquí, la lista de direcciones de entrega (que son tipos embebidos) se mapea a la tabla direcciones_entrega_pedido, con una clave foránea pedido_id y las columnas correspondientes a los atributos de DireccionEntrega (calle, ciudad, codigoPostal).

<a id="relacion-entre-entidades"></a>
### Relaciones entre entidades
En el mundo de las bases de datos relacionales, las relaciones se definen a través de claves foráneas. En JPA, estas relaciones entre tablas se representan como asociaciones entre las entidades Java. **JPA proporciona varias anotaciones para definir estas relaciones**.

<a id="relaciones-unidireccionales-y-bidireccionales"></a>
#### Relaciones unidireccionales y bidireccionales
La direccionalidad de una relación define si se puede navegar desde una entidad a la otra.

> Relación unidireccional

En una relación unidireccional, la navegación solo es posible en una dirección. Por ejemplo, si se tiene una entidad **Pedido** que tiene una referencia a una entidad **Cliente**, **es posible acceder al cliente desde el pedido**, pero **no se puede navegar directamente desde el cliente a todos sus pedidos** (a menos que se defina explícitamente la relación en la entidad **Cliente**).

En JPA, una relación unidireccional se define incluyendo un atributo de la entidad relacionada en la entidad fuente y anotándolo con la anotación de relación apropiada (@ManyToOne, @OneToOne, @OneToMany, @ManyToMany) **sin una propiedad de "mappedBy"**.

> Ejemplo

```
@Entity
public class Cliente {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;
    // ...
}

@Entity
public class Pedido {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Date fechaPedido;

    @ManyToOne
    @JoinColumn(name = "cliente_id")
    private Cliente cliente;

    // ...
}
```
En este caso, desde un objeto Pedido, se puede acceder al Cliente asociado a través del atributo cliente. Sin embargo, desde un objeto Cliente, no se tiene directamente acceso a la lista de Pedidos asociados.

> Relación bidireccional

En una relación bidireccional, la navegación es posible en ambas direcciones. Esto se logra definiendo la relación en ambas entidades. Una de las entidades será la dueña de la relación (la que contiene la información de la clave foránea en su tabla), y la otra tendrá una referencia a la primera y estará marcada como la inversa de la relación utilizando el atributo mappedBy en la anotación de la relación. 

> [!IMPORTANT]
> - El valor de mappedBy debe ser el nombre del atributo en la entidad dueña que representa la relación

```
@Entity
public class Cliente {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @OneToMany(mappedBy = "cliente")
    private List<Pedido> pedidos;

    // ...
}

@Entity
public class Pedido {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Date fechaPedido;

    @ManyToOne
    @JoinColumn(name = "cliente_id")
    private Cliente cliente;

    // ...
}
```
La entidad Pedido es la dueña de la relación (contiene la clave foránea cliente_id). La entidad Cliente tiene una colección de Pedidos y utiliza mappedBy = "cliente" para indicar que esta es la parte inversa de la relación, donde "cliente" es el nombre del atributo en la entidad Pedido que mapea hacia Cliente.

<a id="cardinalidad"></a>
### Cardinalidad
La cardinalidad se refiere al número de instancias de una entidad que pueden estar relacionadas con el número de instancias de otra entidad.

<a id="uno-a-uno"></a>
#### Uno-a-Uno
Una instancia de una entidad está relacionada con como máximo una instancia de otra entidad, y viceversa.

> Ejemplo
```
@Entity
public class Usuario {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombreUsuario;

    @OneToOne(mappedBy = "usuario")
    private CuentaDetalle cuentaDetalle;

    // ...
}

@Entity
public class CuentaDetalle {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Date fechaCreacion;

    @OneToOne
    @JoinColumn(name = "usuario_id", unique = true)
    private Usuario usuario;

    // ...
}
```

Un Usuario puede tener una CuentaDetalle, y una CuentaDetalle pertenece a un Usuario

<a id="uno-a-muchos"></a>
#### Uno-a-Muchos
Una instancia de una entidad puede estar relacionada con muchas instancias de otra entidad, mientras que cada instancia de la segunda entidad está relacionada con una sola instancia de la primera. Estas dos anotaciones siempre trabajan juntas en una relación bidireccional. En una relación unidireccional, solo se usa @OneToMany.

1. Unidireccional (@OneToMany): Una entidad tiene una colección de otras entidades. La tabla de la entidad referenciada tendrá una clave foránea a la tabla de la entidad que contiene la colección.
   
```
@Entity
public class Departamento {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @OneToMany
    @JoinColumn(name = "departamento_id") // La tabla Empleado tendrá esta columna
    private List<Empleado> empleados;

    // ...
}

@Entity
public class Empleado {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;
    // ...
}
```

2. Bidireccional (@OneToMany con mappedBy en la entidad que tiene la colección, y @ManyToOne en la otra entidad)

```
@Entity
public class Departamento {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @OneToMany(mappedBy = "departamento")
    private List<Empleado> empleados;

    // ...
}

@Entity
public class Empleado {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @ManyToOne
    @JoinColumn(name = "departamento_id")
    private Departamento departamento;

    // ...
}
```

<a id="muchos-a-uno"></a>
#### Muchos-a-Uno
Ya cubierto en la relación Uno-a-Muchos. Desde la perspectiva de la entidad "muchos", cada instancia se relaciona con una instancia de la entidad "uno"

<a id="muchos-a-muchos"></a>
#### Muchos-a-Muchos
Muchas instancias de una entidad pueden estar relacionadas con muchas instancias de otra entidad. En la base de datos, esto se implementa típicamente mediante una tabla de unión (o tabla intermedia) que contiene las claves foráneas de ambas tablas relacionadas.
```
@Entity
public class Estudiante {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @ManyToMany
    @JoinTable(
        name = "inscripciones",
        joinColumns = @JoinColumn(name = "estudiante_id"),
        inverseJoinColumns = @JoinColumn(name = "curso_id")
    )
    private List<Curso> cursos;

    // ...
}

@Entity
public class Curso {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombreCurso;

    @ManyToMany(mappedBy = "cursos")
    private List<Estudiante> estudiantes;

    // ...
}
```
Un Estudiante puede inscribirse en muchos Cursos, y un Curso puede tener muchos Estudiantes.
Aquí, la tabla inscripciones se crea con las columnas estudiante_id y curso_id como claves foráneas a las tablas Estudiante y Curso, respectivamente. La entidad Estudiante es la dueña de la relación (define la @JoinTable)
 
<a id="estrategia-de-fetching"></a>
### Estrategias de Fetching
El fetching se refiere a cómo JPA carga las entidades relacionadas desde la base de datos. 

La estrategia de fetching se configura en las anotaciones de las relaciones (@OneToOne, @OneToMany, @ManyToOne, @ManyToMany) utilizando el atributo fetch. Por defecto, las relaciones @OneToOne y @ManyToOne suelen ser EAGER, mientras que las relaciones @OneToMany y @ManyToMany suelen ser LAZY. Sin embargo, estas son solo las estrategias por defecto y pueden ser explícitamente configuradas.

<a id="carga-perezosa"></a>
#### Carga Perezosa (Lazy)
Con la carga perezosa, las entidades relacionadas **no se cargan inmediatamente** cuando se carga la entidad principal. En su lugar, se crea un proxy (un objeto sustituto) para la entidad relacionada. Los datos reales de la entidad relacionada solo se cargarán cuando se acceda por primera vez a la relación (por ejemplo, al llamar a un getter del atributo de la relación)

<a id="carga-ansiosa"></a>
#### Carga Ansiosa (Eager)
Con la carga ansiosa, **las entidades relacionadas se cargan inmediatamente** junto con la entidad principal en la misma consulta (generalmente mediante un JOIN en SQL).

<a id="operaciones-en-casacada"></a>
### Operaciones en cascada
Las operaciones en cascada definen cómo las operaciones de persistencia (como persist, remove, merge, refresh, detach, all) realizadas en una entidad se propagan a las entidades relacionadas. La cascada se configura en las anotaciones de las relaciones utilizando el atributo cascade (que acepta un array de valores de la enumeración CascadeType)

> Valores de CascadeType:

- CascadeType.PERSIST: Cuando se persiste la entidad propietaria, también se persisten las entidades relacionadas que aún no están persistidas.
- CascadeType.MERGE: Cuando se fusiona el estado de la entidad propietaria, también se fusiona el estado de las entidades relacionadas.
- CascadeType.REMOVE: Cuando se elimina la entidad propietaria, también se eliminan las entidades relacionadas (ten cuidado con esto, ya que puede llevar a la pérdida de datos si las entidades relacionadas son referenciadas por otras entidades).
- CascadeType.REFRESH: Cuando se actualiza (refresh) el estado de la entidad propietaria desde la base de datos, también se actualiza el estado de las entidades relacionadas.
- CascadeType.DETACH: Cuando se desasocia (detach) la entidad propietaria del contexto de persistencia, también se desasocian las entidades relacionadas.
- CascadeType.ALL: Aplica todas las operaciones de cascada (PERSIST, MERGE, REMOVE, REFRESH, DETACH).

> Ejemplo
```
@Entity
public class Padre {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @OneToMany(mappedBy = "padre", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Hijo> hijos;

    // ...
}

@Entity
public class Hijo {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

    @ManyToOne
    @JoinColumn(name = "padre_id")
    private Padre padre;

    // ...
}
```
En este ejemplo, si se persiste un objeto Padre, todos los objetos Hijo asociados en su lista hijos también se persistirán automáticamente (CascadeType.PERSIST está implícito en CascadeType.ALL). Si se elimina el Padre, todos los Hijos asociados también se eliminarán (CascadeType.REMOVE en CascadeType.ALL). La opción orphanRemoval = true en @OneToMany también asegura que si un Hijo se remueve de la lista hijos del Padre, también se eliminará de la base de datos (si no es referenciado por otra entidad)

<a id="consultas-y-recuperacion-de-datos"></a>
## Consultas y recuperación de datos
JPA ofrece varias formas de consultar y recuperar datos de la base de datos. Las principales son JPQL (Java Persistence Query Language), la API de Criterios y las Consultas Nativas (SQL directo).

<a id="jpql"></a>
### Java Persistence Query Language (JPQL)
JPQL es un lenguaje de consultas orientado a objetos definido por la especificación JPA. Se asemeja a SQL pero opera sobre las entidades y sus atributos en lugar de directamente sobre las tablas y columnas de la base de datos. Esto proporciona un mayor nivel de abstracción y portabilidad.

> Ejemplo
```
SELECT [DISTINCT] objeto_seleccionado
FROM entidad [AS alias] [[INNER|LEFT|RIGHT] JOIN [FETCH] asociación [AS alias_asociación]]*
[WHERE condición]
[GROUP BY atributos]
[HAVING condición_grupo]
[ORDER BY atributos [ASC|DESC]*]
```

<a id="sintaxis-de-jpql"></a>
#### Sintaxis Básica

<a id="sentencias-basicas"></a>
##### Sentencias básicas SELECT, FROM, WHERE, ORDER BY, GROUP BY, HAVING

- SELECT: Especifica las entidades o los atributos que se van a recuperar. Se puede seleccionar una entidad completa (SELECT p FROM Producto p), un atributo específico (SELECT p.nombre FROM Producto p), o múltiples atributos (SELECT p.nombre, p.precio FROM Producto p). La palabra clave DISTINCT se puede usar para eliminar resultados duplicados.

- FROM: Especifica la entidad (o entidades) sobre la cual se realiza la consulta. Se requiere un alias para referirse a la entidad en el resto de la consulta (FROM Producto p).

- WHERE: Filtra los resultados de la consulta basándose en una o más condiciones. Las condiciones pueden involucrar atributos de la entidad, parámetros, funciones y operadores.

```
  SELECT p FROM Producto p WHERE p.precio > 100 AND p.categoria = 'Electrónicos'
```

- ORDER BY: Especifica el orden en el que se deben devolver los resultados. Es posible ordenar por uno o más atributos, en orden ascendente (ASC, por defecto) o descendente (DESC).
```
  SELECT p FROM Producto p ORDER BY p.precio DESC, p.nombre ASC
```

- GROUP BY: Agrupa los resultados basándose en los valores de uno o más atributos. Se utiliza a menudo con funciones agregadas.
```
  SELECT c.categoria, COUNT(p) FROM Producto p JOIN p.categoria c GROUP BY c.categoria
```

- HAVING: Filtra los resultados de los grupos creados por la cláusula GROUP BY. Se utiliza para aplicar condiciones a los grupos.
```
  SELECT c.categoria, COUNT(p) FROM Producto p JOIN p.categoria c GROUP BY c.categoria HAVING COUNT(p) > 10
```

<a id="consultas-con-parametros"></a>
#### Consultas con Parámetros
Las consultas con parámetros sirve para escribir consultas más flexibles y seguras (previniendo la inyección SQL), JPQL permite el uso de parámetros. Hay dos tipos de parámetros:

1. Parámetros Posicionales:
   
Se identifican por su posición en la consulta, precedidos por un signo de interrogación (?1, ?2, etc.). El índice comienza en 1.

```
SELECT p FROM Producto p WHERE p.precio > ?1 AND p.categoria = ?2
```

- Para establecer los valores, se utiliza el método setParameter(int position, Object value) del objeto Query.

2. Parámetros con Nombre: Se identifican por un nombre precedido por dos puntos (:nombreParametro).
   
```
SELECT p FROM Producto p WHERE p.precio > :precioMinimo AND p.categoria = :categoria
```
Para establecer los valores, se utiliza el método setParameter(String name, Object value) del objeto Query.

<a id="funciones-y-operadores"></a>
#### Funciones y Operadores
JPQL soporta una variedad de funciones y operadores que se pueden utilizar en las cláusulas WHERE y HAVING.

<a id="funciones-agregadas"></a>
##### Funciones agregadas
Las funciones agregadas operan sobre un conjunto de valores y devuelven un único valor. Las funciones agregadas comunes incluyen:

- COUNT(): Devuelve el número de elementos. Puede usarse con * para contar todas las filas o con un atributo para contar los valores no nulos de ese atributo. COUNT(DISTINCT atributo) cuenta los valores distintos no nulos.
- AVG(atributo): Devuelve el valor promedio de un atributo numérico.
- SUM(atributo): Devuelve la suma de los valores de un atributo numérico.
- MIN(atributo): Devuelve el valor mínimo de un atributo.
- MAX(atributo): Devuelve el valor máximo de un atributo.

> Ejemplo
```
SELECT AVG(p.precio) FROM Producto p WHERE p.categoria = 'Electrónicos'
```

<a id="funciones-de-cadena"></a>
##### Funciones de cadena
Las funciones de cadena permiten manipular o comparar valores de tipo cadena:

- CONCAT(cadena1, cadena2): Concatena dos cadenas.
- SUBSTRING(cadena, inicio, longitud): Extrae una subcadena.
- TRIM([LEADING|TRAILING|BOTH] [caracter] FROM cadena): Elimina espacios en blanco (o un carácter específico) al principio, al final o en ambos extremos de una cadena.
- LOWER(cadena): Convierte una cadena a minúsculas.
- UPPER(cadena): Convierte una cadena a mayúsculas.
- LENGTH(cadena): Devuelve la longitud de una cadena.
- LOCATE(subcadena, cadena [, inicio]): Devuelve la posición de la primera ocurrencia de una subcadena dentro de una cadena.

> Ejemplo
```
SELECT u FROM Usuario u WHERE LOWER(u.nombre) = 'juan'
```

<a id="operadores-logicos-y-de-comparacion"></a>
##### Operadores lógicos y de comparación

1. Operadores de Comparación:
- =, >, <, >=, <=, <> o !=, [NOT] BETWEEN ... AND ..., [NOT] LIKE ..., [NOT] IN (...), IS [NOT] NULL, IS [NOT] EMPTY (para colecciones).
```
SELECT p FROM Producto p WHERE p.precio BETWEEN 50 AND 150
SELECT u FROM Usuario u WHERE u.email LIKE '%@example.com'
SELECT p FROM Producto p WHERE p.categoria IN ('Libros', 'Música')
SELECT d FROM Departamento d WHERE d.empleados IS NOT EMPTY
```

2. Operadores Lógicos:
- AND, OR, NOT
```
SELECT p FROM Producto p WHERE p.precio > 100 AND (p.categoria = 'Electrónicos' OR p.marca = 'Sony')
```

<a id="relaciones-en-jpql"></a>
### Relaciones en JPQL (JOIN, FETCH JOIN)
JPQL permite navegar y consultar entidades relacionadas utilizando la cláusula JOIN.

1. JOIN (INNER JOIN): Devuelve resultados solo cuando hay coincidencias en ambas entidades relacionadas.
```
SELECT p, c FROM Producto p JOIN p.categoria c WHERE c.nombre = 'Electrónicos'
```

2. LEFT JOIN (LEFT OUTER JOIN): Devuelve todos los resultados de la entidad de la izquierda (en el FROM o el lado izquierdo del JOIN) y los datos coincidentes de la entidad de la derecha. Si no hay coincidencia, los atributos de la entidad de la derecha serán NULL.
```
SELECT d, e FROM Departamento d LEFT JOIN d.empleados e
```

3. RIGHT JOIN (RIGHT OUTER JOIN): Similar a LEFT JOIN pero devuelve todos los resultados de la entidad de la derecha y los datos coincidentes de la izquierda.

4. FETCH JOIN (JOIN FETCH): Se utiliza para cargar las entidades relacionadas de forma ansiosa (eagerly) en la misma consulta. Esto evita el problema del "N+1 select" que puede ocurrir con la carga perezosa.
```
SELECT p FROM Pedido p JOIN FETCH p.cliente WHERE p.id = :pedidoId
```

En este caso, al cargar el Pedido, también se cargará inmediatamente el Cliente asociado.

<a id="consultas-dinamicas"></a>
### Consultas dinámicas

<a id="criteria-api"></a>
### La API de Criterios (Criteria API)

<a id="construccion-de-consultas-programaticamente"></a>
#### Construcción de consultas programáticamente

<a id="predicados-ordenamiento-y-agrupacion"></a>
#### Predicados, ordenamientos y agrupaciones

<a id="consultas-con-relaciones"></a>
#### Consultas con relaciones

<a id="consultas-nativas"></a>
### Consultas Nativas (Native Queries)

<a id="sql-directo"></a>
#### Ejecución de SQL Directo

<a id="mapeo-de-resultado-en-consulta-nativa"></a>
#### Mapeo de resultados a entidades o escalares

<a id="transacciones-y-concurrencia"></a>
## Transacciones y concurrencia

<a id="gestion-de-transacciones"></a>
### Gestión de transacciones

<a id="transacciones-locales"></a>
#### Transacciones locales (Resource-Local)

<a id="transacciones-jta"></a>
#### Transacciones JTA

<a id="delimitacion-de-transacciones"></a>
### Delimitación de transacciones

<a id="inicio-commit-rollback"></a>
#### Inicio, commit y rollback

<a id="aislamiento-de-transacciones"></a>
### Aislamiento de transacciones

<a id="lectura-sucia"></a>
#### Lectura sucia

<a id="lectura-no-repetible"></a>
#### Lectura no repetible

<a id="lectura-fantasma"></a>
#### Lectura fantasma

<a id="bloqueos"></a>
### Bloqueos

<a id="optimistic"></a>
#### Optimistic

<a id="pessimistic-locking"></a>
#### Pessimistic Locking

<a id="contexto-de-persistencia"></a>
### Contexto de persistencia

<a id="cache-de-primer-nivel"></a>
#### Cache de primer nivel

<a id="ciclo-de-vida-de-las-entidadeS"></a>
#### Ciclo de vida de las entidades

<a id="el-estado-new"></a>
##### El estado New

<a id="el-estado-managed"></a>
##### El estado Managed

<a id="el-estado-detached"></a>
##### El estado Detached

<a id="el-estado-removed"></a>
##### El estado Removed

<a id="operaciones-de-entity-manager"></a>
### Operaciones del EntityManager
 
<a id="metodos-comunes-del-entity-manager"></a>
#### Métodos persist(), merge(), remove(), find(), refresh(), detach(), clear() y flush()
