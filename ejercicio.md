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

__________________________________________________________________________________

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
__________________________________________________________________________________

Parte 2. READ

2. Mostrar todos los registros de la tabla `anfitriones`.

```sql
SELECT * FROM "owner";
```
**Resultado:**

<img width="1008" height="307" alt="showOwners" src="https://github.com/user-attachments/assets/574ffac6-5881-46e5-91f0-5702c77bde38" />

__________________________________________________________________________________

3. Mostrar todos los registros de la tabla `huespedes`.

```sql
SELECT * FROM client;
```

**Resultado:**

<img width="1625" height="328" alt="showClients" src="https://github.com/user-attachments/assets/4e3ca447-96ff-4607-a47b-6ae228e15a14" />

__________________________________________________________________________________

4. Mostrar todos los registros de la tabla `habitaciones`.

```sql
SELECT * FROM room;
```

**Resultado:**

<img width="1162" height="313" alt="showRooms" src="https://github.com/user-attachments/assets/b863070d-6741-464b-8e55-881ccc6737e9" />

__________________________________________________________________________________

5. Mostrar todos los registros de la tabla `reservas`.

```sql
SELECT * FROM reservation;
```

**Resultado:**

<img width="911" height="312" alt="showReservations" src="https://github.com/user-attachments/assets/f94f917d-fe21-4fdc-9748-9e89251f1070" />

__________________________________________________________________________________

6. Mostrar todos los registros de la tabla `pagos`.

```sql
SELECT * FROM payment;
```

**Resultado:**

<img width="585" height="302" alt="showPayments" src="https://github.com/user-attachments/assets/42d242e4-b4a7-41a8-8083-4fca403797dc" />

__________________________________________________________________________________

7. Mostrar solo el nombre y el email de los anfitriones.

```sql
SELECT "name", email FROM "owner";
```

**Resultado:**

<img width="437" height="300" alt="ownerNameEmail" src="https://github.com/user-attachments/assets/5e0ab1cb-28d0-406c-b6d9-4a7c324762ad" />

__________________________________________________________________________________

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

__________________________________________________________________________________

9. Mostrar el nombre y teléfono de los huéspedes.

```sql
SELECT "name", phone
FROM client;
```

**Resultado:**

<img width="471" height="330" alt="9clientNamePhone" src="https://github.com/user-attachments/assets/ce5bb6f0-f0ae-449b-ad4a-666b48bf6143" />

__________________________________________________________________________________

Parte 3. UPDATE

10. Actualizar el teléfono de un anfitrión.

```sql
UPDATE "owner" SET phone = '699888777' WHERE owner_id = 1;
```

**Resultado:**

<img width="937" height="112" alt="updateOwnerPhone" src="https://github.com/user-attachments/assets/7e5db991-eec8-4ca2-a036-1a3fde016c05" />

__________________________________________________________________________________

11. Cambiar el estado de una reserva de `pendiente` a `confirmada`.

```sql
UPDATE reservation 
SET status = 'confirmada' 
WHERE status = 'pendiente' 
AND reservation_id = 3;
```

**Resultado:**

<img width="892" height="241" alt="11updateReservationStatus" src="https://github.com/user-attachments/assets/faaa2dba-ad76-42ce-8846-d99ec269d573" />

__________________________________________________________________________________

12. Actualizar el precio por noche de una habitación.

```sql
UPDATE room SET price = 310.00 WHERE room_id = 1; 
```

**Resultado:**

<img width="1193" height="213" alt="12updateRoomPrice" src="https://github.com/user-attachments/assets/74a3f002-9a4a-4bb9-bcc9-dc3c67810fa6" />

__________________________________________________________________________________

13. Cambiar el costo adicional de un servicio asignado a una habitación.

```sql
UPDATE facilities
SET heating = TRUE
WHERE room_id = 1;
```

**Resultado:**

<img width="1906" height="236" alt="13updateRoomHeating" src="https://github.com/user-attachments/assets/96d354fa-0ce1-4b10-ac60-171b4c599a90" />

__________________________________________________________________________________

14. Actualizar el monto de un pago.

```sql
UPDATE payment SET amount = 350.00 WHERE payment_id = 1;
```

**Resultado:**

<img width="587" height="250" alt="14updatePaymentAmount" src="https://github.com/user-attachments/assets/a18e4aa1-3e09-4f39-bc7d-19d7ff7bb335" />

__________________________________________________________________________________

Parte 4. DELETE

15. Eliminar un servicio asignado a una habitación.

```sql
SELECT * FROM facilities;
DELETE FROM facilities WHERE room_id = 2;
```

