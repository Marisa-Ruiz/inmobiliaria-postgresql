Queries SQL - plataforma de alquiler

NIVEL I

Parte 1. CREATE

0. Crea las tablas de tu modelo de datos

CREATE TABLE "owner" (
    owner_id     BIGSERIAL PRIMARY KEY,
    "name"       VARCHAR(100) NOT NULL,
    surname      VARCHAR(100)NOT NULL,
    email        VARCHAR(150) UNIQUE,
    phone    VARCHAR(20)
);

CREATE TABLE city (
    city_id     BIGSERIAL PRIMARY KEY,
    proximity_university       VARCHAR(100),
    neihborhood      VARCHAR(100)
);

CREATE TABLE property (
    property_id     BIGSERIAL PRIMARY KEY,
    owner_id        INT NOT NULL REFERENCES "owner"(owner_id),
    city_id         INT NOT NULL REFERENCES city(city_id),
    "name"            VARCHAR(100) NOT NULL,
    status          VARCHAR(20) NOT NULL,
    description     VARCHAR(1000),
    square_meters   NUMERIC(6,2),
    property_type   VARCHAR(50),
    smokers         BOOLEAN DEFAULT FALSE,
    pets            BOOLEAN DEFAULT FALSE,
    gender          VARCHAR(20),
    cadastre        VARCHAR(50) UNIQUE,
    price           NUMERIC(7,2) NOT NULL,
    publish_date    DATE DEFAULT CURRENT_DATE
);

CREATE TABLE room (
    room_id         BIGSERIAL PRIMARY KEY,
    property_id     INT NOT NULL REFERENCES property(property_id),
    "name"            VARCHAR(100) NOT NULL,
    description     VARCHAR(1000),
    status          VARCHAR(20) NOT NULL,
    price           NUMERIC(7,2) NOT NULL,
    publish_date    DATE DEFAULT CURRENT_DATE
);

CREATE TABLE client (
    client_id             BIGSERIAL PRIMARY KEY,
    "name"                  VARCHAR(100) NOT NULL,
    surname               VARCHAR(100),
    client_description    VARCHAR(1000),
    smoker                BOOLEAN DEFAULT FALSE,
    pets                  BOOLEAN DEFAULT FALSE,
    children              BOOLEAN DEFAULT FALSE,
    email                 VARCHAR(150) UNIQUE,
    phone                 VARCHAR(20),
    occupation            VARCHAR(100),
    gender                VARCHAR(20)
);
 
CREATE TABLE reservation (
    reservation_id   BIGSERIAL PRIMARY KEY,
    client_id        INT NOT NULL REFERENCES client(client_id),
    property_id      INT NOT NULL REFERENCES property(property_id),
    room_id          INT REFERENCES room(room_id),
    status           VARCHAR(20) NOT NULL,
    start_date       DATE NOT NULL,
    end_date         DATE,
    CHECK (end_date IS NULL OR end_date >= start_date)
);
 
CREATE TABLE facilities (
    facilities_id     BIGSERIAL PRIMARY KEY,
    property_id       INT REFERENCES property(property_id),
    room_id           INT REFERENCES room(room_id),
    bathroom          BOOLEAN DEFAULT FALSE,
    number_bathroom   INT,
    air_conditioning  BOOLEAN DEFAULT FALSE,
    heating           BOOLEAN DEFAULT FALSE,
    terrace           BOOLEAN DEFAULT FALSE,
    type_cousine      VARCHAR(50),
    lighting          VARCHAR(50),
    orientation       VARCHAR(50),
    pool              BOOLEAN DEFAULT FALSE,
    environment       VARCHAR(50),
    garage            BOOLEAN DEFAULT FALSE
);
 
CREATE TABLE chat (
    chat_id      BIGSERIAL PRIMARY KEY,
    owner_id     INT NOT NULL REFERENCES owner(owner_id),
    client_id    INT NOT NULL REFERENCES client(client_id),
    message      VARCHAR(500)
);
 
CREATE TABLE review (
    review_id      BIGSERIAL PRIMARY KEY,
    property_id    INT NOT NULL REFERENCES property(property_id),
    client_id      INT NOT NULL REFERENCES client(client_id),
    review_text    VARCHAR(500),
    punctuation    NUMERIC(2,1)
);
 
