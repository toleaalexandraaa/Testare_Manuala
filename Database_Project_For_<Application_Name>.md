# Database Project for HOTEL

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

**Application under test:** HOTEL

**Tools used:** MySQL Workbench

**Database description:** Baza de date "Hotel" are scopul de a gestiona informațiile esențiale ale unui hotel, incluzând datele clienților, rezervările efectuate, angajații și programul acestora, precum și alocările de personal în timpul șederii clienților.

Informațiile salvate:

Clienti: Informații despre clienți, inclusiv nume, prenume, email, telefon și gen. Aceste date sunt utile pentru procesarea rezervărilor și pentru contactarea clienților în legătură cu detalii legate de sejur.

Functie: Detalii despre funcțiile angajaților din hotel, incluzând salariile minime și maxime pentru fiecare funcție (ex. Manager, Administrator, Bucătar, Cameristă, Receptioner). 

Personal (Angajat): Date despre angajați, cum ar fi numele, prenumele, funcția, salariul, data angajării și adresa. Acestea sunt folosite pentru a monitoriza personalul disponibil și pentru a calcula salariile acestora.

Rezervare: Informații referitoare la rezervările clienților, inclusiv ID-ul clientului, data de check-in, data de check-out, numărul de camere și valoarea totală de plată. 

AlocarePersonal: Informații despre angajații alocați pentru diverse activități sau servicii în timpul șederii clienților. Include detalii precum ID-ul angajatului și ID-ul rezervării. 

ProgramPersonal: Informații despre programul angajaților, inclusiv perioadele de lucru și tipul programului (de exemplu, tura de zi sau tura de noapte).

## Database Schema 
  
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

The tables are connected in the following way:

- Tabel Angajat is connected with tabela functie through a tip 1:N relationship which was implemented through Functie.FunctieID as a primary key and Angajat.FunctieID as a foreign key
- Tabela Rezervare is connected with tabela Clienti through a 1:N relationship which was implemented through Clienti.ClientiID as a primary key and Rezervare.ClientiD as a foreign key
- Tabela AlocarePersonal is connected with Tabela Personal through a 1:N relationship which was implemented through PersonalPersonalID as a primary key and AlocarePersonal.PersonalID as a foreign key
- Tabela ProgramPersonal is connected with Personal through a 1:N relationship which was implemented through Personal.PersonalID as a primary key and ProgramPersonal.PersonalID as a foreign key

## Database Queries

### DDL (Data Definition Language)

  The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

create database hotel;
use hotel;

CREATE TABLE Clienti (
    ClientID INT PRIMARY KEY AUTO_INCREMENT,
    Nume VARCHAR(50) NOT NULL,
    Prenume VARCHAR(50) NOT NULL,
    Email VARCHAR(100),
    Telefon VARCHAR(15)
);
CREATE TABLE Functie (
    FunctieID INT PRIMARY KEY,
    Denumire VARCHAR(50),
    Salariu_Minim DECIMAL(10, 2),
    Salariu_Maxim DECIMAL(10, 2)
);

CREATE TABLE Personal (
    PersonalID INT PRIMARY KEY AUTO_INCREMENT,
    Nume VARCHAR(50) NOT NULL,
    Prenume VARCHAR(50) NOT NULL,
    FunctieID INT,
    Salariu DECIMAL(10, 2),
    Data_Angajare date,
    Adresa VARCHAR (100),
    FOREIGN KEY (FunctieID) REFERENCES Functie(FunctieID)
);

CREATE TABLE Rezervare (
    RezervareID INT PRIMARY KEY AUTO_INCREMENT,
    ClientID INT,
    DataCheckIn DATE,
    DataCheckOut DATE,
    NumarCamere INT,
    TotalPlata DECIMAL(10, 2),
    FOREIGN KEY (ClientID) REFERENCES Clienti(ClientID)
); 

CREATE TABLE AlocarePersonal (
    AlocareID INT PRIMARY KEY AUTO_INCREMENT,
    PersonalID INT,
    RezervareID INT,
    DataAlocare DATE,
    FOREIGN KEY (PersonalID) REFERENCES Personal(PersonalID),
    FOREIGN KEY (RezervareID) REFERENCES Rezervare(RezervareID)
);

CREATE TABLE ProgramPersonal (
    ProgramID INT PRIMARY KEY AUTO_INCREMENT,
    PersonalID INT,
    DataIncepere DATE,
    DataSfarsit DATE,
    TipProgram VARCHAR(50),
    FOREIGN KEY (PersonalID) REFERENCES Personal(PersonalID)
);

  After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

ALTER TABLE Personal RENAME TO Angajat;
ALTER TABLE Clienti ADD COLUMN Gen VARCHAR(10);
DESC Clienti;
ALTER TABLE ProgramPersonal ADD COLUMN TipConcediu VARCHAR(50);
ALTER TABLE ProgramPersonal RENAME TO Concediu;
ALTER TABLE Clienti MODIFY Gen VARCHAR(10) NOT NULL;

 
  
### DML (Data Manipulation Language)

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:
INSERT INTO functie (FunctieID, Denumire, Salariu_Minim, Salariu_Maxim) 
VALUES 
(1, 'Manager', 10000, NULL),
(2, 'Administrator', 5000, 9000),
(3, 'Bucatar', 1000, 1500),
(4, 'Camerista', 600, 900),
(5, 'Receptioner', 500, 700);
select * from functie;

