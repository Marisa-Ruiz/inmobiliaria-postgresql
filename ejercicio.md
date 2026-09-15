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