CREATE TABLE images (
    images_id     BIGSERIAL PRIMARY KEY,
    property_id   INT REFERENCES property(property_id),
    room_id       INT REFERENCES room(room_id),
    review_id     INT REFERENCES review(review_id),
    chat_id       INT REFERENCES chat(chat_id),
    "name"          VARCHAR(150),
    "path"          VARCHAR(255) NOT NULL,
    "type"          VARCHAR(20),
    "size"          INT
);
 
CREATE TABLE payment (
    payment_id       BIGSERIAL PRIMARY KEY,
    reservation_id   INT NOT NULL REFERENCES reservation(reservation_id),
    deposit          NUMERIC(7,2),
    amount           NUMERIC(7,2) NOT NULL
);

1. Inserta registros en las tablas creadas.

INSERT INTO "owner" ("name", surname, email, phone) VALUES
('Carlos', 'Ramírez', 'carlos.ramirez@email.com', '612345678'),
('Laura', 'Gómez', 'laura.gomez@email.com', '623456789'),
('Marta', 'Fernández', 'marta.fernandez@email.com', '634567890'),
('Javier', 'Torres', 'javier.torres@email.com', '645678901');

INSERT INTO city (proximity_university, neihborhood) VALUES
('Universidad de Granada', 'Realejo'),
('Universidad de Granada', 'Albaicín'),
('Universidad de Granada', 'Zaidín');

INSERT INTO property (owner_id, city_id, "name", status, description, square_meters, property_type, smokers, pets, gender, cadastre, price, publish_date) VALUES
(1, 1, 'Piso Centro', 'alquilada', 'Piso luminoso con habitación disponible', 80.00, 'piso', FALSE, TRUE, NULL, 'CAD001', 750.00, '2026-01-05'),
(1, 2, 'Ático Playa', 'libre', 'Ático con vistas al mar', 65.00, 'ático', FALSE, FALSE, NULL, 'CAD002', 950.00, '2026-02-10'),
(2, 1, 'Casa Estudiantes', 'alquilada', 'Casa compartida para estudiantes, varias habitaciones', 110.00, 'casa', FALSE, TRUE, NULL, 'CAD003', 600.00, '2025-11-20'),
(3, 3, 'Apartamento Universitario', 'reservada', 'Apartamento pequeño cerca de la universidad', 45.00, 'piso', TRUE, FALSE, NULL, 'CAD004', 500.00, '2026-03-01');

INSERT INTO room (property_id, "name", description, status, price, publish_date) VALUES
(3, 'Habitación Norte', 'Habitación individual con escritorio', 'alquilada', 300.00, '2025-11-20'),
(3, 'Habitación Sur', 'Habitación doble con balcón', 'libre', 280.00, '2025-11-20'),
(3, 'Habitación Este', 'Habitación individual, baño compartido', 'libre', 290.00, '2025-11-20'),
(1, 'Habitación Invitados', 'Habitación libre dentro del Piso Centro', 'libre', 320.00, '2026-01-05');

INSERT INTO client ("name", surname, client_description, smoker, pets, children, email, phone, occupation, gender) VALUES
('Paula', 'Ríos', 'Estudiante de máster, no fumadora', FALSE, FALSE, FALSE, 'paula.rios@email.com', '655111222', 'Estudiante', 'femenino'),
('Miguel', 'Sánchez', 'Profesional con mascota', FALSE, TRUE, FALSE, 'miguel.sanchez@email.com', '655222333', 'Ingeniero', 'masculino'),
('Ana', 'López', 'Estudiante de grado', FALSE, FALSE, FALSE, 'ana.lopez@email.com', '655333444', 'Estudiante', 'femenino'),
('Diego', 'Molina', 'Recién llegado a la ciudad', TRUE, FALSE, FALSE, 'diego.molina@email.com', '655444555', 'Autónomo', 'masculino');

