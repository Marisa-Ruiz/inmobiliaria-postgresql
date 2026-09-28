# 🏢 Base de Datos de Gestión Inmobiliaria (PostgreSQL) 

Este repositorio contiene el diseño, creación y consultas SQL para un sistema de gestión de alquileres de propiedades y habitaciones desarrollado en **PostgreSQL** y ejecutado mediante **pgAdmin 4**. 

---  

## 📌 Descripción del Proyecto 
El objetivo del proyecto es modelar y gestionar las interacciones entre propietarios (*hosts*), propiedades, habitaciones individuales, clientes (*guests*), reservas, pagos, valoraciones y servicios asignados a cada inmueble. 
El proyecto abarca desde la definición de la estructura relacional (DDL) hasta la extracción de métricas de negocio complejas (DML y consultas agregadas). 

--- 

## 🛠️ Tecnologías Utilizadas 
* **Motor de Base de Datos:** PostgreSQL
* **Gestor de BD:** pgAdmin 4
* **Control de Versiones:** Git / GitHub

--- 

## 🗂️ Estructura del Repositorio 
* `ejercicio.md`: Script completo con todas las consultas SQL estructuradas y documentadas con sus respectivos resultados y justificaciones técnicas.
* `/capturas`: Capturas de pantalla de los resultados ejecutados paso a paso en pgAdmin 4.

--- 

## 📊 Modelo de Datos 

La base de datos se compone de las siguientes tablas principales interrelacionadas: 

1. **`owner`**: Información de los propietarios/anfitriones.
2. **`city`**: Ubicaciones y datos de cercanía a universidades o barrios.
3. **`property`**: Propiedades registradas (pisos, casas, áticos).
4. **`room`**: Habitaciones individuales asociadas a las propiedades.
5. **`client`**: Datos de los clientes o huéspedes.
6. **`reservation`**: Registro de reservas realizadas.
7. **`facilities`**: Equipamiento y servicios de las habitaciones/propiedades.
8. **`payment`**: Registro de depósitos e ingresos por reserva.
9. **`review`**, **`chat`**, **`images`**: Tablas complementarias de feedback, comunicación y multimedia.

--- 

## 🚀 Contenido Desarrollado 

### **Nivel I: Fundamentos y Gestión de Datos** 
* **Parte 1 (CREATE &amp; INSERT):** Definición de tablas con claves primarias, foráneas, restricciones de integridad e inserción de datos iniciales.
* **Parte 2 (READ):** Consultas elementales de lectura.
* **Parte 3 (UPDATE):** Modificación de registros y actualización de estados.
* **Parte 4 (DELETE):** Eliminación controlada de registros y manejo de restricciones de clave foránea (\`FOREIGN KEY\`).

### **Nivel II: Filtrado, Búsqueda y Ordenamiento** 
* **Parte 5 a 8:** Uso de cláusulas `WHERE`, operadores de búsqueda de patrones (`LIKE`/`ILIKE`), ordenamiento (`ORDER BY`), rangos (`BETWEEN`) y listas (`IN`).

### **Nivel III: Relaciones y Métricas Agregadas** 
* **Parte 9 y 10 (JOIN &amp; LEFT JOIN):** Consultas relacionales complejas entre múltiples tablas y lógica de negocio.
* **Parte 11 (Agregaciones):** Cálculo de conteos, valores máximos (`MAX`), mínimos (`MIN`), promedios (`AVG`) y totales (`SUM`).
* **Parte 12 y 13 (GROUP BY &amp; HAVING):** Agrupamiento de datos por entidades y filtrado de agregaciones.

--- 

## 👤 Autor 

* **Marisa Ruiz** — *Desarrollo e implementación del script SQL*