**Resultado:**

<img width="1917" height="292" alt="15facilities" src="https://github.com/user-attachments/assets/459448e6-461c-4403-a627-e22e11f60bd1" />
<img width="1906" height="243" alt="15deleteRoomFacility" src="https://github.com/user-attachments/assets/25a8cfeb-a43f-40aa-b90c-a34cc8dc0fe5" />

__________________________________________________________________________________

16. Eliminar una reserva específica.

```sql
SELECT * FROM reservation;
DELETE FROM payment WHERE reservation_id = 4;
DELETE FROM reservation WHERE reservation_id = 4;
```

**Resultado:**

<img width="911" height="312" alt="5showReservations" src="https://github.com/user-attachments/assets/4b76cb26-1a99-4612-b335-17b42c71ab70" />
<img width="892" height="243" alt="16deleteReservation" src="https://github.com/user-attachments/assets/5fa5c36d-01bc-434c-a6cd-67386ab02647" />

__________________________________________________________________________________

17. Intentar eliminar una habitación que tenga reservas o reseñas y explicar qué ocurre y por qué.

```sql
SELECT * FROM reservation WHERE room_id = 1;
DELETE FROM room WHERE room_id = 1;
```

**Resultado:**

<img width="1016" height="220" alt="17deleteRoomError" src="https://github.com/user-attachments/assets/0bbaf240-c929-4459-a225-d8bc839d55c8" />

La base de datos no nos deja borrar la habitación y da error porque tiene una reserva vinculada. PostgreSQL impide el borrado para no dejar datos "huérfanos" o colgados (una reserva apuntando a una habitación que ya no existe). Para poder borrarla, habría que eliminar primero la reserva asociada.

__________________________________________________________________________________

18. Intentar eliminar un anfitrión que tenga habitaciones registradas y explicar qué ocurre y por qué.

```sql
DELETE FROM "owner" WHERE owner_id = 1;
```

**Resultado:**

<img width="978" height="186" alt="18deleteOwnerError" src="https://github.com/user-attachments/assets/4d91edfa-ff76-49f9-a37e-06b76362a8b3" />

La base de datos bloquea el borrado y da error porque el anfitrión tiene propiedades registradas a su nombre. PostgreSQL no permite eliminarlo para evitar que esos pisos queden "huérfanos" o colgados sin propietario en el sistema. Para poder borrar al anfitrión, habría que eliminar o reasignar primero sus propiedades.

__________________________________________________________________________________

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

__________________________________________________________________________________

20. Mostrar las habitaciones cuyo tipo sea `Compartida`.

```sql
SELECT room.*, property.property_type
FROM room
JOIN property ON room.property_id = property.property_id
WHERE property.property_type = 'piso';
```

**Resultado:**

<img width="1381" height="202" alt="20filterSharedRooms" src="https://github.com/user-attachments/assets/cf080e65-4c75-4771-9b84-f1de41a9a09a" />

__________________________________________________________________________________

21. Mostrar las reservas cuyo estado sea `pendiente`.

```sql
SELECT * FROM reservation WHERE status = 'pendiente';
```

**Resultado:**

<img width="885" height="195" alt="21filterPendingReservations" src="https://github.com/user-attachments/assets/5d4d8107-fe8e-4d99-9f8d-ff6d17c816c3" />

__________________________________________________________________________________

22. Mostrar las reservas cuyo estado sea `confirmada`.

```sql
SELECT * FROM reservation WHERE status = 'confirmada';
```

**Resultado:**

<img width="903" height="267" alt="22filterConfirmedReservations" src="https://github.com/user-attachments/assets/0caf9dd2-9581-4258-ba47-fa4d9d0b5815" />

__________________________________________________________________________________

23. Mostrar las habitaciones cuyo precio por noche sea mayor a `120000`.

```sql
SELECT * FROM room WHERE price > 290.00;
```

**Resultado:**

<img width="1217" height="232" alt="23filterHighPriceRooms" src="https://github.com/user-attachments/assets/4780c866-8d25-4446-b40e-a5207a388b3b" />

__________________________________________________________________________________

24. Mostrar las habitaciones publicadas después del `1 de enero de 2024`.

```sql
SELECT * FROM room WHERE publish_date > '2024-01-01';
```

**Resultado:**

<img width="1202" height="277" alt="24filterRoomsPublishedAfterDate" src="https://github.com/user-attachments/assets/608b5ee6-0198-4816-a173-e9bac38869fa" />

__________________________________________________________________________________

25. Mostrar los anfitriones cuyo nombre sea `Carlos`.

```sql
SELECT * FROM "owner" WHERE "name" = 'Carlos';
```