INSERT INTO reservation (client_id, property_id, room_id, status, start_date, end_date) VALUES
(1, 3, 1, 'confirmada', '2026-01-10', '2026-06-10'),
(2, 1, NULL, 'confirmada', '2026-02-01', NULL),
(3, 3, 2, 'pendiente', '2026-03-01', '2026-09-01'),
(1, 4, NULL, 'cancelada', '2025-12-01', '2025-12-15');

INSERT INTO facilities (property_id, room_id, bathroom, number_bathroom, air_conditioning, heating, terrace, type_cousine, lighting, orientation, pool, environment, garage) VALUES
(3, NULL, TRUE, 2, TRUE, TRUE, FALSE, 'americana', 'natural', 'sur', FALSE, 'tranquilo', FALSE),
(NULL, 1, TRUE, 1, TRUE, FALSE, FALSE, NULL, 'natural', 'este', FALSE, NULL, FALSE),
(NULL, 2, FALSE, NULL, FALSE, TRUE, TRUE, NULL, 'artificial', 'norte', FALSE, NULL, FALSE),
(1, NULL, TRUE, 2, TRUE, TRUE, TRUE, 'independiente', 'natural', 'sur', TRUE, 'exclusivo', TRUE);

INSERT INTO review (property_id, client_id, review_text, punctuation) VALUES
(3, 1, 'Muy buena experiencia, casa cómoda y bien comunicada.', 4.5),
(1, 2, 'Piso excelente, tal como se describía.', 5.0),
(3, 3, 'Correcto, aunque algo ruidoso por las noches.', 3.8);

INSERT INTO payment (reservation_id, deposit, amount) VALUES
(1, 300.00, 300.00),
(2, 750.00, 750.00),
(3, 280.00, 280.00),
(4, 500.00, 0.00);

INSERT INTO chat (owner_id, client_id, message) VALUES
(1, 1, 'Hola, ¿la habitación sigue disponible para marzo?'),
(2, 3, 'Sí, puedes venir a verla este fin de semana.'),
(1, 2, 'Perfecto, confirmamos el contrato para el día 1.');

INSERT INTO images (property_id, room_id, review_id, chat_id, "name", "path", "type", "size") VALUES
(1, NULL, NULL, NULL, 'fachada_piso_centro', '/img/property1_fachada.jpg', 'jpg', 204800),
(NULL, 1, NULL, NULL, 'habitacion_norte', '/img/room1_norte.jpg', 'jpg', 153600),
(NULL, NULL, 1, NULL, 'foto_review_casa', '/img/review1_casa.jpg', 'jpg', 102400),
(NULL, NULL, NULL, 3, 'captura_contrato', '/img/chat3_contrato.png', 'png', 51200);

Parte 2. READ

2. Mostrar todos los registros de la tabla `anfitriones`.

```sql
SELECT * FROM "owner";
```
**Resultado:**

<img width="1008" height="307" alt="showOwners" src="https://github.com/user-attachments/assets/574ffac6-5881-46e5-91f0-5702c77bde38" />


3. Mostrar todos los registros de la tabla `huespedes`.

```sql
SELECT * FROM client;
```

**Resultado:**

<img width="1625" height="328" alt="showClients" src="https://github.com/user-attachments/assets/4e3ca447-96ff-4607-a47b-6ae228e15a14" />


4. Mostrar todos los registros de la tabla `habitaciones`.

```sql
SELECT * FROM room;
```

**Resultado:**

<img width="1162" height="313" alt="showRooms" src="https://github.com/user-attachments/assets/b863070d-6741-464b-8e55-881ccc6737e9" />


5. Mostrar todos los registros de la tabla `reservas`.

```sql
SELECT * FROM reservation;
```

**Resultado:**

<img width="911" height="312" alt="showReservations" src="https://github.com/user-attachments/assets/f94f917d-fe21-4fdc-9748-9e89251f1070" />


6. Mostrar todos los registros de la tabla `pagos`.

```sql
SELECT * FROM payment;
```

**Resultado:**

<img width="585" height="302" alt="showPayments" src="https://github.com/user-attachments/assets/42d242e4-b4a7-41a8-8083-4fca403797dc" />


7. Mostrar solo el nombre y el email de los anfitriones.

```sql
SELECT "name", email FROM "owner";
```

**Resultado:**