INSERT INTO angajat (Nume, Prenume, FunctieID, Salariu, Data_Angajare, Adresa)
VALUES
('Popescu', 'Ion', 1, 12000, '2022-05-01', 'Str. Primăverii 12'),
('Ionescu', 'Maria', 2, 7000, '2021-03-12', 'Str. Florilor 7'),
('Georgescu', 'Andrei', 3, 2500, '2023-01-23', 'Str. Crizantemelor 4'),
('Vasilescu', 'Elena', 4, 3500, '2020-09-30', 'Str. Nucilor 10'),
('Dumitru', 'Alexandru', 5, 2000, '2019-07-15', 'Str. Bălăița 22');
SELECT * from angajat;

INSERT INTO Clienti (Nume, Prenume, Email, Telefon, Gen)
VALUES 
('Popescu', 'Ana', 'ana.popescu@gmail.com', '0745123456', 'Feminin'),
('Stan', 'Dana', 'dana.st09@gmail.com', '0765890672', 'Feminin'),
('Marcu', 'Andrei', 'andreimarcu@gmail.com', '0721675900', 'Masculin'),
('Cristea', 'Catalin', 'cristeaflorin73@gmail.com', '0756745120', 'Masculin'),
('Folea', 'Andreea', 'andfolea@gmail.com', '0732551127', 'Feminin');
SELECT * FROM clienti;

INSERT INTO Rezervare
VALUES 
    (NULL, 1, '2024-10-15', '2024-10-20', 2, 1500.00),
    (NULL, 2, '2024-11-01', '2024-11-05', 1, 800.00),
    (NULL, 3, '2024-12-23', '2024-12-27', 4, 4000.00);
    SELECT * FROM rezervare;

INSERT INTO Concediu
VALUES 
    (NULL, 1, '2024-10-01', '2024-10-15', 'Tura de zi', 'Concediu medical'),
    (NULL, 2, '2024-10-10', '2024-10-20', 'Tura de noapte', 'Concediu de odihnă');
    SELECT * FROM Concediu;

  After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

 UPDATE Angajat
SET Salariu = 4000.00
WHERE Nume = 'Dumitru' AND Prenume = 'Alexandru';
SELECT * FROM Angajat;

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 

DELETE FROM Concediu
WHERE ProgramID = 1;
SELECT * FROM Concediu;


### DQL (Data Query Language)


In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

SELECT Nume, Prenume, Salariu 
FROM Angajat
WHERE Salariu > (SELECT AVG(Salariu) FROM Angajat);

SELECT Nume, Prenume
FROM Angajat
WHERE Salariu = (SELECT MAX(Salariu) FROM Angajat);

SELECT Clienti.Nume, Clienti.Prenume, Rezervare.DataCheckIn, AlocarePersonal.DataAlocare
FROM Clienti
LEFT JOIN Rezervare ON Clienti.ClientID = Rezervare.ClientID
LEFT JOIN AlocarePersonal ON Rezervare.RezervareID = AlocarePersonal.RezervareID;

SELECT Clienti.Nume, Clienti.Prenume, Rezervare.DataCheckIn, Rezervare.DataCheckOut
FROM Clienti
INNER JOIN Rezervare ON Clienti.ClientID = Rezervare.ClientID;

SELECT Angajat.Nume AS Employee, Clienti.Nume AS Client
FROM Angajat
CROSS JOIN Clienti;

SELECT ClientID, SUM(TotalPlata) AS TotalCheltuieli
FROM Rezervare
GROUP BY ClientID
HAVING SUM(TotalPlata) > 100;

SELECT Functie.Denumire, AVG(Angajat.Salariu) AS AverageSalary
FROM Angajat
INNER JOIN Functie ON Angajat.FunctieID = Functie.FunctieID
GROUP BY Functie.Denumire
HAVING AVG(Angajat.Salariu) > 1000;

SELECT Nume, Prenume, Email
FROM Clienti
WHERE Nume LIKE 'A%' OR Nume LIKE 'P%';

SELECT * FROM Clienti
WHERE NOT Nume LIKE 'C%';

SELECT * FROM Clienti
WHERE Email LIKE '%gmail%' AND Telefon LIKE '07%';
## Conclusions

Proiectul ”HOTEL” a avut ca scop crearea unei baze de date complete și eficiente pentru gestionarea activităților unui hotel, acoperind aspecte precum managementul rezervărilor, alocarea personalului și monitorizarea programului de lucru. Prin implementarea tabelelor interconectate și utilizarea tehnicilor avansate de interogare SQL, am reușit să construiesc o soluție robustă pentru gestionarea datelor specifice unei unități hoteliere.

În timpul acestui proiect am lucrat cu:
Crearea și modificarea tabelelor (inclusiv redenumiri și adăugări de coloane).

Gestionarea relațiilor între tabele folosind chei primare și chei străine.

Insert, update și delete pentru gestionarea datelor în tabele.

Subquery-uri și funcții agregate pentru a extrage rapoarte complexe, utile în managementul hotelului.

Diverse tipuri de join-uri (left, right, inner) pentru interogări eficiente între tabele.

Am învățat cum să structurez o bază de date într-un mod eficient și scalabil, să folosesc corect funcțiile SQL și să implementez relațiile dintre entitățile din baza de date pentru a oferi o soluție optimă. În plus, am explorat cum agregarea datelor poate contribui la analize relevante și rapoarte utile în domeniul hotelier. Această experiență mă ajută să înțeleg mai bine gestionarea datelor și să aplic tehnici avansate în viitoarele proiecte.

