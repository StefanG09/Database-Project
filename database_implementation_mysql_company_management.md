# Database Project for Company Management
The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

**Application under test**: Company Management

**Tools used**: MySQL Workbench

**Database description:** The purpose of the database is to handle all data pertaining to a company in order to be able manage all resources related to that company

**Database Schema:**

![image](https://github.com/user-attachments/assets/a9552c69-3d35-4da7-a824-3d855fcb4cba)

You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.
The tables are connected in the following way:

The **departamente** table is connected to the **angajati** table through a **one-to-many** relationship implemented by using **departamente.nume_departament** as the primary key and **angajati.departament** as the foreign key.
 The **angajati** table is connected to the **masini** table through a **one-to-many** relationship implemented by using **angajati.id** as the primary key and **masini.id_angajat** as the foreign key. 
The **angajati** table is connected to the **telefoane** table through a **one-to-many** relationship implemented by using **angajati.id** as the primary key and **telefoane.id_angajat** as the foreign key.
 The **angajati** table is connected to the **angajati_training** table through a **one-to-many** relationship implemented by using **angajati.id** as the primary key and **angajati_training.id_angajat** as the foreign key. 
The **training** table is connected to the **angajati_training** table through a **one-to-many** relationship implemented by using **training.id_training** as the primary key and **angajati_training.id_training** as the foreign key.

![image](https://github.com/user-attachments/assets/1af8cfe3-0442-43e4-9a98-40436e33c963)


## Database Queries

### DDL (Data Definition Language)
The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

```
create database COMPANIE;
```

```
use COMPANIE;
```

```
create table DEPARTAMENTE (
nume_departament varchar (20) primary key,
data_infiintarii date,
buget_anual int,
functie varchar (20),
contact bigint,
locatie varchar (20)
);
```

```
create table ANGAJATI(
id int primary key auto_increment,
nume varchar(30),
prenume varchar(20),
data_nasterii date,
tara_de_origine varchar (30),
oras_de_origine varchar (30),
sex char (1),
adresa_actuala varchar (50),
numar_telefon varchar(15),
departament varchar (20),
functie varchar (20),
FOREIGN KEY (departament) REFERENCES DEPARTAMENTE(nume_departament)
);
```

```

create table MASINI (
numar_inmatriculare varchar(40) primary key,
id_angajat int,
marca_auto varchar (15),
model_auto varchar (20),
an_fabricatie int,
kilometraj int,
stare_tehnica varchar (10),
utilizare varchar (20),
FOREIGN KEY (id_angajat) REFERENCES ANGAJATI(id)
);
```

```
create table TELEFOANE (
numar_inventar int primary key,
id_angajat int,
marca_tel varchar (10),
model_tel varchar (15),
data_achizitie date,
numar_tel bigint,
stare varchar (10),
data_ultima_actualizare_software date,
FOREIGN KEY (id_angajat) REFERENCES ANGAJATI(id)
);
```

```
CREATE TABLE TRAINING (
    id_training INT PRIMARY KEY AUTO_INCREMENT,
    nume_training VARCHAR(50),
    data_incepere DATE,
    data_sfarsit DATE,
    locatie VARCHAR(50),
    departament_responsabil VARCHAR(20),
    numar_participanti INT,
    instructor VARCHAR(50),
    contact_instructor BIGINT,
    descriere TEXT
);
```

```

CREATE TABLE ANGAJATI_TRAINING (
    id_angajat INT,
    id_training INT,
    data_participare DATE,
    PRIMARY KEY (id_angajat, id_training),
    FOREIGN KEY (id_angajat) REFERENCES ANGAJATI(id),
    FOREIGN KEY (id_training) REFERENCES TRAINING(id_training)
);
```


After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

```
ALTER TABLE ANGAJATI
DROP COLUMN oras_de_origine;
```

```
ALTER TABLE TELEFOANE
RENAME COLUMN data_ultima_actualizare_software TO ultima_actualizare;
```

```
ALTER TABLE MASINI
MODIFY COLUMN numar_inmatriculare VARCHAR(20);
```

```
ALTER TABLE DEPARTAMENTE
RENAME COLUMN functie TO rolul_departamentului;
```


### DML (Data Manipulation Language)
In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase.

Below you can find all the insert instructions that were created in the scope of this project:

```
INSERT INTO DEPARTAMENTE (nume_departament, data_infiintarii, buget_anual, functie, contact, locatie) VALUES
('IT', '2005-06-15', 500000, 'Suport Tehnic', 40712345678, 'București'),
('HR', '2010-03-10', 300000, 'Resurse Umane', 40723456789, 'Cluj-Napoca'),
('Finanțe', '2008-11-20', 700000, 'Gestionare Finanțe', 40734567890, 'Timișoara'),
('Marketing', '2012-08-05', 400000, 'Promovare', 40745678901, 'Iași'),
('Vânzări', '2007-01-30', 600000, 'Gestionare Vânzări', 40756789012, 'Constanța');
```



```
INSERT INTO ANGAJATI (nume, prenume, data_nasterii, tara_de_origine, oras_de_origine, sex, adresa_actuala, numar_telefon, departament, functie) VALUES
('Popescu', 'Ion', '1985-05-12', 'România', 'București', 'M', 'Strada X, Nr. 10', '40712345678', 'IT', 'Dezvoltator'),
('Ionescu', 'Maria', '1990-03-25', 'România', 'Cluj', 'F', 'Strada Y, Nr. 5', '40722345678', 'HR', 'Recrutor'),
('Schmidt', 'Hans', '1987-07-19', 'Germania', 'Berlin', 'M', 'Strada A, Nr. 15', '491512345678', 'Finanțe', 'Contabil'),
('Dubois', 'Pierre', '1979-01-15', 'Franța', 'Paris', 'M', 'Strada B, Nr. 20', '33612345678', 'Marketing', 'Manager'),
('Garcia', 'Luis', '1992-11-30', 'Spania', 'Madrid', 'M', 'Strada C, Nr. 25', '34612345678', 'Vânzări', 'Reprezentant'),
('Smith', 'John', '1988-12-05', 'Regatul Unit', 'Londra', 'M', 'Strada D, Nr. 30', '44712345678', 'IT', 'Suport'),
('Nielsen', 'Emma', '1995-02-10', 'Danemarca', 'Copenhaga', 'F', 'Strada E, Nr. 35', '45212345678', 'HR', 'Coordonator'),
('Rossi', 'Giulia', '1983-08-23', 'Italia', 'Roma', 'F', 'Strada F, Nr. 40', '39312345678', 'Finanțe', 'Analist'),
('Novak', 'Ivan', '1975-09-11', 'Croația', 'Zagreb', 'M', 'Strada G, Nr. 45', '38512345678', 'Marketing', 'Specialist'),
('Kovač', 'Ana', '1982-04-18', 'Slovenia', 'Ljubljana', 'F', 'Strada H, Nr. 50', '38612345678', 'Vânzări', 'Manager'),
('Lefevre', 'Chloe', '1991-07-08', 'Franța', 'Lyon', 'F', 'Strada I, Nr. 60', '33698765432', 'IT', 'Dezvoltator'),
('Jensen', 'Lars', '1980-11-21', 'Danemarca', 'Aarhus', 'M', 'Strada J, Nr. 70', '45287654321', 'HR', 'Manager'),
('Martinez', 'Carlos', '1986-03-17', 'Spania', 'Barcelona', 'M', 'Strada K, Nr. 80', '34623456789', 'Finanțe', 'Auditor'),
('Gonzalez', 'Sofia', '1993-10-30', 'Spania', 'Sevilia', 'F', 'Strada L, Nr. 90', '34634567890', 'Marketing', 'Coordonator'),
('Mueller', 'Anna', '1984-06-25', 'Germania', 'Hamburg', 'F', 'Strada M, Nr. 100', '49123456789', 'Vânzări', 'Executiv'),
('Verdi', 'Luca', '1978-12-12', 'Italia', 'Milano', 'M', 'Strada N, Nr. 110', '39345678901', 'IT', 'Analist'),
('Petrov', 'Elena', '1992-02-14', 'Bulgaria', 'Sofia', 'F', 'Strada O, Nr. 120', '35912345678', 'HR', 'Asistent'),
('Horvath', 'Zoltan', '1981-04-07', 'Ungaria', 'Budapesta', 'M', 'Strada P, Nr. 130', '36123456789', 'Finanțe', 'Controlor'),
('Olsen', 'Maja', '1989-11-19', 'Danemarca', 'Odense', 'F', 'Strada Q, Nr. 140', '45234567890', 'Marketing', 'Strategist'),
('Andersen', 'Frederik', '1994-09-28', 'Danemarca', 'Aalborg', 'M', 'Strada R, Nr. 150', '45245678901', 'Vânzări', 'Asociat'),
('Doe', 'Jane', '1987-12-31', 'Regatul Unit', 'Manchester', 'F', 'Strada S, Nr. 160', '44723456789', 'IT', 'Suport'),
('Muller', 'Clara', '1986-05-15', 'Germania', 'Munchen', 'F', 'Strada T, Nr. 170', '49134567890', 'HR', 'Specialist'),
('Nash', 'Peter', '1990-07-12', 'Regatul Unit', 'Birmingham', 'M', 'Strada U, Nr. 180', '44734567890', 'Finanțe', 'Consultant'),
('Klein', 'Lucas', '1983-03-05', 'Germania', 'Koln', 'M', 'Strada V, Nr. 190', '49145678901', 'Marketing', 'Consultant'),
('Radu', 'Andreea', '1991-11-21', 'România', 'Timișoara', 'F', 'Strada W, Nr. 200', '40734567890', 'Vânzări', 'Reprezentant'),
('Dumitru', 'Mihai', '1984-08-12', 'România', 'Iași', 'M', 'Strada X, Nr. 210', '40745678901', 'IT', 'Inginer'),
('Larsen', 'Karin', '1985-12-17', 'Danemarca', 'Esbjerg', 'F', 'Strada Y, Nr. 220', '45256789012', 'HR', 'Manager'),
('Weber', 'Tim', '1982-10-09', 'Germania', 'Dusseldorf', 'M', 'Strada Z, Nr. 230', '49156789012', 'Finanțe', 'Consultant');
```




```
INSERT INTO MASINI (numar_inmatriculare, id_angajat, marca_auto, model_auto, an_fabricatie, kilometraj, stare_tehnica, utilizare) VALUES
('B-123-XYZ', 1, 'Dacia', 'Duster', 2018, 50000, 'Buna', 'Serviciu'),
('CJ-456-ABC', 2, 'Volkswagen', 'Golf', 2017, 60000, 'Buna', 'Personala'),
('TM-789-DEF', 3, 'Ford', 'Focus', 2019, 30000, 'Buna', 'Serviciu'),
('IS-101-GHI', 4, 'Toyota', 'Corolla', 2020, 15000, 'Noua', 'Personala'),
('BV-202-JKL', 5, 'Renault', 'Megane', 2016, 80000, 'Buna', 'Serviciu'),
('B-303-MNO', 6, 'BMW', 'X5', 2015, 90000, 'Buna', 'Personala'),
('CT-404-PQR', 7, 'Audi', 'A4', 2018, 45000, 'Buna', 'Serviciu'),
('OT-505-STU', 8, 'Mercedes', 'C-Class', 2017, 70000, 'Buna', 'Personala'),
('PH-606-VWX', 9, 'Skoda', 'Octavia', 2019, 35000, 'Buna', 'Serviciu'),
('BR-707-YZA', 10, 'Hyundai', 'Tucson', 2020, 20000, 'Noua', 'Personala'),
('GL-909-EFG', 12, 'Peugeot', '308', 2016, 75000, 'Buna', 'Personala'),
('B-010-HIJ', 13, 'Nissan', 'Qashqai', 2019, 40000, 'Buna', 'Serviciu'),
('DJ-111-KLM', 14, 'Mazda', 'CX-5', 2017, 65000, 'Buna', 'Personala'),
('B-212-NOP', 15, 'Honda', 'Civic', 2018, 50000, 'Buna', 'Serviciu'),
('AR-313-QRS', 16, 'Chevrolet', 'Cruze', 2015, 85000, 'Buna', 'Personala'),
('B-414-TUV', 17, 'Opel', 'Astra', 2016, 70000, 'Buna', 'Serviciu'),
('B-818-FGH', 21, 'Suzuki', 'Vitara', 2018, 50000, 'Buna', 'Serviciu'),
('NT-919-IJK', 22, 'Subaru', 'Forester', 2019, 30000, 'Buna', 'Personala'),
('B-020-LMN', 23, 'Mitsubishi', 'Outlander', 2016, 72000, 'Buna', 'Serviciu');
```



```
INSERT INTO TELEFOANE (numar_inventar, id_angajat, marca_tel, model_tel, data_achizitie, numar_tel, stare, data_ultima_actualizare_software) VALUES
(1001, 1, 'Apple', 'iPhone 12', '2021-01-15', 40712345678, 'Buna', '2023-01-15'),
(1002, 2, 'Samsung', 'Galaxy S21', '2021-02-10', 40723456789, 'Buna', '2023-02-10'),
(1003, 3, 'Huawei', 'P30', '2020-05-20', 40734567890, 'Buna', '2023-05-20'),
(1004, 4, 'Xiaomi', 'Mi 10', '2020-08-30', 40745678901, 'Buna', '2023-08-30'),
(1005, 5, 'Nokia', '8.3', '2019-11-05', 40756789012, 'Buna', '2022-11-05'),
(1006, 6, 'Apple', 'iPhone 11', '2020-03-15', 40767890123, 'Buna', '2023-03-15'),
(1007, 7, 'Samsung', 'Galaxy A52', '2021-06-25', 40778901234, 'Buna', '2023-06-25'),
(1008, 8, 'Huawei', 'Mate 40', '2021-09-10', 40789012345, 'Buna', '2023-09-10'),
(1009, 9, 'Xiaomi', 'Redmi Note 9', '2020-12-01', 40790123456, 'Buna', '2022-12-01'),
(1010, 10, 'Nokia', '7.2', '2019-04-10', 40701234567, 'Buna', '2022-04-10'),
(1011, 11, 'Apple', 'iPhone SE', '2021-07-20', 40712349876, 'Buna', '2023-07-20'),
(1012, 12, 'Samsung', 'Galaxy S20', '2020-11-15', 40723450987, 'Buna', '2023-11-15'),
(1013, 13, 'Huawei', 'P40', '2021-02-25', 40734562098, 'Buna', '2023-02-25'),
(1014, 14, 'Xiaomi', 'Mi 9', '2019-05-30', 40745673109, 'Buna', '2022-05-30'),
(1015, 15, 'Nokia', '6.2', '2020-01-20', 40756784210, 'Buna', '2023-01-20');
```



```
INSERT INTO TRAINING (nume_training, data_incepere, data_sfarsit, locatie, departament_responsabil, numar_participanti, instructor, contact_instructor, descriere) VALUES
('Curs de Programare Java', '2024-07-01', '2024-07-05', 'Sala 101', 'IT', 20, 'Ion Popescu', 40712345678, 'Curs intensiv de programare în limbajul Java.'),
('Training de Management', '2024-08-10', '2024-08-12', 'Sala de conferințe', 'HR', 15, 'Maria Ionescu', 40723456789, 'Training pentru dezvoltarea abilităților de management.'),
('Workshop Finanțe Avansate', '2024-09-15', '2024-09-17', 'Sala 205', 'Finanțe', 25, 'Hans Schmidt', 491512345678, 'Workshop avansat pentru analiza financiară.'),
('Curs de Marketing Digital', '2024-10-20', '2024-10-22', 'Sala 305', 'Marketing', 30, 'Pierre Dubois', 33612345678, 'Curs despre strategiile moderne de marketing digital.'),
('Sesiune de Vânzări Eficiente', '2024-11-05', '2024-11-07', 'Sala 102', 'Vânzări', 20, 'Luis Garcia', 34612345678, 'Sesiune de training pentru tehnici eficiente de vânzări.');
```



```
INSERT INTO ANGAJATI_TRAINING (id_angajat, id_training, data_participare) VALUES
(1, 1, '2024-07-01'),
(1, 2, '2024-08-10'),
(2, 3, '2024-09-15'),
(2, 4, '2024-10-20'),
(3, 1, '2024-07-01'),
(3, 5, '2024-11-05'),
(4, 2, '2024-08-10'),
(4, 4, '2024-10-20'),
(5, 3, '2024-09-15'),
(5, 5, '2024-11-05'),
(6, 1, '2024-07-01'),
(6, 2, '2024-08-10'),
(3, 4, '2024-10-20'),
(1, 3, '2024-09-15'),
(2, 5, '2024-11-05');
```

After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

```
UPDATE telefoane
SET stare = 'Defect'
WHERE marca_tel IN ('Huawei', 'Samsung');
```

```
UPDATE masini
SET kilometraj = kilometraj + 10000, stare_tehnica = 'Buna'
WHERE numar_inmatriculare = 'IS-101-GHI';
```

### DQL (Data Query Language)
In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

```
-- We want to see what departments have employees
SELECT DISTINCT(departament) FROM ANGAJATI;
```

```
SELECT * FROM DEPARTAMENTE;
```

```
SELECT * FROM TELEFOANE;
```

```
-- Searching for men who work in IT
SELECT prenume, nume, departament
FROM angajati
WHERE sex = 'M' AND departament = 'IT';
```

```
-- Count how many phones of each brand we have. We want to see which are the most common (more than 2 phones).
SELECT COUNT(numar_inventar) as numar, marca_tel 
FROM telefoane
GROUP BY marca_tel
HAVING numar > 2;
```

```
-- We want to see the new cars or those made by Dacia.
SELECT numar_inmatriculare, an_fabricatie, marca_auto, id_angajat
FROM masini
WHERE stare_tehnica = 'Noua' OR marca_auto = 'Dacia';
```

```
-- We want to see non-HR trainings with more than 20 participants.
SELECT id_training, nume_training, locatie, numar_participanti
FROM training
WHERE NOT(departament_responsabil = 'HR' OR numar_participanti <= 20);
```

```
 -- We want to see every phone whose name starts with “p” or begins with a letter, continues with “Phone“, then continues with any string.
SELECT numar_inventar, marca_tel, model_tel, id_angajat
FROM telefoane
WHERE model_tel LIKE('P%') OR model_tel LIKE('_Phone%');
```

```
-- We want to see where each employee works
SELECT angajati.id, angajati.nume, angajati.prenume, angajati.departament, departamente.locatie
FROM angajati INNER JOIN departamente 
ON angajati.departament = departamente.nume_departament;
```

```
-- We want to see which trainings each employee is enrolled in
SELECT angajati.id, angajati.nume, angajati.prenume, angajati_training.id_training, training.nume_training
FROM angajati LEFT JOIN angajati_training
ON angajati.id = angajati_training.id_angajat
INNER JOIN training
ON  angajati_training.id_training = training.id_training;
```

```
-- Search for employees with newer cars (after 2019)
SELECT id, prenume, nume, functie, sex
FROM angajati
WHERE id IN (
SELECT id_angajat
FROM masini
WHERE an_fabricatie > 2019
);
```

```
-- Find employees from Romania or Bulgaria
SELECT nume, prenume, departament
FROM ANGAJATI
WHERE tara_de_origine = 'România' OR tara_de_origine = 'Bulgaria';
```

```
-- Find the average budget for a department
SELECT AVG(buget_anual) AS buget_mediu
FROM DEPARTAMENTE;
```

```
-- Find the smallest budget for a department
SELECT MIN(buget_anual) AS buget_minim
FROM DEPARTAMENTE;
```

```
-- Count the female HR employees
SELECT nume, prenume, functie
FROM ANGAJATI
WHERE departament = 'HR' AND sex = 'F';
```

```
-- Count the employees in the marketing department
SELECT COUNT(*) AS numar_angajati
FROM ANGAJATI
WHERE departament = 'Marketing';
```

```
-- Search departments with at least 5 employees
SELECT departament, COUNT(*) AS numar_angajati
FROM ANGAJATI
GROUP BY departament
HAVING COUNT(*) > 5;
```

```
-- Look up all Apple phones used in the company
SELECT *
FROM telefoane
WHERE marca_tel = 'Apple';
```

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean:

```
DELETE FROM angajati_training
WHERE id_angajat = 6;
```
```
DELETE
FROM masini
WHERE id_angajat IN (
SELECT id 
FROM angajati 
WHERE tara_de_origine = 'Regatul Unit');

DELETE
FROM telefoane
WHERE id_angajat IN (
SELECT id 
FROM angajati 
WHERE tara_de_origine = 'Regatul Unit');

DELETE FROM angajati
WHERE tara_de_origine = 'Regatul Unit';
```

### Conclusions
The aim of this project was to design and manage a database to streamline the administration of human resources, equipment, and departments. By organizing critical information into interconnected tables, we created a system that ensures efficient data management and reporting, thereby enhancing overall productivity.