<img width="437" height="300" alt="ownerNameEmail" src="https://github.com/user-attachments/assets/5e0ab1cb-28d0-406c-b6d9-4a7c324762ad" />


8. Mostrar solo el título, tipo y ciudad de las habitaciones.

```sql
SELECT
room."name" AS titulo,
    property.property_type AS tipo,
    city.neihborhood AS ciudad
FROM room
JOIN property ON room.property_id = property.property_id
JOIN city ON property.city_id = city.city_id;
```

**Resultado:**

<img width="667" height="336" alt="roomsTitleTypeCity" src="https://github.com/user-attachments/assets/93ad50e4-3225-415f-9072-3ca35a862699" />


9. Mostrar el nombre y teléfono de los huéspedes.

```sql
SELECT "name", phone
FROM client;
```

**Resultado:**

<img width="471" height="330" alt="9clientNamePhone" src="https://github.com/user-attachments/assets/ce5bb6f0-f0ae-449b-ad4a-666b48bf6143" />


Parte 3. UPDATE

10. Actualizar el teléfono de un anfitrión.

```sql
UPDATE "owner" SET phone = '699888777' WHERE owner_id = 1;
```

**Resultado:**

<img width="937" height="112" alt="updateOwnerPhone" src="https://github.com/user-attachments/assets/7e5db991-eec8-4ca2-a036-1a3fde016c05" />


11. Cambiar el estado de una reserva de `pendiente` a `confirmada`.

```sql
UPDATE reservation 
SET status = 'confirmada' 
WHERE status = 'pendiente' 
AND reservation_id = 3;
```

**Resultado:**

<img width="892" height="241" alt="11updateReservationStatus" src="https://github.com/user-attachments/assets/faaa2dba-ad76-42ce-8846-d99ec269d573" />


12. Actualizar el precio por noche de una habitación.

```sql
UPDATE room SET price = 310.00 WHERE room_id = 1; 
```

**Resultado:**

<img width="1193" height="213" alt="12updateRoomPrice" src="https://github.com/user-attachments/assets/74a3f002-9a4a-4bb9-bcc9-dc3c67810fa6" />


13. Cambiar el costo adicional de un servicio asignado a una habitación.

```sql
UPDATE facilities
SET heating = TRUE
WHERE room_id = 1;
```

**Resultado:**

<img width="1906" height="236" alt="13updateRoomHeating" src="https://github.com/user-attachments/assets/96d354fa-0ce1-4b10-ac60-171b4c599a90" />


14. Actualizar el monto de un pago.

```sql
UPDATE payment SET amount = 350.00 WHERE payment_id = 1;
```

**Resultado:**

<img width="587" height="250" alt="14updatePaymentAmount" src="https://github.com/user-attachments/assets/a18e4aa1-3e09-4f39-bc7d-19d7ff7bb335" />

Parte 4. DELETE

15. Eliminar un servicio asignado a una habitación.

```sql
SELECT * FROM facilities;
DELETE FROM facilities WHERE room_id = 2;
```

**Resultado:**

<img width="1917" height="292" alt="15facilities" src="https://github.com/user-attachments/assets/459448e6-461c-4403-a627-e22e11f60bd1" />
<img width="1906" height="243" alt="15deleteRoomFacility" src="https://github.com/user-attachments/assets/25a8cfeb-a43f-40aa-b90c-a34cc8dc0fe5" />

16. Eliminar una reserva específica.

```sql
SELECT * FROM reservation;
DELETE FROM payment WHERE reservation_id = 4;
DELETE FROM reservation WHERE reservation_id = 4;
```

**Resultado:**

<img width="911" height="312" alt="5showReservations" src="https://github.com/user-attachments/assets/4b76cb26-1a99-4612-b335-17b42c71ab70" />
<img width="892" height="243" alt="16deleteReservation" src="https://github.com/user-attachments/assets/5fa5c36d-01bc-434c-a6cd-67386ab02647" />

17. Intentar eliminar una habitación que tenga reservas o reseñas y explicar qué ocurre y por qué.

```sql
SELECT * FROM reservation WHERE room_id = 1;
DELETE FROM room WHERE room_id = 1;
```

