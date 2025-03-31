
DROP DATABASE IF EXISTS invitation;
CREATE DATABASE invitation CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE invitation;
DROP TABLE IF EXISTS inv_personne;
CREATE TABLE inv_personne(
    id int NOT NULL AUTO_INCREMENT, 
    prenom VARCHAR(100) NOT NULL,
    nom VARCHAR(100) NOT NULL,
    age INT NOT NULL,
    inscription DATE NOT NULL,
    etat TINYINT NOT NULL DEFAULT 1, -- 1 ok 0 pas ok
    statut ENUM('membre','non membre') NOT NULL DEFAULT 'non membre',
    cv TEXT, -- peut etre null / si on ne met pas NULL
    salaire INT NOT NULL,
    CONSTRAINT pk_personne PRIMARY KEY(id) # pk_personne le nom de la contrainte de clef primaire
) ENGINE=InnoDB; 
 