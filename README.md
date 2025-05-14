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
 - [Fundamentos de Spring Data JPA](#fundamentos-de-spring-data-jpa)
    - [¿Qué es Spring Data JPA](#que-es-spring-data-jpa)
    - [Configuración de Spring Data JPA](#configuracion-de-spring-data-jpa)
    - [Repositorios en Spring Data JPA](#repositorios-en-spring-jpa)
    - [Derivación de consultas](#derivacion-de-consultas)
        - [Uso de palabras clave](#palabras-clave)
        - [Ejemplos de derivación de consultas](#ejemplos-de-derivacion-de-consultas)
    - [La anotación @Query](#la-anotacion-query)
        - [Consultas JPQL con @Query](#consultas-jpql-con-query)
        - [Consultas nativas con @Query](#consultas-nativas-con-query)
    - [Paginación y ordenamiento](#paginacion-y-ordenamiento)
        - [Ejemplos con la interfaz Pageable](#ejemplos-con-la-interfaz-pageable)
        - [Ejemplos con la interfaz Sort](#ejemplos-con-la-interfaz-sort)
        - [Ejemplos con la interfaz PageRequest](#ejemplos-con-la-interfaz-pagerequest)
    - [Auditoria](#auditoria)
        - [Uso de la anotación @CreatedDate](#la-anotacion-created-date)
        - [Uso de la anotación @LastModifiedDate](#la-anotacion-last-modified-date)
        - [Uso de la anotación @CreatedBy](#la-anotacion-created-by)
        - [Uso de la anotación @LastModifiedBy ](#la-anotacion-last-modified-by)
    - [Eventos de Repositorio](#eventos-de-repositorio)
        - [BeforeSaveEvent](#before-save-event)
        - [AfterSaveEvent](#after-save-event)
        - [Otro tipo de eventos](#otro-tipo-de-eventos)
    - [Proyecciones](#proyeccioones-en-jpa)
    - [Consultas asincronas](#consultas-asincronas)
        - [La clase Future](#la-clase-future)
        - [La clase ListenableFuture](#la-clase-listeneable-future)
        - [La clase CompletableFuture](#la-clase-completable-future)
    - [Transacciones en Spring Data JPA](#transacciones-en-spring-data-jpa)
        - [La anotación @Transactional](#la-anotacion-transactional)
    - [Manejo de excepciones de Spring Data JPA](#manejo-de-excepciones)
   
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
A veces, las condiciones de la consulta no se conocen en tiempo de compilación y deben construirse dinámicamente en tiempo de ejecución basándose en la entrada del usuario u otros factores. Si bien se pueden construir cadenas JPQL dinámicamente, esto puede ser propenso a errores y más difícil de mantener. La API de Criterios ofrece una forma más segura y orientada a objetos de construir consultas dinámicamente.


<a id="criteria-api"></a>
### La API de Criterios (Criteria API)
La API de Criterios proporciona una forma programática de construir consultas JPQL utilizando objetos Java en lugar de cadenas de texto. Esto ofrece beneficios como seguridad de tipos, detección de errores en tiempo de compilación (en muchos casos) y una estructura más orientada a objetos para construir consultas complejas

<a id="construccion-de-consultas-programaticamente"></a>
#### Construcción de consultas programáticamente

La construcción de una consulta de criterios implica los siguientes pasos principales:

1.  Obtener una instancia de CriteriaBuilder: Se obtiene del EntityManager.
```
    CriteriaBuilder cb = entityManager.getCriteriaBuilder();
```

2. Crear una instancia de CriteriaQuery: Representa la consulta que se va a ejecutar. Se especifica el tipo de resultado esperado (por ejemplo, una entidad Producto).
```
    CriteriaQuery<Producto> cq = cb.createQuery(Producto.class);
```

3. Definir la raíz de la consulta (Root): Representa la entidad principal sobre la cual se está consultando (la entidad en la cláusula FROM de JPQL)
```
    Root<Producto> productoRoot = cq.from(Producto.class);
```

4. Seleccionar los resultados (Selection): Especifica qué se va a recuperar (la entidad raíz, un atributo, etc.).
```
    cq.select(productoRoot); // Para seleccionar la entidad completa
    cq.select(productoRoot.get("nombre")); // Para seleccionar el atributo "nombre"
```

5. Agregar predicados (Predicate - para la cláusula WHERE): Se construyen utilizando métodos del CriteriaBuilder y se aplican a la CriteriaQuery
```
    Predicate precioMayorQue = cb.greaterThan(productoRoot.get("precio"), 100.0);
    Predicate categoriaElectronicos = cb.equal(productoRoot.get("categoria"), "Electrónicos");
    cq.where(cb.and(precioMayorQue, categoriaElectronicos));
```

6. Especificar el ordenamiento (Order - para la cláusula ORDER BY)
```
    cq.orderBy(cb.desc(productoRoot.get("precio")), cb.asc(productoRoot.get("nombre")));
```

7. Especificar la agrupación (Expression - para la cláusula GROUP BY) y la condición de grupo (Predicate - para la cláusula HAVING)
```
    Expression<String> categoriaExp = productoRoot.get("categoria");
    cq.groupBy(categoriaExp);
    cq.having(cb.greaterThan(cb.count(productoRoot), 10L));
```

8. Crear y ejecutar la consulta: Se crea una instancia de Query a partir de la CriteriaQuery y se ejecuta
```
   Query<Producto> query = entityManager.createQuery(cq);
   List<Producto> resultados = query.getResultList();
```

<a id="predicados-ordenamiento-y-agrupacion"></a>
#### Predicados, ordenamientos y agrupaciones
Como se pudo observar, la API de Criterios utiliza objetos para **representar las diferentes partes** de una consulta:

- Predicate: Representa una condición (como las de la cláusula WHERE o HAVING). Se construyen utilizando los métodos del CriteriaBuilder (por ejemplo, equal, greaterThan, lessThan, like, in, isNull, and, or, not).

- Order: Representa un criterio de ordenamiento (para la cláusula ORDER BY). Se crean con cb.asc() para ascendente y cb.desc() para descendente, pasando la expresión del atributo por el cual ordenar.

- Expression: Representa un atributo de la entidad o el resultado de una función (por ejemplo, para la cláusula GROUP BY o para funciones agregadas)

<a id="consultas-con-relaciones"></a>
#### Consultas con relaciones
La API de Criterios también permite **navegar y realizar joins entre entidades relacionadas** utilizando el método join() de la raíz (Root) o de un join existente. Se pueden especificar diferentes tipos de joins (INNER, LEFT, RIGHT) utilizando los métodos correspondientes (join(atributo, JoinType.INNER), join(atributo, JoinType.LEFT), etc.). Para realizar un FETCH JOIN, se utiliza el método fetch().


> Ejemplo
```
    CriteriaQuery<Pedido> cq = cb.createQuery(Pedido.class);
    Root<Pedido> pedidoRoot = cq.from(Pedido.class);
    Join<Pedido, Cliente> clienteJoin = pedidoRoot.join("cliente", JoinType.INNER); // "cliente" es el nombre del atributo de relación en Pedido
    cq.select(pedidoRoot).where(cb.equal(clienteJoin.get("nombre"), "Juan"));
    
    // Fetch Join
    CriteriaQuery<Pedido> cqFetch = cb.createQuery(Pedido.class);
    Root<Pedido> pedidoRootFetch = cqFetch.from(Pedido.class);
    pedidoRootFetch.fetch("cliente", JoinType.LEFT);
    cqFetch.select(pedidoRootFetch).where(cb.equal(pedidoRootFetch.get("id"), 1L));
```

<a id="consultas-nativas"></a>
### Consultas Nativas (Native Queries)
En situaciones donde JPQL o la API de Criterios no pueden expresar una consulta específica (por ejemplo, el uso de funciones específicas de la base de datos o consultas muy complejas), JPA permite ejecutar consultas SQL nativas directamente contra la base de datos.

> [!IMPORTANT]
> Las consultas nativas ofrecen la máxima flexibilidad pero sacrifican la portabilidad entre bases de datos, ya que están ligadas al dialecto SQL específico de la base de datos utilizada.

<a id="sql-directo"></a>
#### Ejecución de SQL Directo
Las consultas nativas se crean utilizando el método createNativeQuery() del EntityManager. Este método puede tomar una cadena SQL como argumento y, opcionalmente, la clase de la entidad a la que se mapearán los resultados.

> Ejemplo
```
    // Consulta nativa que devuelve entidades Producto
    Query<Producto> nativeQuery = entityManager.createNativeQuery("SELECT * FROM productos WHERE precio > :precioMinimo", Producto.class);
    nativeQuery.setParameter("precioMinimo", 100.0);
    List<Producto> productos = nativeQuery.getResultList();
    
    // Consulta nativa que devuelve resultados escalares (no entidades)
    Query<?> scalarNativeQuery = entityManager.createNativeQuery("SELECT nombre, precio FROM productos WHERE categoria = ?1");
    scalarNativeQuery.setParameter(1, "Libros");
    List<?> resultadosEscalares = scalarNativeQuery.getResultList();
    for (Object[] resultado : (List<Object[]>) resultadosEscalares) {
        String nombre = (String) resultado[0];
        BigDecimal precio = (BigDecimal) resultado[1];
    }
```

<a id="mapeo-de-resultado-en-consulta-nativa"></a>
#### Mapeo de resultados a entidades o escalares

- Mapeo a Entidades: Si se proporciona la clase de la entidad al crear la consulta nativa, JPA **intentará mapear las columnas de la tabla resultante a los atributos de la entidad**. Los nombres de las columnas deben coincidir con los nombres de las columnas definidos en el mapeo de la entidad (a través de @Column o por defecto).

- Mapeo a Resultados Escalares: Si no se proporciona una clase de entidad, la consulta nativa **devolverá una lista de arrays de objetos** (List<Object[]>), donde cada array representa una fila y los elementos del array son los valores de las columnas seleccionadas. Se debe de realizar el casting adecuado a los tipos de datos esperados.

- Mapeo de Resultados Complejos: Para mapear consultas nativas a conjuntos de resultados más complejos (por ejemplo, cuando la consulta involucra joins y se quieren devolver objetos que no son entidades), se pueden utilizar result set mappings definidos con la anotación @SqlResultSetMapping.

<a id="transacciones-y-concurrencia"></a>
## Transacciones y concurrencia
La gestión de transacciones y la concurrencia son aspectos cruciales en cualquier sistema de persistencia de datos. JPA proporciona mecanismos para asegurar la atomicidad, consistencia, aislamiento y durabilidad (ACID) de las operaciones, así como estrategias para manejar el acceso simultáneo a los datos por múltiples usuarios o procesos.

<a id="gestion-de-transacciones"></a>
### Gestión de transacciones
Una transacción es una secuencia de operaciones que se tratan como una sola unidad lógica de trabajo. Todas las operaciones dentro de una transacción deben completarse con éxito (commit) para que los cambios se guarden en la base de datos, o todas deben deshacerse (rollback) para mantener la integridad de los datos. 

JPA soporta dos modelos principales de gestión de transacciones: locales (resource-local) y JTA (Java Transaction API).

<a id="transacciones-locales"></a>
#### Transacciones locales (Resource-Local)
En las transacciones locales, **la gestión de la transacción está directamente controlada por el proveedor de persistencia** (por ejemplo, Hibernate) y está estrechamente ligada a la conexión de la base de datos

> [!NOTE]
> Común en aplicaciones Java SE o en entornos donde no se dispone de un administrador de transacciones JTA.

> Ejemplo
```
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("miUnidadPersistencia");
    EntityManager em = emf.createEntityManager();
    EntityTransaction tx = em.getTransaction();
    
    try {
        tx.begin();
        // Realizar operaciones de persistencia (persist, merge, remove, etc.)
        Cliente cliente = em.find(Cliente.class, 1L);
        cliente.setNombre("Nuevo Nombre");
        tx.commit();
    } catch (Exception e) {
        if (tx != null && tx.isActive()) {
        tx.rollback();
    }
    } finally {
        em.close();
        emf.close();
    }
```

<a id="transacciones-jta"></a>
#### Transacciones JTA
JTA **es una API estándar de Java para la gestión de transacciones distribuidas**. Permite coordinar transacciones que pueden involucrar múltiples recursos (por ejemplo, múltiples bases de datos, colas de mensajes). La gestión de las transacciones JTA **se realiza por un administrador de transacciones externo**.

En un entorno JTA, la delimitación de la transacción a menudo **se realiza de forma declarativa utilizando anotaciones como @Transactional** (de Jakarta EE o Spring)

> Ejemplo
```
import jakarta.ejb.Stateless;
import jakarta.persistence.EntityManager;
import jakarta.persistence.PersistenceContext;
import jakarta.transaction.Transactional;

@Stateless
public class ServicioCliente {

    @PersistenceContext
    private EntityManager em;

    @Transactional
    public void actualizarNombreCliente(Long id, String nuevoNombre) {
        Cliente cliente = em.find(Cliente.class, id);
        if (cliente != null) {
            cliente.setNombre(nuevoNombre);
        }
        // Si ocurre una excepción no controlada dentro de este método,
        // la transacción se marcará para rollback automáticamente.
    }
}
```

<a id="delimitacion-de-transacciones"></a>
### Delimitación de transacciones
La delimitación de transacciones se refiere a definir el inicio y el final de una transacción. Esto puede hacerse de forma programática (manualmente en el código) o de forma declarativa.

<a id="inicio-commit-rollback"></a>
#### Inicio, commit y rollback

- Inicio (Begin): **Marca el comienzo de una nueva unidad de trabajo**. En transacciones locales, se llama a tx.begin(). En JTA, la transacción puede iniciarse implícitamente al entrar en un método transaccional o explícitamente a través de la API JTA.

- Commit: **Si todas las operaciones dentro de la transacción se completan con éxito, se realiza un commit**. Esto hace que todos los cambios realizados en la base de datos durante la transacción sean permanentes. En transacciones locales, se llama a tx.commit(). En JTA, el commit es gestionado por el administrador de transacciones al finalizar la unidad de trabajo transaccional.

- Rollback: Si ocurre algún error durante la transacción, o si se decide deshacer los cambios, se realiza un rollback. **Esto revierte la base de datos al estado en el que se encontraba antes de que comenzara la transacción**. En transacciones locales, se llama a tx.rollback(). En JTA, el rollback puede ser iniciado por el código o por el administrador de transacciones en caso de una excepción no controlada.

<a id="aislamiento-de-transacciones"></a>
### Aislamiento de transacciones
El aislamiento de transacciones define el grado en que una transacción debe estar aislada de las operaciones realizadas por otras transacciones concurrentes. Un mayor nivel de aislamiento proporciona mayor consistencia pero puede reducir la concurrencia. JPA delega el manejo de los niveles de aislamiento al proveedor de persistencia y a la base de datos subyacente.

> Niveles de Aislamiento Estándar (de menor a mayor aislamiento)

1. Read Uncommitted: Permite lecturas sucias.

2. Read Committed: Asegura que una transacción solo lee datos que han sido confirmados por otras transacciones. Evita las lecturas sucias pero puede permitir lecturas no repetibles y lecturas fantasma.

3. Repeatable Read: Asegura que si una transacción lee una fila varias veces, siempre verá los mismos datos (a menos que la propia transacción los modifique). Evita las lecturas sucias y las lecturas no repetibles, pero puede permitir lecturas fantasma.

4. Serializable: Proporciona el nivel de aislamiento más alto. Garantiza que las transacciones concurrentes se ejecuten de forma que parezca que se ejecutaron secuencialmente. Evita lecturas sucias, lecturas no repetibles y lecturas fantasma.

<a id="lectura-sucia"></a>
#### Lectura sucia
Ocurre cuando una transacción lee datos que han sido modificados por otra transacción que aún no ha sido confirmada (commit). Si la segunda transacción realiza un rollback, la primera transacción habrá leído datos que nunca fueron realmente válidos. Este es el nivel de aislamiento más bajo.

<a id="lectura-no-repetible"></a>
#### Lectura no repetible
Ocurre cuando una transacción lee la misma fila dos veces durante su ejecución, y los valores son diferentes. Esto se debe a que otra transacción ha modificado esa fila y ha hecho commit entre las dos lecturas de la primera transacción.

<a id="lectura-fantasma"></a>
#### Lectura fantasma
Ocurre cuando una transacción ejecuta la misma consulta dos veces y el conjunto de filas devuelto es diferente. Esto se debe a que otra transacción ha insertado o eliminado filas que coinciden con la consulta de la primera transacción y ha hecho commit entre las dos ejecuciones.

<a id="bloqueos"></a>
### Bloqueos
JPA proporciona mecanismos para controlar el acceso concurrente a las entidades y prevenir problemas de integridad de datos. Estos mecanismos se conocen como bloqueos (locking) y pueden ser optimistas o pesimistas.

<a id="optimistic"></a>
#### Optimistic
El bloqueo optimista asume que los conflictos entre transacciones concurrentes son raros. En lugar de bloquear explícitamente las filas en la base de datos, **JPA utiliza un campo de versión (anotado con @Version) en la entidad para detectar modificaciones concurrentes**. 

- Funcionamiento: 

1. Cuando una entidad es leída, su valor de versión se almacena.
2. Antes de que la transacción que modificó la entidad realice el commit, el proveedor de JPA verifica si el valor de la versión en la base de datos coincide con el valor de la versión que se leyó al inicio de la transacción.
3. Si los valores de versión coinciden, la transacción se confirma y el valor de la versión se incrementa. 
4. Si los valores de versión no coinciden, significa que otra transacción ha modificado la entidad entre la lectura y la escritura de la transacción actual. En este caso, se lanza una excepción (javax.persistence.OptimisticLockException), y la transacción actual debe realizar un rollback y posiblemente reintentar la operación.

- Ventajas:
1. Mayor concurrencia ya que no se mantienen bloqueos exclusivos durante largos períodos.
2. Menor sobrecarga en el sistema si los conflictos son raros.

- Desventajas:
1. Los conflictos solo se detectan en el momento del commit, lo que puede llevar a la pérdida de trabajo si una transacción larga falla debido a un bloqueo optimista.
2. Requiere que la aplicación maneje las excepciones de bloqueo optimista y posiblemente reintente las operaciones.

<a id="pessimistic-locking"></a>
#### Pessimistic Locking
El bloqueo pesimista asume que los conflictos entre transacciones concurrentes son probables. Por lo tanto, bloquea explícitamente las filas correspondientes a las entidades en la base de datos tan pronto como se acceden dentro de una transacción, impidiendo que otras transacciones las modifiquen hasta que se libere el bloqueo (al hacer commit o rollback).

- Funcionamiento:
1. Al leer una entidad que se va a modificar, la transacción adquiere un bloqueo en la base de datos para esa fila (o filas relacionadas).
2. Otras transacciones que intenten acceder a las mismas filas tendrán que esperar hasta que la primera transacción libere el bloqueo
3. El bloqueo se libera cuando la transacción que lo adquirió realiza un commit o un rollback

- Ventajas:

1. Evita los problemas de concurrencia al impedir que otras transacciones modifiquen los datos que se están utilizando.
2. La detección de conflictos ocurre inmediatamente al intentar acceder a los datos bloqueados.

- Desventajas:
1. Reduce la concurrencia ya que las transacciones pueden tener que esperar por los bloqueos.
2. Puede llevar a problemas de rendimiento y a posibles interbloqueos (deadlocks) si los bloqueos se mantienen durante mucho tiempo o si las transacciones adquieren bloqueos en un orden inconsistente.


> Tipos de Bloqueo Pesimista en JPA (a través de LockModeType en los métodos find() o al crear consultas):

1. LockModeType.PESSIMISTIC_READ: Adquiere un bloqueo de lectura compartido. Permite que otras transacciones lean los datos pero no los modifiquen.
2. LockModeType.PESSIMISTIC_WRITE: Adquiere un bloqueo de escritura exclusivo. Impide que otras transacciones lean o modifiquen los datos.
3. LockModeType.PESSIMISTIC_FORCE_INCREMENT: Similar a PESSIMISTIC_WRITE pero también fuerza el incremento del campo de versión (si existe) al adquirir el bloqueo.

<a id="contexto-de-persistencia"></a>
### Contexto de persistencia
El contexto de persistencia es un entorno gestionado que **contiene las instancias de las entidades que están siendo gestionadas por un EntityManager**. Actúa como una caché de primer nivel para las entidades.

<a id="cache-de-primer-nivel"></a>
#### Cache de primer nivel
La caché de primer nivel es una caché a nivel de EntityManager. Dentro de la misma instancia de EntityManager y durante la misma transacción, **si se solicita la misma entidad varias veces por su clave primaria, se devolverá la misma instancia de objeto de la caché en lugar de acceder a la base de datos nuevamente** (a menos que la caché haya sido invalidada o la entidad haya sido desasociada).

- Características:
1. La caché es privada para cada instancia de EntityManager y no se comparte entre diferentes EntityManagers, incluso si pertenecen a la misma EntityManagerFactory.
2. La caché existe mientras el EntityManager esté abierto. **Se destruye cuando se cierra el EntityManager**.
3. El EntityManager es responsable de sincronizar el estado de las entidades en la caché con la base de datos durante el commit de la transacción o cuando se llama al método flush().

<a id="ciclo-de-vida-de-las-entidadeS"></a>
#### Ciclo de vida de las entidades
Una entidad en JPA puede estar en uno de los siguientes estados en relación con el contexto de persistencia:

<a id="el-estado-new"></a>
##### El estado New
Una entidad está en estado New (o Transient) cuando **acaba de ser instanciada y aún no está asociada con ningún contexto de persistencia**. No tiene una representación correspondiente en la base de datos (a menos que ya exista con la misma clave primaria)

- Características:
1. No está gestionada por el EntityManager.
2. Las operaciones de persistencia realizadas en esta entidad no se propagan automáticamente a la base de datos.

<a id="el-estado-managed"></a>
##### El estado Managed
Una entidad está en estado Managed cuando **está asociada con un contexto de persistencia y tiene una representación correspondiente en la base de datos**. Esto ocurre después de que una entidad New es persistida (con persist()), o cuando una entidad existente se recupera de la base de datos (con find() o una consulta).

- Características:
1. Está gestionada por el EntityManager.
2. Cualquier cambio realizado en los atributos de una entidad Managed dentro de una transacción se detectará automáticamente (dirty checking) y se sincronizará con la base de datos durante el flush() o el commit() de la transacción.

<a id="el-estado-detached"></a>
##### El estado Detached
Una entidad está en estado Detached cuando **previamente estuvo Managed pero ya no está asociada con ningún contexto de persistencia**. Esto puede ocurrir después de que **el EntityManager que la gestionaba se ha cerrado**, o después de que la entidad ha sido explícitamente desasociada (con detach() o clear())

> [!IMPORTANT]
> Para volver a asociar una entidad Detached con un contexto de persistencia y sincronizar sus cambios, se utiliza el método merge().

<a id="el-estado-removed"></a>
##### El estado Removed

Una entidad está en estado Removed cuando **ha sido marcada para su eliminación de la base de datos**. Esto ocurre después de que se llama al método remove() en una entidad Managed. La **eliminación real de la base de datos ocurre durante el flush() o el commit()** de la transacción.

- Características:
1. Está gestionada por el EntityManager (hasta que se elimina de la base de datos).
2. Será eliminada de la base de datos al finalizar la transacción.
3. Una vez en estado Removed, las operaciones posteriores en la entidad pueden tener un comportamiento indefinido.

<a id="operaciones-de-entity-manager"></a>
### Operaciones del EntityManager
El EntityManager proporciona varios métodos para gestionar el ciclo de vida de las entidades y para interactuar con el contexto de persistencia y la base de datos.

<a id="metodos-comunes-del-entity-manager"></a>
#### Métodos persist(), merge(), remove(), find(), refresh(), detach(), clear() y flush()
-  persist(entity): Hace que una entidad New se convierta en Managed, lo que significa que se asocia con el contexto de persistencia y se planifica su inserción en la base de datos (la inserción real ocurre durante el flush() o commit()). 

> [!NOTE]
> Si la clave primaria se genera automáticamente, puede que no esté disponible inmediatamente después de la llamada a persist() hasta que se realice el flush.

- merge(entity): Fusiona el estado de una entidad Detached con el contexto de persistencia. Si una entidad con la misma identidad ya existe en el contexto de persistencia, se actualiza con el estado de la entidad Detached. Si no existe, se crea una nueva instancia Managed y se copia el estado de la entidad Detached. merge() devuelve la instancia Managed resultante (que puede o no ser la misma instancia que se pasó como argumento).

- remove(entity): Marca una entidad Managed para su eliminación de la base de datos. La eliminación real ocurre durante el flush() o commit(). 

> [!NOTE]
> La entidad debe estar en estado Managed para poder ser removida.

- find(entityClass, primaryKey): Busca una entidad por su clave primaria. Primero busca en la caché de primer nivel. Si no se encuentra, intenta recuperarla de la base de datos y la coloca en el estado Managed. Si no se encuentra en la base de datos, devuelve null.

- refresh(entity): Actualiza el estado de una entidad Managed desde la base de datos, sobrescribiendo cualquier cambio que se haya realizado en la entidad en el contexto de persistencia. Es útil para descartar cambios locales y obtener el estado actual de la base de datos.

- detach(entity): Desasocia una entidad Managed del contexto de persistencia, moviéndola al estado Detached. Los cambios futuros realizados en la entidad detached no serán rastreados ni sincronizados automáticamente con la base de datos. Si se desea volver a gestionar la entidad y sincronizar sus cambios, se debe de utilizar el método merge().

- clear(): Desasocia todas las entidades Managed del contexto de persistencia. Esto significa que la caché de primer nivel se vacía. Las entidades que estaban Managed pasan al estado Detached. 

- flush(): Sincroniza los cambios realizados en el contexto de persistencia con la base de datos. Esto incluye las operaciones de persist(), merge(), remove(), refresh(), detach() y clear().

> [!NOTE]
>  Llamar a flush() puede ser útil para forzar la escritura de los cambios en la base de datos en un punto específico dentro de una transacción, por ejemplo, para hacer que los cambios sean visibles para otras operaciones dentro de la misma transacción o para verificar si hay errores antes de realizar el commit final.

<a id="fundamentos-de-spring-data-jpa"></a>
## Fundamentos de Spring Data JPA
Spring Data JPA **es un submódulo del proyecto Spring Data** que tiene como objetivo simplificar la capa de acceso a datos en aplicaciones que utilizan Java Persistence API (JPA) para la persistencia en bases de datos relacionales. Se basa en los conceptos de los repositorios y elimina gran parte del código boilerplate que normalmente se escribe para interactuar con JPA.

<a id="que-es-spring-data-jpa"><a/>
### ¿Qué es Spring Data JPA?
Spring Data JPA **se sitúa como una capa de abstracción sobre los proveedores de JPA** (como Hibernate, EclipseLink, etc.). Su principal objetivo es reducir significativamente la cantidad de código necesario para implementar la capa de acceso a datos (el Data Access Object o DAO).

> Puntos clave de Spring Data JPA

1. El concepto central de Spring Data JPA son los repositorios. En lugar de implementar clases DAO manualmente, se definen interfaces que extienden interfaces específicas proporcionadas por Spring Data JPA (como CrudRepository o JpaRepository). **Spring Data JPA se encarga de generar automáticamente la implementación de estas interfaces en tiempo de ejecución**.

2.  Una de las características más potentes es la capacidad de derivar consultas automáticamente a partir de los nombres de los métodos definidos en las interfaces de repositorio. Siguiendo ciertas convenciones de nomenclatura.

3. Para consultas más complejas que no se pueden derivar fácilmente, Spring Data JPA permite definir consultas JPQL o SQL nativas directamente en los métodos de repositorio utilizando la anotación @Query

<a id="configuracion-de-spring-data-jpa"><a/>
### Configuración de Spring Data JPA
Para utilizar Spring Data JPA dentro de un proyecto Spring, es necesario realizar algunos pasos de configuración:

1. Añadir las Dependencias: Se deben de incluir la dependencia de Spring Data JPA. También son necesarias la dependencia de un proveedor de JPA (como Hibernate) y el driver JDBC para la base de datos.

- Maven (pom.xml):
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-core</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

- Gradle (build.gradle):
```
  implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
  implementation 'org.hibernate.orm:hibernate-core'
  runtimeOnly 'com.h2database:h2'
```

2. Se debe configurar cómo la aplicación se conecta a la base de datos. Esto generalmente se hace en el archivo de propiedades de Spring (application.properties o application.yml).

- application.properties:
```
  spring.datasource.url=jdbc:h2:mem:testdb
  spring.datasource.driver-class-name=org.h2.Driver
  spring.datasource.username=sa
  spring.datasource.password=
  spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
  spring.jpa.hibernate.ddl-auto=create-drop
```


<a id="repositorios-en-spring-jpa"><a/>
### Repositorios en Spring Data JPA
Los repositorios son interfaces que extienden interfaces base proporcionadas por Spring Data JPA. Estas interfaces base ya definen métodos para las operaciones CRUD y otras funcionalidades comunes. Spring Data JPA proporciona dos interfaces principales como punto de partida:

1. CrudRepository<T, ID> : Proporciona métodos básicos para realizar operaciones CRUD en una entidad de tipo T con un tipo de ID. Los métodos incluyen save(), findById(), existsById(), findAll(), deleteById(), delete(), deleteAll(), count().

2. JpaRepository<T, ID>: Extiende CrudRepository y añade métodos específicos de JPA, como soporte para flushing (flush(), saveAndFlush()), operaciones por lotes y la capacidad de realizar paginación y ordenamiento a través de los métodos findAll(Pageable pageable) y findAll(Sort sort).

Para crear un repositorio personalizado, simplemente se define una interfaz que **extiende una de estas interfaces base**, especificando el tipo de la entidad que gestiona y el tipo de su ID

> Ejemplo
```
import org.springframework.data.jpa.repository.JpaRepository;
import com.example.entity.Usuario;

public interface UsuarioRepository extends JpaRepository<Usuario, Long> {
    // Aquí se pueden añadir métodos de consulta personalizados
}
```
Spring Data JPA analizará esta interfaz y generará automáticamente una implementación que proporciona los métodos definidos en JpaRepository (y por lo tanto, en CrudRepository).

Se puede inyectar una instancia de UsuarioRepository dentro de servicios o controladores y utilizar sus métodos para **interactuar con la base de datos sin escribir ninguna implementación de DAO explícita**.


<a id="derivacion-de-consultas"><a/>
### Derivación de consultas
La derivación de consultas es una de las características más convenientes de Spring Data JPA. Permite definir métodos de consulta simplemente siguiendo ciertas convenciones de nomenclatura en las interfaces de tus repositorios. Spring Data JPA analiza el nombre del método y genera automáticamente la consulta JPA subyacente.

<a id="palabras-clave"><a/>
#### Uso de palabras clave
Spring Data JPA soporta una amplia gama de palabras clave que se pueden utilizar en los nombres de tus métodos de repositorio para definir las condiciones de las consultas. Algunas de las palabras clave más comunes incluyen:

- findBy...: Busca entidades que coinciden con las condiciones especificadas después de By.
- countBy...: Devuelve el número de entidades que coinciden con las condiciones.
- existsBy...: Devuelve true si existen entidades que coinciden con las condiciones, false en caso contrario.
- deleteBy...: Elimina las entidades que coinciden con las condiciones.
- Operadores lógicos: And, Or.
- Operadores de comparación: Between, LessThan, GreaterThan, LessThanEqual, GreaterThanEqual, After, Before.
- Operadores de cadena: Like, StartingWith, EndingWith, Containing.
- Operadores booleanos: True, False.
- Operadores de nulidad: IsNull, IsNotNull.
- Operadores de colección: In, NotIn, IsEmpty, IsNotEmpty, Contains, DoesNotContain.
- Ordenamiento: OrderBy.

> [!NOTE]
> Después de la palabra clave By, se especifican los atributos de la entidad sobre los que se quiere realizar la consulta, utilizando los nombres de los atributos de la clase de la entidad. Si se necesita combinar múltiples condiciones, se utilizan las palabras And u Or.

<a id="ejemplos-de-derivacion-de-consultas"><a/>
#### Ejemplos de derivación de consultas
Consideremos una entidad Usuario con los atributos id (Long), nombre (String), email (String) y activo (boolean):

- findByNombre(String nombre): Genera una consulta que busca usuarios por su nombre. (WHERE nombre = ?1)

- findByEmailAndActivo(String email, boolean activo): Busca usuarios por su email y si están activos. (WHERE email = ?1 AND activo = ?2)

- findByNombreLike(String patron): Busca usuarios cuyo nombre coincide con el patrón LIKE. (WHERE nombre LIKE ?1)

- findByEdadBetween(int minEdad, int maxEdad): Busca usuarios cuya edad está entre un rango. (WHERE edad BETWEEN ?1 AND ?2)

- findByFechaCreacionAfter(Date fecha): Busca usuarios creados después de una fecha específica. (WHERE fechaCreacion > ?1)

- countByActivo(boolean activo): Cuenta el número de usuarios activos o inactivos. (SELECT COUNT(*) FROM Usuario WHERE activo = ?1)

- deleteByEmail(String email): Elimina los usuarios con un email específico. (DELETE FROM Usuario WHERE email = ?1)

- findByDireccionCodigoPostalIn(List<String> codigos): Busca usuarios cuya dirección tiene un código postal en la lista proporcionada. (WHERE direccion.codigoPostal IN (?1))

- findByNombreOrderByFechaCreacionDesc(String nombre): Busca usuarios por nombre y los ordena por fecha de creación descendente. (WHERE nombre = ?1 ORDER BY fechaCreacion DESC)

<a id="la-anotacion-query"><a/>
### La anotación @Query
Para consultas más complejas o cuando se necesite un control total sobre la consulta JPA que se ejecuta, se puede utilizar la anotación @Query directamente en los métodos del repositorio. Esta anotación te permite escribir consultas JPQL o SQL nativas.

<a id="consultas-jpql-con-query"><a/>
#### Consultas JPQL con @Query
Dentro de la anotación @Query, puedes especificar una cadena JPQL que se ejecutará cuando se llame al método del repositorio. Puedes utilizar parámetros en tu consulta JPQL y enlazarlos a los argumentos del método utilizando la notación :nombreParametro o ?indiceParametro

> Ejemplo
```
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import com.example.entity.Producto;
import java.util.List;

public interface ProductoRepository extends JpaRepository<Producto, Long> {

    @Query("SELECT p FROM Producto p WHERE p.nombre LIKE %:patron%")
    List<Producto> buscarPorNombreConteniendo(@Param("patron") String patron);

    @Query("SELECT p FROM Producto p WHERE p.precio BETWEEN :minPrecio AND :maxPrecio ORDER BY p.nombre ASC")
    List<Producto> buscarPorRangoDePrecioOrdenado(@Param("minPrecio") double minPrecio, @Param("maxPrecio") double maxPrecio);

    @Query("SELECT COUNT(p) FROM Producto p WHERE p.categoria = :categoria")
    int contarProductosPorCategoria(@Param("categoria") String categoria);
}
```

<a id="consultas-nativas-con-query"><a/>
#### Consultas nativas con @Query
Si se necesita ejecutar una consulta SQL directamente (por ejemplo, para utilizar funciones específicas de la base de datos), se puede establecer el atributo nativeQuery de la anotación @Query en true. En este caso, la cadena dentro de @Query se interpretará como SQL nativo.

> Ejemplo
```
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import com.example.entity.Usuario;
import java.util.List;

public interface UsuarioRepository extends JpaRepository<Usuario, Long> {

    @Query(value = "SELECT u.id, u.nombre, u.email FROM usuarios u WHERE u.nombre LIKE %?1%", nativeQuery = true)
    List<Object[]> buscarUsuariosPorNombreNativo(String patron);

    @Query(value = "SELECT * FROM usuarios WHERE email = :email", nativeQuery = true)
    Usuario buscarPorEmailNativo(@Param("email") String email);
}
```

<a id="paginacion-y-ordenamiento"><a/>
### Paginación y ordenamiento
Spring Data JPA simplifica enormemente la implementación de la paginación y el ordenamiento en tus consultas a través de las interfaces Pageable y Sort.

<a id="ejemplos-con-la-interfaz-pageable"><a/>
#### Ejemplos con la interfaz Pageable
La interfaz Pageable representa una solicitud de paginación. **Contiene información sobre el número de página que se solicita, el tamaño de la página y los criterios de ordenamiento**. Se puede pasar una instancia de Pageable como argumento a los métodos de los repositorios para obtener resultados paginados.

Spring Data JPA proporciona el tipo concreto **PageRequest como una implementación de Pageable**. Se pueden crear instancias de PageRequest especificando el número de página (la primera página es 0), el tamaño de la página y opcionalmente un objeto Sort.

> Ejemplo
```
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.jpa.repository.JpaRepository;
import com.example.entity.Producto;

public interface ProductoRepository extends JpaRepository<Producto, Long> {

    Page<Producto> findAll(Pageable pageable);

    @Query("SELECT p FROM Producto p WHERE p.categoria = :categoria")
    Page<Producto> buscarPorCategoria(@Param("categoria") String categoria, Pageable pageable);
}
```
Spring Data JPA ejecutará una consulta que recupera solo la página de resultados solicitada y también ejecutará una consulta de conteo para determinar el número total de elementos. El tipo de retorno Page<Producto> contiene la lista de productos para la página actual, así como información sobre la paginación (número total de páginas, número total de elementos, etc.)

Para el uso de estos métodos en los servicios o controladores se necesita de:
```
    int numeroDePagina = 0;
    int tamanoDePagina = 10;
    Pageable primeraPaginaCon10Productos = PageRequest.of(numeroDePagina, tamanoDePagina);
    Page<Producto> productosPagina1 = productoRepository.findAll(primeraPaginaCon10Productos);
    
    Pageable segundaPaginaCon5ProductosOrdenadosPorPrecio = PageRequest.of(1, 5, Sort.by("precio").descending());
    Page<Producto> productosPagina2Ordenados = productoRepository.findAll(segundaPaginaCon5ProductosOrdenadosPorPrecio);
    
    Page<Producto> productosElectronicosPagina1 = productoRepository.buscarPorCategoria("Electrónicos", primeraPaginaCon10Productos);
```

<a id="ejemplos-con-la-interfaz-sort"><a/>
#### Ejemplos con la interfaz Sort
La interfaz Sort representa las opciones de ordenamiento. Se puede especificar por qué atributos ordenar y en qué dirección (ascendente o descendente). Es necesario crear instancias de Sort utilizando los métodos estáticos proporcionados por la clase Sort.

> Ejemplo
```
import org.springframework.data.domain.Sort;
import org.springframework.data.jpa.repository.JpaRepository;
import com.example.entity.Producto;
import java.util.List;

public interface ProductoRepository extends JpaRepository<Producto, Long> {

    List<Producto> findAll(Sort sort);

    List<Producto> findByNombreLike(String patron, Sort sort);
}
```
Para utilizar el ordenamiento se necesita de:
```
    Sort ordenarPorNombreAscendente = Sort.by("nombre").ascending();
    List<Producto> productosOrdenadosPorNombre = productoRepository.findAll(ordenarPorNombreAscendente
```

<a id="ejemplos-con-la-interfaz-pagerequest"><a/>
#### Ejemplos con la interfaz PageRequest
PageRequest es una implementación concreta de la interfaz Pageable. Se utiliza para construir objetos Pageable que se pasan a los métodos de los repositorios para habilitar la paginación.

El método estático of() de la clase PageRequest es la forma principal de crear instancias. Tiene las siguientes sobrecargas:

1. PageRequest.of(int page, int size): Crea un PageRequest para la página especificada (page, comenzando desde 0) con el tamaño de página dado (size). El ordenamiento no se especifica en esta sobrecarga.

> Ejemplos
```
// Solicitar la primera página con 10 elementos
Pageable primeraPagina = PageRequest.of(0, 10);

// Solicitar la tercera página con 5 elementos
Pageable terceraPagina = PageRequest.of(2, 5);
```

2. PageRequest.of(int page, int size, Sort sort): Crea un PageRequest para la página y tamaño especificados, y también incluye la información de ordenamiento (Sort).

> Ejemplos
```
// Solicitar la primera página con 10 elementos, ordenados por nombre ascendente
Pageable primeraPaginaOrdenadaPorNombreAsc = PageRequest.of(0, 10, Sort.by("nombre").ascending());

// Solicitar la segunda página con 5 elementos, ordenados por precio descendente y luego por nombre ascendente
Pageable segundaPaginaOrdenadaPorPrecioDescNombreAsc = PageRequest.of(1, 5, Sort.by("precio").descending().and(Sort.by("nombre").ascending()));
```

> Uso con los métodos del repositorio
```
@Service
public class ProductoService {

    @Autowired
    private ProductoRepository productoRepository;

    public Page<Producto> obtenerProductosPaginados(int numeroPagina, int tamanoPagina) {
        Pageable pageable = PageRequest.of(numeroPagina, tamanoPagina);
        return productoRepository.findAll(pageable);
    }

    public Page<Producto> obtenerProductosPorCategoriaPaginadosOrdenados(String categoria, int numeroPagina, int tamanoPagina, String sortBy, String sortDirection) {
        Sort sort = Sort.by(sortBy);
        if (sortDirection.equalsIgnoreCase("desc")) {
            sort = sort.descending();
        }
        Pageable pageable = PageRequest.of(numeroPagina, tamanoPagina, sort);
        return productoRepository.buscarPorCategoria(categoria, pageable);
    }
}
```

<a id="auditoria"><a/>
### Auditoria
La auditoría en Spring Data JPA permite rastrear automáticamente quién creó o modificó una entidad y cuándo lo hizo. 

> Configuración de la Auditoría

1.  Habilitar la funcionalidad de auditoría en tu configuración de Spring (a menudo en la clase principal o en una clase de configuración específica) utilizando la anotación @EnableJpaAuditing
```
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
import org.springframework.boot.SpringApplication;

@SpringBootApplication
@EnableJpaAuditing
public class MiAplicacion {

    public static void main(String[] args) {
        SpringApplication.run(MiAplicacion.class, args);
    }
}
```
2. Configurar el AuditorAware (opcional): Para rastrear quién realizó la creación o modificación 
```
import org.springframework.data.domain.AuditorAware;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.stereotype.Component;
import java.util.Optional;

@Component
public class AuditorAwareImpl implements AuditorAware<String> {

    @Override
    public Optional<String> getCurrentAuditor() {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

        if (authentication == null || !authentication.isAuthenticated()) {
            return Optional.empty();
        }

        return Optional.of(authentication.getName());
    }
}
```
> [!IMPORTANT]
> Si no proporcionas un AuditorAware, los campos @CreatedBy y @LastModifiedBy no se poblarán.


<a id="la-anotacion-created-date"><a/>
#### Uso de la anotación @CreatedDate
La anotación @CreatedDate se aplica a un campo de tipo fecha (como java.util.Date, java.time.LocalDateTime, etc.). Spring Data JPA poblará automáticamente este campo con la fecha y hora de creación de la entidad cuando se persiste por primera vez

> Ejemplo
```
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import org.springframework.data.annotation.CreatedDate;
import java.time.LocalDateTime;

@Entity
public class ProductoAuditado { 
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String nombre;
    private double precio;
    
    @CreatedDate
    private LocalDateTime fechaCreacion;
    
    // ... 
}
```

<a id="la-anotacion-last-modified-date"><a/>
#### Uso de la anotación @LastModifiedDate
La anotación `@LastModifiedDate` se aplica a un campo de tipo fecha. Spring Data JPA actualizará automáticamente este campo con la fecha y hora de la última modificación de la entidad cada vez que se guarda.

> Ejemplo
```
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import java.time.LocalDateTime;

@Entity
public class ProductoAuditado {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;
    private double precio;

    @CreatedDate
    private LocalDateTime fechaCreacion;

    @LastModifiedDate
    private LocalDateTime fechaModificacion;

    // ...
}
```

<a id="la-anotacion-created-by"><a/>
#### Uso de la anotación @CreatedBy
La anotación @CreatedBy se aplica a un campo del tipo devuelto por tu AuditorAware (en nuestro ejemplo, String). Spring Data JPA poblará automáticamente este campo con el "principal" que creó la entidad cuando se persiste por primera vez. Para que esto funcione, se debe de tener configurado un AuditorAware bean.

> Ejemplo
```
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import org.springframework.data.annotation.CreatedBy;
import org.springframework.data.annotation.CreatedDate;
import java.time.LocalDateTime; 

@Entity
public class ProductoAuditado {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String nombre;
    private double precio;
    
    @CreatedDate
    private LocalDateTime fechaCreacion;
    
    @CreatedBy
    private String creadoPor;
    
    // ...
}
```


<a id="la-anotacion-last-modified-by"><a/>
#### Uso de la anotación @LastModifiedBy
La anotación `@LastModifiedBy` se aplica a un campo del tipo devuelto por tu `AuditorAware`. Spring Data JPA actualizará automáticamente este campo con el "principal" que modificó la entidad por última vez cada vez que se guarda. También requiere la configuración de un `AuditorAware` bean.

> Ejemplo

```
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import org.springframework.data.annotation.CreatedBy;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedBy;
import org.springframework.data.annotation.LastModifiedDate;
import java.time.LocalDateTime;

@Entity
public class ProductoAuditado {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;
    private double precio;

    @CreatedDate
    private LocalDateTime fechaCreacion;

    @CreatedBy
    private String creadoPor;

    @LastModifiedDate
    private LocalDateTime fechaModificacion;

    @LastModifiedBy
    private String modificadoPor;

    // ...
}
```

<a id="eventos-de-repositorio"><a/>
### Eventos de Repositorio
Spring Data JPA permite interceptar ciertos eventos que ocurren durante el ciclo de vida de las operaciones del repositorio. Es posible definir listeners para estos eventos para realizar acciones personalizadas antes o después de que se ejecuten las operaciones del repositorio (como guardar, eliminar, etc.)

Spring Data JPA publica varios eventos, tanto antes como después de las operaciones del repositorio. Algunos de los eventos más comunes son:

<a id="before-save-event"><a/>
#### BeforeSaveEvent
Este evento se publica justo antes de que una entidad sea guardada (ya sea una nueva persistencia o una actualización). El listener para este evento recibirá una instancia de BeforeSaveEvent que contiene la entidad que se va a guardar. **Se puede modificar la entidad dentro de este listener**.

> Ejemplo
```
import org.springframework.context.ApplicationListener;
import org.springframework.data.domain.BeforeSaveEvent;
import org.springframework.stereotype.Component;

@Component
public class ProductoBeforeSaveListener implements ApplicationListener<BeforeSaveEvent<ProductoAuditado>> {

    @Override
    public void onApplicationEvent(BeforeSaveEvent<ProductoAuditado> event) {
        ProductoAuditado producto = event.getEntity();
        System.out.println("Antes de guardar el producto: " + producto.getNombre());
        // Aquí puedes realizar validaciones, modificaciones, etc.
        if (producto.getNombre() != null) {
            producto.setNombre(producto.getNombre().trim());
        }
    }
}
```

<a id="after-save-event"><a/>
#### AfterSaveEvent
Este evento se publica justo después de que una entidad ha sido guardada (persistida o actualizada). El listener para este evento recibirá una instancia de AfterSaveEvent que contiene la entidad guardada.

> Ejemplo
```
import org.springframework.context.ApplicationListener;
import org.springframework.data.domain.AfterSaveEvent;
import org.springframework.stereotype.Component;

@Component
public class ProductoAfterSaveListener implements ApplicationListener<AfterSaveEvent<ProductoAuditado>> {

    @Override
    public void onApplicationEvent(AfterSaveEvent<ProductoAuditado> event) {
        ProductoAuditado producto = event.getEntity();
        System.out.println("Después de guardar el producto con ID: " + producto.getId());
        // Aquí puedes realizar tareas de notificación, logging, etc.
    }
}
```

<a id="otro-tipo-de-eventos"><a/>
#### Otro tipo de eventos
Además de los eventos de guardado, Spring Data JPA también publica eventos para otras operaciones del repositorio, como:

- BeforeDeleteEvent y AfterDeleteEvent: Publicados antes y después de eliminar una entidad.
- BeforeDeleteAllEvent y AfterDeleteAllEvent: Publicados antes y después de eliminar todas las entidades.
- BeforeConvertEvent: Publicado antes de que una entidad sea convertida para ser escrita en la base de datos.
- Eventos específicos de cada operación del repositorio (por ejemplo, eventos antes y después de una consulta personalizada)

Para escuchar estos eventos, simplemente se debe de implementar la interfaz ApplicationListener para el tipo de evento que es de interés y además tiene que ser registrado como un bean de Spring (usando @Component, @Service, etc.). El tipo genérico de ApplicationListener debe ser el tipo específico del evento que se quiera escuchar (por ejemplo, BeforeSaveEvent<NombreDeLaEntidad>).

<a id="proyecciones-en-jpa"><a/>
### Proyecciones

<a id="consultas-asincronas"><a/>
### Consultas asíncronas

<a id="la-clase-future"><a/>
#### La clase Future

<a id="la-clase-listeneable-future"><a/>
#### La clase ListenableFuture

<a id="la-clase-completable-future"><a/>
#### La clase CompletableFuture

<a id="transacciones-en-spring-data-jpa"><a/>   
### Transacciones en Spring Data JPA

<a id="la-anotacion-transactional"><a/>
#### La anotación @Transactional

<a id="manejo-de-excepciones"><a/>
### Manejo de excepciones de Spring Data JPA
