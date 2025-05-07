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
    - [Generación de claves](#generacion-de-claves)
        - [Estrategia AUTO, IDENTITY, SEQUENCE, TABLE](#estrategias-de-generacion-de-claves)
    - [Mapeo de tipos de datos](#mapeo-de-tipo-de-datos)
    - [Columnas](#columns)
        - [Propiedad de nulabilidad](#propiedad-nullable)
        - [Propiedad de unicidad](#propiedad-unique)
        - [Otras propiedades](#otras-propiedades)
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

<a id="interfaces-centrales-en-jpa"></a>
### Interfaces centrales en JPA

<a id="entity-manager-factory"></a>
#### EntityManagerFactory

<a id="entity-manager"></a>
#### EntityManager

<a id="provedores-de-jpa"></a>
### Proveedores de JPA

<a id="hibernate"></a>
#### Hibernate

<a id="open-jpa"></a>
#### OpenJPA

<a id="orm"></a>
## Mapeo Objeto-Relacional (ORM)

<a id="anotaciones-basicas"></a>
### Anotaciones básicas

<a id="la-anotacion-entity"></a>
#### La anotación @Entity

<a id="la-anotacion-table"></a>
#### La anotación @Table

<a id="id-y-generated-value"></a>
#### Las anotaciones @Id y @GeneratedValued

<a id="la-anotacion-basic"></a>
#### La anotación @Basic

<a id="la-anotacion-temporal"></a>
#### La anotación @Temporal

<a id="la-anotacion-enumerated"></a>
#### La anotación @Enumerated

<a id="la-anotacion-lob"></a>
#### La anotación @Lob

<a id="la-anotacion-transient"></a>
#### La anotación @Transient

<a id="claves-primarias"></a>
### Claves Primarias

<a id="claves-simples"></a>
#### Claves simples

<a id="claves-embebidas"></a>
#### Claves embebidas

<a id="generacion-de-claves"></a>
### Generación de claves

<a id="estrategias-de-generacion-de-claves"></a>
#### Estrategia AUTO, IDENTITY, SEQUENCE, TABLE

<a id="mapeo-de-tipo-de-datos"></a>
### Mapeo de tipos de datos

<a id="columns"></a>
### Columnas

<a id="propiedad-nullable"></a>
#### Propiedad de nulabilidad

<a id="propiedad-unique"></a>
#### Propiedad de unicidad

<a id="otras-propiedades"></a>
#### Otras propiedades

<a id="mapeo-de-tipos-embebidos"></a>
### Mapeo de Tipos Embebidos

<a id="anotacion-embeddable"></a>
#### La anotación @Embeddable

<a id="anotacion-embedded"></a>
#### La anotación @Embedded

<a id="mapeo-de-colecciones"></a>
### Mapeo de colecciones

<a id="anotacion-element-collection"></a>
#### La anotación @ElementCollection

<a id="anotacion-collection-table"></a>
#### La anotación @CollectionTable

<a id="relacion-entre-entidades"></a>
### Relaciones entre entidades

<a id="relaciones-unidireccionales-y-bidireccionales"></a>
#### Relaciones unidireccionales y bidireccionales

<a id="cardinalidad"></a>
### Cardinalidad

<a id="uno-a-uno"></a>
#### Uno-a-Uno

<a id="uno-a-muchos"></a>
#### Uno-a-Muchos

<a id="muchos-a-uno"></a>
#### Muchos-a-Uno

<a id="muchos-a-muchos"></a>
#### Muchos-a-Muchos

<a id="estrategia-de-fetching"></a>
### Estrategias de Fetching

<a id="carga-perezosa"></a>
#### Carga Perezosa (Lazy)

<a id="carga-ansiosa"></a>
#### Carga Ansiosa (Eager)

<a id="operaciones-en-casacada"></a>
### Operaciones en cascada

<a id="consultas-y-recuperacion-de-datos"></a>
## Consultas y recuperación de datos

<a id="jpql"></a>
### Java Persistence Query Language (JPQL)

<a id="sintaxis-de-jpql"></a>
#### Sintaxis Básica

<a id="sentencias-basicas"></a>
##### Sentencias básicas SELECT, FROM, WHERE, ORDER BY, GROUP BY, HAVING

<a id="consultas-con-parametros"></a>
#### Consultas con Parámetros

<a id="funciones-y-operadores"></a>
#### Funciones y Operadores

<a id="funciones-agregadas"></a>
##### Funciones agregadas

<a id="funciones-de-cadena"></a>
##### Funciones de cadena

<a id="operadores-logicos-y-de-comparacion"></a>
##### Operadores lógicos y de comparación

<a id="relaciones-en-jpql"></a>
### Relaciones en JPQL (JOIN, FETCH JOIN)

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