**Resultado:**

<img width="956" height="187" alt="25filterOwnerCarlos" src="https://github.com/user-attachments/assets/1920ef8b-5ba1-41f9-965c-e98fc1360aa3" />

__________________________________________________________________________________

Parte 6. Búsquedas con LIKE

26. Mostrar los anfitriones cuyo nombre empiece por `L`.

```sql
SELECT * FROM "owner" WHERE "name" LIKE 'L%';
```

**Resultado:**

<img width="946" height="243" alt="26ownersStartingWithL" src="https://github.com/user-attachments/assets/d18178dc-04ef-4e5a-bfcc-90856bb3d7a0" />

__________________________________________________________________________________

27. Mostrar los huéspedes cuyo nombre empiece por `M`.

```sql
SELECT * FROM client WHERE "name" LIKE 'M%';
```

**Resultado:**

<img width="1822" height="192" alt="27clientsStartingWithM" src="https://github.com/user-attachments/assets/734587cc-0957-40ca-b6ce-fe03c76d193d" />

__________________________________________________________________________________

28. Mostrar las habitaciones cuyo título contenga `Centro`.

```sql
<SELECT room.*, property."name" AS nombre_propiedad
FROM room
JOIN property ON room.property_id = property.property_id
WHERE property."name" LIKE '%Centro%';
```

**Resultado:**

<img width="1376" height="207" alt="28roomsContainingCentro" src="https://github.com/user-attachments/assets/5ee21a6f-53be-4c48-954e-3c1081dc4bea" />

__________________________________________________________________________________

29. Mostrar los servicios cuyo nombre termine en `Fi`.

```sql
SELECT * FROM facilities WHERE type_cousine LIKE '%Fi';
```

**Resultado:**

<img width="1907" height="198" alt="29facilitiesEndingWithFi" src="https://github.com/user-attachments/assets/b75e6aaa-4fb3-4cf6-a869-bcec21219042" />

__________________________________________________________________________________

30. Mostrar las habitaciones cuya ciudad contenga la letra `a`.

```sql
SELECT room.*, city.neihborhood AS ciudad
FROM room
JOIN property ON room.property_id = property.property_id
JOIN city ON property.city_id = city.city_id
WHERE city.neihborhood ILIKE '%a%';
```

**Resultado:**

<img width="1395" height="301" alt="30roomsCityContainingA" src="https://github.com/user-attachments/assets/dcd5da19-209a-4822-8f03-4e92c3814650" />

__________________________________________________________________________________

Parte 7. Ordenamiento

30. Mostrar las habitaciones ordenadas alfabéticamente por título.

```sql
SELECT * 
FROM room 
ORDER BY "name" ASC;
```

**Resultado:**

<img width="1211" height="281" alt="30roomsOrderByNameAsc" src="https://github.com/user-attachments/assets/f5482386-d0aa-4e23-bd73-878c4c0bd3c6" />

__________________________________________________________________________________

31. Mostrar las habitaciones ordenadas de mayor a menor precio por noche.

```sql
SELECT * 
FROM room 
ORDER BY price DESC;
```

**Resultado:**

<img width="1217" height="292" alt="31roomsOrderByPriceDesc" src="https://github.com/user-attachments/assets/971e6d22-6c9e-42c4-a757-ba172f2aba6d" />

__________________________________________________________________________________

32. Mostrar las reservas ordenadas por fecha de check-in, de la más próxima a la más lejana.

```sql
SELECT * 
FROM reservation 
ORDER BY start_date ASC;
```

**Resultado:**

<img width="896" height="271" alt="32reservationsOrderByStartDateAsc" src="https://github.com/user-attachments/assets/54684a8f-e801-468d-a7a0-660f94c6322b" />

__________________________________________________________________________________

33. Mostrar los anfitriones ordenados por nombre descendente.

```sql
SELECT * 
FROM "owner" 
ORDER BY "name" DESC;
```

**Resultado:**

<img width="971" height="295" alt="33ownersOrderByNameDesc" src="https://github.com/user-attachments/assets/4eb1f1a5-66e6-41be-ac64-b2b352fc2785" />

__________________________________________________________________________________

Parte 8. Rangos y listas

34. Mostrar las habitaciones con precio por noche entre `80000` y `200000`.

```sql
SELECT * 
FROM room 
WHERE price BETWEEN 280.00 AND 310.00;
```

**Resultado:**

<img width="1203" height="262" alt="34roomsPriceBetweenRange" src="https://github.com/user-attachments/assets/8f0815f9-b53b-4ae2-9164-5becd29ac324" />