**Resultado:**

<img width="1016" height="220" alt="17deleteRoomError" src="https://github.com/user-attachments/assets/0bbaf240-c929-4459-a225-d8bc839d55c8" />

La base de datos no nos deja borrar la habitación y da error porque tiene una reserva vinculada. PostgreSQL impide el borrado para no dejar datos "huérfanos" o colgados (una reserva apuntando a una habitación que ya no existe). Para poder borrarla, habría que eliminar primero la reserva asociada.

18. Intentar eliminar un anfitrión que tenga habitaciones registradas y explicar qué ocurre y por qué.

```sql
DELETE FROM "owner" WHERE owner_id = 1;
```

**Resultado:**

<img width="978" height="186" alt="18deleteOwnerError" src="https://github.com/user-attachments/assets/4d91edfa-ff76-49f9-a37e-06b76362a8b3" />

La base de datos bloquea el borrado y da error porque el anfitrión tiene propiedades registradas a su nombre. PostgreSQL no permite eliminarlo para evitar que esos pisos queden "huérfanos" o colgados sin propietario en el sistema. Para poder borrar al anfitrión, habría que eliminar o reasignar primero sus propiedades.

NIVEL II

Parte 5. Filtros con WHERE

19. Mostrar las habitaciones cuyo tipo sea `Privada`.

```sql
SELECT room.*, property.property_type
FROM room
JOIN property ON room.property_id = property.property_id
WHERE property.property_type = 'casa';
```

**Resultado:**

<img width="1386" height="252" alt="19filterPrivateRooms" src="https://github.com/user-attachments/assets/173af1d2-1d58-4060-8714-6b40c71f881b" />

20. Mostrar las habitaciones cuyo tipo sea `Compartida`.



21. Mostrar las reservas cuyo estado sea `pendiente`.
22. Mostrar las reservas cuyo estado sea `confirmada`.
23. Mostrar las habitaciones cuyo precio por noche sea mayor a `120000`.
24. Mostrar las habitaciones publicadas después del `1 de enero de 2024`.
25. Mostrar los anfitriones cuyo nombre sea `Carlos`.

Parte 6. Búsquedas con LIKE
26. Mostrar los anfitriones cuyo nombre empiece por `L`.
27. Mostrar los huéspedes cuyo nombre empiece por `M`.
28. Mostrar las habitaciones cuyo título contenga `Centro`.
29. Mostrar los servicios cuyo nombre termine en `Fi`.
30. Mostrar las habitaciones cuya ciudad contenga la letra `a`.

Parte 7. Ordenamiento
30. Mostrar las habitaciones ordenadas alfabéticamente por título.
31. Mostrar las habitaciones ordenadas de mayor a menor precio por noche.
32. Mostrar las reservas ordenadas por fecha de check-in, de la más próxima a la más lejana.
33. Mostrar los anfitriones ordenados por nombre descendente.

Parte 8. Rangos y listas
34. Mostrar las habitaciones con precio por noche entre `80000` y `200000`.
35. Mostrar las habitaciones cuyo tipo esté entre `Privada` y `Compartida` usando `IN`.
36. Mostrar las reservas cuyo estado sea `pendiente` o `confirmada`.
37. Mostrar las habitaciones ubicadas en `Bogotá` o `Medellín`.

NIVEL III

Parte 9. Relaciones con JOIN
38. Mostrar el título de cada habitación junto con el nombre de su anfitrión.
39. Mostrar el título, tipo de habitación y nombre del anfitrión.
40. Mostrar las reservas junto con el nombre del huésped.
41. Mostrar las reservas junto con el título de la habitación.
42. Mostrar las reservas con id de reserva, huésped, habitación, fecha check-in, fecha check-out y estado.
43. Mostrar los pagos junto con el nombre del huésped.
44. Mostrar los servicios asignados a cada habitación.
45. Mostrar habitación, servicio, costo_adicional y disponibilidad.
46. Mostrar todas las habitaciones con su anfitrión y sus reseñas.
47. Mostrar todas las reservas con huésped, anfitrión y habitación.

