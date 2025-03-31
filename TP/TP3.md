DROP DATABASE IF EXISTS zoo;
CREATE DATABASE zoo CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE zoo;

CREATE TABLE IF NOT EXISTS chat(
 id INT NOT NULL AUTO_INCREMENT,
 nom VARCHAR(50) NOT NULL,
 yeux VARCHAR(20) NOT NULL,
 age INT NOT NULL,
 CONSTRAINT pk_chat PRIMARY KEY (id)
)ENGINE=INNODB;
USE zoo;
-- vider la table
TRUNCATE chat;
-- INSERTION DES DONNES
INSERT INTO chat (nom,yeux,age)
VALUES
('maine coon','marron',20),
('siamois','bleu',15),
('bengal','marron',18),
('scottish Fold','marron',10);
USE zoo;
SELECT
    id,nom,yeux,age
FROM chat
ORDER BY nom,age;
USE zoo;
SELECT 
    nom, age 
FROM chat 
WHERE age >=11
AND age <=19;
USE zoo; 
SELECT 
    nom, age 
FROM chat  
WHERE nom LIKE 'sia%';
USE zoo; 
SELECT 
    nom, age 
FROM chat  
WHERE nom LIKE '%a%';
USE zoo;
SELECT 
    AVG(age) AS age_moyen
FROM chat; 
USE zoo;
SELECT 
    COUNT(id) AS nb_chat
FROM chat; 
USE zoo;
SELECT
    yeux AS couleur,
    COUNT(id) AS nb_chat
FROM chat
WHERE yeux ='marron'
GROUP BY (yeux);
USE zoo;
SELECT
id,
nom,
yeux,
age
FROM chat
ORDER BY AGE ASC
LIMIT 1;
USE zoo;
SELECT
id,
nom,
yeux,
age
FROM chat
ORDER BY AGE DESC
LIMIT 1;
USE zoo;
SELECT
    yeux AS couleur,
    COUNT(id) AS nb_chat
FROM chat
GROUP BY (yeux);