__________________________________________________________________________________

35. Mostrar las habitaciones cuyo tipo esté entre `Privada` y `Compartida` usando `IN`.

```sql
SELECT room.*, property.property_type
FROM room 
JOIN property ON room.property_id = property.property_id 
WHERE property.property_type IN ('piso', 'casa');
```

**Resultado:**

<img width="1378" height="285" alt="35roomsTypeInList" src="https://github.com/user-attachments/assets/7cf09d30-63fd-432e-a374-3b511f3878a8" />

__________________________________________________________________________________

36. Mostrar las reservas cuyo estado sea `pendiente` o `confirmada`.

```sql
SELECT * 
FROM reservation 
WHERE status IN ('pendiente', 'confirmada');
```

**Resultado:**

<img width="896" height="243" alt="36reservationsStatusPendingOrConfirmed" src="https://github.com/user-attachments/assets/b9d045ee-ef0c-4307-8320-10ec95fc26a8" />

__________________________________________________________________________________

37. Mostrar las habitaciones ubicadas en `Bogotá` o `Medellín`.

```sql
SELECT room.*, city.neihborhood AS ciudad 
FROM room 
JOIN property ON room.property_id = property.property_id 
JOIN city ON property.city_id = city.city_id 
WHERE city.neihborhood IN ('Realejo', 'Albaicín');
```

**Resultado:**

<img width="1377" height="272" alt="37roomsInCitiesList" src="https://github.com/user-attachments/assets/21b51d8f-4300-4ab9-bfa9-a56c0d0b5486" />

__________________________________________________________________________________

NIVEL III

Parte 9. Relaciones con JOIN

38. Mostrar el título de cada habitación junto con el nombre de su anfitrión.

```sql
SELECT 
room."name" AS habitacion, 
"owner"."name" AS nombre_anfitrion, 
"owner".surname AS apellido_anfitrion 
FROM room 
JOIN property ON room.property_id = property.property_id 
JOIN "owner" ON property.owner_id = "owner".owner_id;
```

**Resultado:**

<img width="657" height="282" alt="38roomTitleAndOwnerName" src="https://github.com/user-attachments/assets/a3a6c284-6af1-435f-9ea3-a0de5430e27e" />

__________________________________________________________________________________

39. Mostrar el título, tipo de habitación y nombre del anfitrión.

```sql
SELECT 
room."name" AS titulo_habitacion, 
property.property_type AS tipo_propiedad, 
"owner"."name" AS nombre_anfitrion, 
"owner".surname AS apellido_anfitrion 
FROM room 
JOIN property ON room.property_id = property.property_id 
JOIN "owner" ON property.owner_id = "owner".owner_id;
```

**Resultado:**

<img width="837" height="272" alt="39roomTitleTypeOwner" src="https://github.com/user-attachments/assets/96407038-8005-4b2c-9541-43688665353f" />

__________________________________________________________________________________

40. Mostrar las reservas junto con el nombre del huésped.

```sql
SELECT 
reservation.reservation_id, 
client."name" AS nombre_huesped, 
client.surname AS apellido_huesped, 
reservation.status AS estado_reserva, 
reservation.start_date, 
reservation.end_date 
FROM reservation 
JOIN client ON reservation.client_id = client.client_id;
```

**Resultado:**

<img width="977" height="246" alt="40reservationsAndClientName" src="https://github.com/user-attachments/assets/f541856b-4201-48d2-92dc-e5fa0df86dc4" />

__________________________________________________________________________________

41. Mostrar las reservas junto con el título de la habitación.

```sql
SELECT 
reservation.reservation_id, 
room."name" AS titulo_habitacion, 
reservation.status AS estado_reserva, 
reservation.start_date, 
reservation.end_date 
FROM reservation 
JOIN room ON reservation.room_id = room.room_id;
```

**Resultado:**

<img width="787" height="217" alt="41reservationsAndRoomTitle" src="https://github.com/user-attachments/assets/1f476503-b816-4f20-b6d6-ec2a5224467a" />

__________________________________________________________________________________

42. Mostrar las reservas con id de reserva, huésped, habitación, fecha check-in, fecha check-out y estado.

```sql
SELECT 
reservation.reservation_id, 
CONCAT(client."name", ' ', 
client.surname) AS huesped, 
room."name" AS habitacion, 
reservation.start_date AS fecha_check_in, 
reservation.end_date AS fecha_check_out, 
reservation.status AS estado 
FROM reservation 
JOIN client ON reservation.client_id = client.client_id 
LEFT JOIN room ON reservation.room_id = room.room_id;
```

**Resultado:**

