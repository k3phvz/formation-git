CREATE DATABASE IF NOT EXISTS GestionDevisFactures;

USE GestionDevisFactures;

CREATE TABLE IF NOT EXISTS Clients (
    ClientID INT PRIMARY KEY AUTO_INCREMENT,
    NomClient VARCHAR(255) NOT NULL
);

CREATE TABLE IF NOT EXISTS Devis (
    DevisID VARCHAR(50) PRIMARY KEY,
    ClientID INT,
    Montant DECIMAL(10, 2),
    Projet VARCHAR(255),
    FOREIGN KEY (ClientID) REFERENCES Clients(ClientID)
);

CREATE TABLE IF NOT EXISTS Factures (
    FactureID VARCHAR(50) PRIMARY KEY,
    DevisID VARCHAR(50),
    Total DECIMAL(10, 2),
    DateFacture DATE,
    DatePaiement DATE,
    Info VARCHAR(255),
    FOREIGN KEY (DevisID) REFERENCES Devis(DevisID)
);

INSERT INTO Clients (NomClient) VALUES
('Mairie de Rennes'),
('Neo Soft'),
('Sopra'),
('Accenture'),
('Amazon');

INSERT INTO Devis (DevisID, ClientID, Montant, Projet) VALUES
('DEV2100A', 1, 3000.00, 'Creation de site internet'),
('DEV2100B', 1, 5000.00, 'Creation de site internet'),
('DEV2100C', 2, 5000.00, 'Logiciel CRM'),
('DEV2100D', 3, 3000.00, 'Logiciel de devis'),
('DEV2100E', 4, 5000.00, 'Site internet ecommerce'),
('DEV2100F', 2, 2000.00, 'logiciel ERP'),
('DEV2100G', 5, 1000.00, 'logiciel Gestion de Stock');

INSERT INTO Factures (FactureID, DevisID, Total, DateFacture, DatePaiement, Info) VALUES
('FA001', 'DEV2100A', 1500.00, '2023-09-01', '2023-10-01', 'Site internet partie 1'),
('FA002', 'DEV2100A', 1500.00, '2023-09-20', NULL, 'Site internet partie 2'),
('FA003', 'DEV2100C', 5000.00, '2024-02-01', NULL, 'Logiciel CRM'),
('FA004', 'DEV2100D', 3000.00, '2024-03-03', '2024-04-03', 'Logiciel devis'),
('FA005', 'DEV2100E', 5000.00, '2023-03-01', NULL, 'Site internet ecommerce'),
('FA006', 'DEV2100F', 2000.00, '2023-03-01', NULL, 'logiciel ERP');

SELECT
    f.FactureID AS ref,
    c.NomClient AS client,
    f.Info AS info,
    f.Total AS total,
    f.DateFacture AS date,
    f.DatePaiement AS paiement
FROM
    Factures f
JOIN
    Devis d ON f.DevisID = d.DevisID
JOIN
    Clients c ON d.ClientID = c.ClientID;

SELECT
    c.NomClient AS client,
    COUNT(f.FactureID) AS nb_factures
FROM
    Clients c
LEFT JOIN
    Devis d ON c.ClientID = d.ClientID
LEFT JOIN
    Factures f ON d.DevisID = f.DevisID
GROUP BY
    c.NomClient;

SELECT
    c.NomClient AS client,
    SUM(f.Total) AS ca_par_client
FROM
    Clients c
LEFT JOIN
    Devis d ON c.ClientID = d.ClientID
LEFT JOIN
    Factures f ON d.DevisID = f.DevisID
GROUP BY
    c.NomClient;

SELECT
    SUM(Total) AS ca_total
FROM
    Factures;

SELECT
    SUM(Total) AS total_factures
FROM
    Factures
WHERE
    DatePaiement IS NULL;

SELECT
    FactureID AS facture,
    DATEDIFF(CURDATE(), DateFacture) AS nb_jour
FROM
    Factures
WHERE
    DatePaiement IS NULL AND DATEDIFF(CURDATE(), DateFacture) > 30;

SELECT
    c.NomClient AS client,
    f.FactureID AS facture,
    DATEDIFF(CURDATE(), f.DateFacture) AS nb_jour
FROM
    Factures f
JOIN
    Devis d ON f.DevisID = d.DevisID
JOIN
    Clients c ON d.ClientID = c.ClientID
WHERE
    f.DatePaiement IS NULL AND DATEDIFF(CURDATE(), f.DateFacture) > 30;

SELECT
    c.NomClient AS client,
    f.FactureID AS facture,
    DATEDIFF(CURDATE(), f.DateFacture) AS nb_jour,
    DATEDIFF(CURDATE(), f.DateFacture) * 2 AS penalite
FROM
    Factures f
JOIN
    Devis d ON f.DevisID = d.DevisID
JOIN
    Clients c ON d.ClientID = c.ClientID
WHERE
    f.DatePaiement IS NULL AND DATEDIFF(CURDATE(), f.DateFacture) > 30;

SELECT
    AVG(ca_par_client) AS moyenne_ca_client
FROM
    (SELECT
        c.NomClient AS client,
        SUM(f.Total) AS ca_par_client
    FROM
        Clients c
    LEFT JOIN
        Devis d ON c.ClientID = d.ClientID
    LEFT JOIN
        Factures f ON d.DevisID = f.DevisID
    GROUP BY
        c.NomClient) AS ca_par_client_table;