Parte 10. Consultas de negocio con JOIN
48. ¿Qué habitaciones pertenecen al anfitrión `Carlos Ramírez`?
49. ¿Qué reservas tiene el huésped `Paula Ríos`?
50. ¿Qué servicios tiene asignados la habitación `Loft Central`?
51. ¿Qué huésped reservó la habitación `Suite Norte`?
52. ¿Qué habitaciones tienen al menos una reseña registrada?
53. ¿Qué habitaciones no tienen reseñas registradas? Sugerencia: usar `LEFT JOIN`.
54. ¿Qué habitaciones tienen servicios asignados?
55. ¿Qué habitaciones no tienen servicios asignados?

Parte 11. Funciones de agregación
56. ¿Cuántos anfitriones hay registrados?
57. ¿Cuántos huéspedes hay registrados?
58. ¿Cuántas habitaciones hay publicadas?
59. ¿Cuántas reservas hay registradas?
60. ¿Cuál es el precio promedio por noche de las habitaciones?
61. ¿Cuál es la habitación más costosa por noche?
62. ¿Cuál es la habitación más económica por noche?
63. ¿Cuál es la suma total de ingresos registrados en pagos?

Parte 12. GROUP BY
64. ¿Cuántas habitaciones hay por tipo?
65. ¿Cuántas habitaciones tiene cada anfitrión?
66. ¿Cuántas reservas tiene cada huésped?
67. ¿Cuántas reservas tiene cada habitación?
68. ¿Cuántos servicios tiene asignados cada habitación?
69. ¿Cuál es el precio promedio por noche por ciudad?

Parte 13. GROUP BY + HAVING
70. Mostrar los anfitriones que tengan más de una habitación.
71. Mostrar los tipos de habitación que tengan más de una publicación.
72. Mostrar los huéspedes que tengan más de una reserva.
73. Mostrar las habitaciones que tengan más de un servicio asignado.

NIVEL IV NO OBLIGATORIO

Parte 14. Subconsultas
74. Mostrar las habitaciones cuyo precio por noche sea mayor al promedio de precios.
75. Mostrar los pagos cuyo monto sea mayor al monto promedio.
76. Mostrar las habitaciones que tienen reservas registradas.
77. Mostrar las habitaciones que no tienen reservas registradas.
78. Mostrar los anfitriones que tienen al menos una habitación.
79. Mostrar los huéspedes que tienen reservas confirmadas.
80. Mostrar el nombre de la habitación o habitaciones que tengan más reservas.
81. Mostrar la habitación más costosa por noche usando una subconsulta.

Parte 15. LEFT JOIN y análisis de datos faltantes
82. Mostrar los anfitriones que no tengan habitaciones registradas.
83. Mostrar las habitaciones que no tengan reservas.
84. Mostrar las habitaciones que no tengan servicios.
85. Mostrar los huéspedes que no tengan reservas asignadas.
86. Mostrar las habitaciones que no tengan reseñas.

Parte 16. Consultas de reto
87. Mostrar un listado completo con habitación, tipo, anfitrión, huésped de la reserva y servicio asignado.
88. Mostrar cuántas habitaciones hay por tipo, pero solo los tipos con `2` o más habitaciones.
89. Mostrar el anfitrión con mayor número de habitaciones.
90. Mostrar la habitación con mayor número de reservas.
91. Mostrar los huéspedes ordenados por cantidad de reservas, de mayor a menor.
92. Mostrar las habitaciones que tienen tanto reservas como servicios.
93. Mostrar las habitaciones que tienen reservas pero no servicios.
94. Mostrar los servicios que nunca han sido asignados.
95. Mostrar el ingreso total por habitación (sumando pagos de sus reservas).
96. Mostrar la habitación con la reserva de mayor valor pagado.

Parte 17. Preguntas de análisis
97. ¿Cómo identificarías si una relación `1:1` entre `reservas` y `pagos` está bien implementada?
98. ¿Qué consulta usarías para demostrar que la relación entre `habitaciones` y `servicios` es muchos a muchos?
99. ¿Qué diferencia hay entre usar `JOIN` y `LEFT JOIN`?
100. ¿Cuándo usarías una subconsulta en lugar de un `JOIN`?