<img width="787" height="216" alt="42reservationsFullDetails" src="https://github.com/user-attachments/assets/118d61a7-c00c-4b81-99da-8d226eb8d54a" />

__________________________________________________________________________________

43. Mostrar los pagos junto con el nombre del huésped.

```sql
SELECT 
payment.payment_id, 
CONCAT(client."name", ' ', 
client.surname) AS huesped, 
payment.deposit AS fianza, 
payment.amount AS monto_pagado 
FROM payment 
JOIN reservation ON payment.reservation_id = reservation.reservation_id 
JOIN client ON reservation.client_id = client.client_id;
```

**Resultado:**

<img width="562" height="248" alt="43paymentsAndClientName" src="https://github.com/user-attachments/assets/63d12734-254f-4aca-8539-1dd6c713c608" />

__________________________________________________________________________________

44. Mostrar los servicios asignados a cada habitación.

```sql
SELECT
room.room_id,
room."name" AS habitacion,
facilities.bathroom AS baño_privado,
facilities.air_conditioning AS aire_acondicionado,
facilities.heating AS calefaccion,
facilities.terrace AS terraza,
facilities.lighting AS iluminacion,
facilities.orientation AS orientacion
FROM room
JOIN facilities ON room.room_id = facilities.room_id;
```

**Resultado:**

<img width="1232" height="180" alt="44roomsAndFacilities" src="https://github.com/user-attachments/assets/417919da-f88e-4512-a296-976d31cec7f5" />

__________________________________________________________________________________

45. Mostrar habitación, servicio, costo_adicional y disponibilidad.

```sql
SELECT
room."name" AS habitacion,
facilities.air_conditioning AS aire_acondicionado,
facilities.heating AS calefaccion,
facilities.terrace AS terraza,
room.price AS costo_precio,
room.status AS disponibilidad
FROM room
LEFT JOIN facilities ON room.room_id = facilities.room_id;
```

**Resultado:**

<img width="977" height="287" alt="45roomsFacilitiesCostAvailability" src="https://github.com/user-attachments/assets/6d906455-6b3d-45c7-af77-bc620e08119f" />

__________________________________________________________________________________

46. Mostrar todas las habitaciones con su anfitrión y sus reseñas.

```sql
SELECT
room."name" AS habitacion,
CONCAT("owner"."name", ' ',"owner".surname) AS anfitrion,
review.review_text AS resena,
review.punctuation AS puntuacion
FROM room JOIN property ON room.property_id = property.property_id
JOIN "owner" ON property.owner_id = "owner".owner_id
LEFT JOIN review ON property.property_id = review.property_id;
```

**Resultado:**

<img width="966" height="281" alt="46roomsOwnerAndReviews" src="https://github.com/user-attachments/assets/014886b6-9903-466d-bf98-87870c7ea976" />

__________________________________________________________________________________

47. Mostrar todas las reservas con huésped, anfitrión y habitación.

```sql
SELECT 
reservation.reservation_id, 
CONCAT(client."name", ' ', 
client.surname) AS huesped, 
room."name" AS habitacion, 
CONCAT("owner"."name", ' ', "owner".surname) AS anfitrion, 
reservation.start_date AS fecha_check_in, 
reservation.status AS estado 
FROM reservation 
JOIN client ON reservation.client_id = client.client_id 
LEFT JOIN room ON reservation.room_id = room.room_id 
LEFT JOIN property ON room.property_id = property.property_id 
LEFT JOIN "owner" ON property.owner_id = "owner".owner_id;
```

**Resultado:**

<img width="977" height="277" alt="47reservationsClientOwnerRoom" src="https://github.com/user-attachments/assets/21a1ad06-a02c-49eb-8a81-7c624675e729" />

__________________________________________________________________________________

Parte 10. Consultas de negocio con JOIN

48. ¿Qué habitaciones pertenecen al anfitrión `Carlos Ramírez`?

```sql
SELECT 
room.room_id, 
room."name" AS habitacion, 
property."name" AS inmueble, 
CONCAT("owner"."name", ' ', "owner".surname) AS anfitrion 
FROM room 
JOIN property ON room.property_id = property.property_id 
JOIN "owner" ON property.owner_id = "owner".owner_id 
WHERE "owner"."name" = 'Carlos' AND "owner".surname = 'Ramírez';
```

**Resultado:**

<img width="678" height="212" alt="48roomsByOwnerCarlosRamirez" src="https://github.com/user-attachments/assets/139832e6-06e2-4430-9386-d93469a4cd5d" />

__________________________________________________________________________________

49. ¿Qué reservas tiene el huésped `Paula Ríos`?

```sql
SELECT 
reservation.reservation_id, 
CONCAT(client."name", ' ', client.surname) AS huesped, 
room."name" AS habitacion, 
reservation.start_date AS fecha_check_in, 
reservation.end_date AS fecha_check_out,
reservation.status AS estado 
FROM reservation 
JOIN client ON reservation.client_id = client.client_id 
LEFT JOIN room ON reservation.room_id = room.room_id 
WHERE client."name" = 'Paula' AND client.surname = 'Ríos';
```

**Resultado:**

<img width="965" height="225" alt="49reservationsByClientPaulaRios" src="https://github.com/user-attachments/assets/32d295fa-1ba6-47de-962e-cead424e859c" />

__________________________________________________________________________________

50. ¿Qué servicios tiene asignados la habitación `Loft Central`?

```sql
SELECT 
room."name" AS habitacion, 
facilities.bathroom AS baño_privado, 
facilities.number_bathroom AS num_baños, 
facilities.air_conditioning AS aire_acondicionado, 
facilities.heating AS calefaccion, 
facilities.lighting AS iluminacion, 
facilities.orientation AS orientacion 
FROM room 
JOIN facilities ON room.room_id = facilities.room_id 
WHERE room."name" = 'Habitación Norte';
```

**Resultado:**

<img width="1171" height="191" alt="50roomFacilitiesAssigned" src="https://github.com/user-attachments/assets/e2abfd71-57bb-4a73-b6d8-c55f275d1ba2" />

__________________________________________________________________________________

51. ¿Qué huésped reservó la habitación `Suite Norte`?

```sql
SELECT 
reservation.reservation_id, 
CONCAT(client."name", ' ', client.surname) AS huesped, 
room."name" AS habitacion, 
reservation.start_date AS fecha_check_in, 
reservation.status AS estado 
FROM reservation 
JOIN client ON reservation.client_id = client.client_id 
JOIN room ON reservation.room_id = room.room_id 
WHERE room."name" = 'Habitación Norte';
```

**Resultado:**

<img width="817" height="178" alt="51clientReservedSuiteNorte" src="https://github.com/user-attachments/assets/fe70ec4c-989b-4113-af81-ee101c881b14" />

__________________________________________________________________________________

52. ¿Qué habitaciones tienen al menos una reseña registrada?

```sql
SELECT DISTINCT 
room.room_id, 
room."name" AS habitacion, 
property."name" AS inmueble, 
review.punctuation AS puntuacion, 
review.review_text AS resena 
FROM room 
JOIN property ON room.property_id = property.property_id 
JOIN review ON property.property_id = review.property_id;
```

**Resultado:**

<img width="1087" height="372" alt="52roomsWithReviews" src="https://github.com/user-attachments/assets/d9b0bfc0-3b00-4dd4-929d-ce643fd3b9c2" />

__________________________________________________________________________________

53. ¿Qué habitaciones no tienen reseñas registradas? Sugerencia: usar `LEFT JOIN`.

```sql
SELECT 
room.room_id, 
room."name" AS habitacion, 
property."name" AS inmueble, 
review.review_id AS id_resena, 
review.review_text AS resena 
FROM room 
JOIN property ON room.property_id = property.property_id 
LEFT JOIN review ON property.property_id = review.property_id 
WHERE review.review_id IS NULL;
```

**Resultado:**

<img width="847" height="145" alt="53roomsWithoutReviews" src="https://github.com/user-attachments/assets/69610c00-58d8-4a39-99c4-05ab5d0cd540" />

__________________________________________________________________________________

54. ¿Qué habitaciones tienen servicios asignados?

```sql
SELECT DISTINCT 
room.room_id, 
room."name" AS habitacion, 
property."name" AS inmueble 
FROM room 
JOIN property ON room.property_id = property.property_id 
JOIN facilities ON room.room_id = facilities.room_id;
```

**Resultado:**

<img width="556" height="187" alt="54roomsWithFacilitiesAssigned" src="https://github.com/user-attachments/assets/c61b385e-d173-4a8e-b639-2a20595f1fc5" />

__________________________________________________________________________________

55. ¿Qué habitaciones no tienen servicios asignados?

```sql
SELECT 
room.room_id, 
room."name" AS habitacion, 
property."name" AS inmueble 
FROM room 
JOIN property ON room.property_id = property.property_id 
LEFT JOIN facilities ON room.room_id = facilities.room_id 
WHERE facilities.room_id IS NULL;
```

**Resultado:**

<img width="557" height="252" alt="55roomsWithoutFacilitiesAssigned" src="https://github.com/user-attachments/assets/ae871a48-f030-4caf-812a-6c458635bd7c" />
__________________________________________________________________________________

Parte 11. Funciones de agregación

56. ¿Cuántos anfitriones hay registrados?

```sql
SELECT COUNT(*) AS total_anfitriones FROM "owner";
```

**Resultado:**

<img width="241" height="195" alt="56countOwners" src="https://github.com/user-attachments/assets/a61ee81a-43f6-47a6-b485-957c4d4673ee" />

__________________________________________________________________________________

57. ¿Cuántos huéspedes hay registrados?

```sql
SELECT COUNT(*) AS total_huespedes FROM client;
```

**Resultado:**

<img width="242" height="187" alt="57countClients" src="https://github.com/user-attachments/assets/d2673987-0491-41c8-a69e-8e70f2751be8" />

__________________________________________________________________________________

58. ¿Cuántas habitaciones hay publicadas?

```sql
SELECT COUNT(*) AS total_habitaciones FROM room;
```

**Resultado:**

<img width="253" height="183" alt="58countRooms" src="https://github.com/user-attachments/assets/9d01e2ab-b706-4923-9203-339f4d5cfb23" />

__________________________________________________________________________________

59. ¿Cuántas reservas hay registradas?

```sql
SELECT COUNT(*) AS total_reservas FROM reservation;
```

**Resultado:**

<img width="232" height="196" alt="59countReservations" src="https://github.com/user-attachments/assets/846a1cc6-02fe-4f31-8760-be64b0fce261" />

__________________________________________________________________________________

60. ¿Cuál es el precio promedio por noche de las habitaciones?

```sql
SELECT ROUND(AVG(price), 2) AS precio_medio FROM room;
```

**Resultado:**

<img width="212" height="185" alt="60avgRoomPrice" src="https://github.com/user-attachments/assets/d5d9f0d1-ed20-4672-867b-cb22ce58469c" />

__________________________________________________________________________________

61. ¿Cuál es la habitación más costosa por noche?

```sql
SELECT MAX(price) AS habitacion_mas_costosa FROM room;
```

**Resultado:**

<img width="292" height="192" alt="61maxRoomPrice" src="https://github.com/user-attachments/assets/ef12fedb-1e5e-4b5d-a061-a006485c79c5" />

__________________________________________________________________________________

62. ¿Cuál es la habitación más económica por noche?

```sql
SELECT MIN(price) AS habitacion_mas_economica FROM room;
```

**Resultado:**

<img width="307" height="195" alt="62minRoomPrice" src="https://github.com/user-attachments/assets/a279e336-b123-4872-8b35-d1900ba5a3f8" />

__________________________________________________________________________________

63. ¿Cuál es la suma total de ingresos registrados en pagos?

```sql
SELECT SUM(amount) AS total_ingresos FROM payment;
```

**Resultado:**

<img width="217" height="183" alt="63sumTotalPayments" src="https://github.com/user-attachments/assets/9171bd9e-bc54-4706-95ac-c4e38461567e" />

__________________________________________________________________________________

Parte 12. GROUP BY

64. ¿Cuántas habitaciones hay por tipo?

```sql
SELECT 
property.property_type AS tipo_inmueble, 
COUNT(room.room_id) AS total_habitaciones 
FROM room 
JOIN property ON room.property_id = property.property_id 
GROUP BY property.property_type;
```

**Resultado:**

<img width="437" height="233" alt="64roomsByPropertyType" src="https://github.com/user-attachments/assets/dd9ba1d3-4299-4a2b-bdde-abffbf24072f" />

__________________________________________________________________________________

65. ¿Cuántas habitaciones tiene cada anfitrión?

```sql
SELECT 
CONCAT("owner"."name", ' ', 
"owner".surname) AS anfitrion, 
COUNT(room.room_id) AS total_habitaciones 
FROM "owner" 
LEFT JOIN property ON "owner".owner_id = property.owner_id 
LEFT JOIN room ON property.property_id = room.property_id 
GROUP BY "owner".owner_id, "owner"."name", "owner".surname;
```

**Resultado:**

<img width="372" height="277" alt="65roomsPerOwner" src="https://github.com/user-attachments/assets/eb7c1b72-169b-45db-9f20-7f1ab70c754e" />

__________________________________________________________________________________

66. ¿Cuántas reservas tiene cada huésped?

```sql
SELECT 
client.client_id, 
CONCAT(client."name", ' ', client.surname) AS huesped, 
COUNT(reservation.reservation_id) AS total_reservas 
FROM client 
LEFT JOIN reservation ON client.client_id = reservation.client_id 
GROUP BY client.client_id, client."name", client.surname;
```

**Resultado:**

<img width="447" height="282" alt="66reservationsPerClient" src="https://github.com/user-attachments/assets/bbe96957-e316-454d-ac68-abc5aa06352d" />

__________________________________________________________________________________

67. ¿Cuántas reservas tiene cada habitación?

```sql
SELECT 
room.room_id, 
room."name" AS habitacion, 
COUNT(reservation.reservation_id) AS total_reservas 
FROM room 
LEFT JOIN reservation ON room.room_id = reservation.room_id 
GROUP BY room.room_id, room."name";
```

**Resultado:**

<img width="527" height="285" alt="67reservationsPerRoom" src="https://github.com/user-attachments/assets/f6397ba4-b9ab-4b63-8ad1-78d110631047" />

__________________________________________________________________________________

68. ¿Cuántos servicios tiene asignados cada habitación?

```sql
SELECT 
room.room_id, 
room."name" AS habitacion, 
COUNT(facilities.room_id) AS total_servicios 
FROM room 
LEFT JOIN facilities ON room.room_id = facilities.room_id 
GROUP BY room.room_id, room."name";
```

**Resultado:**

<img width="517" height="287" alt="68facilitiesPerRoom" src="https://github.com/user-attachments/assets/23fd7166-47f1-46bb-8229-7abe2db8501b" />

__________________________________________________________________________________

69. ¿Cuál es el precio promedio por noche por ciudad?

```sql
SELECT 
city.neihborhood AS ciudad, 
ROUND(AVG(room.price), 2) AS precio_promedio 
FROM room 
JOIN property ON room.property_id = property.property_id 
JOIN city ON property.city_id = city.city_id 
GROUP BY city.neihborhood;
```

**Resultado:**

<img width="418" height="190" alt="69avgRoomPriceByCity" src="https://github.com/user-attachments/assets/bb4dea52-f55a-4c42-8216-0e67a1cec0fb" />

__________________________________________________________________________________

Parte 13. GROUP BY + HAVING

70. Mostrar los anfitriones que tengan más de una habitación.

```sql
SELECT 
CONCAT("owner"."name", ' ', 
"owner".surname) AS anfitrion, 
COUNT(room.room_id) AS total_habitaciones 
FROM "owner" 
JOIN property ON "owner".owner_id = property.owner_id
JOIN room ON property.property_id = room.property_id 
GROUP BY "owner".owner_id, "owner"."name", "owner".surname 
HAVING COUNT(room.room_id) > 1;
```

**Resultado:**

<img width="348" height="180" alt="70ownersWithMoreThanOneRoom" src="https://github.com/user-attachments/assets/5d2f9f5f-e97f-4cc3-83a9-128131fdf004" />

__________________________________________________________________________________

71. Mostrar los tipos de habitación que tengan más de una publicación.

```sql
SELECT 
property_type AS tipo_inmueble, 
COUNT(*) AS total_publicaciones 
FROM property 
GROUP BY property_type 
HAVING COUNT(*) > 1;
```

**Resultado:**

<img width="432" height="192" alt="71propertyTypesWithMoreThanOne" src="https://github.com/user-attachments/assets/c631120f-1067-4a38-b69a-1a9bdce5addb" />

__________________________________________________________________________________

72. Mostrar los huéspedes que tengan más de una reserva.

```sql
SELECT 
client.client_id, 
CONCAT(client."name", ' ', 
client.surname) AS huesped, 
COUNT(reservation.reservation_id) AS total_reservas 
FROM client 
JOIN reservation ON client.client_id = reservation.client_id 
GROUP BY client.client_id, client."name", client.surname 
HAVING COUNT(reservation.reservation_id) > 1;
```

**Resultado:**

<img width="425" height="167" alt="72clientsWithMoreThanOneReservation" src="https://github.com/user-attachments/assets/42b6b0dc-a4e4-49c4-91bc-9ba22dd3ac03" />

__________________________________________________________________________________

73. Mostrar las habitaciones que tengan más de un servicio asignado.

```sql
SELECT 
room.room_id, 
room."name" AS habitacion, 
COUNT(facilities.facilities_id) AS total_servicios 
FROM room 
JOIN facilities ON room.room_id = facilities.room_id 
GROUP BY room.room_id, room."name" 
HAVING COUNT(facilities.facilities_id) > 1;
```

**Resultado:**

<img width="517" height="151" alt="73roomsWithMoreThanOneFacility" src="https://github.com/user-attachments/assets/78737e3c-fcd0-4c20-a4f5-0158124bfd1d" />

__________________________________________________________________________________

