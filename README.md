# -rh-analytics-sql
Projet RH Analytics – Analyse des effectifs
CREATE TABLE departements (
    id INT PRIMARY KEY,
    nom TEXT,
    manager TEXT,
    budget NUMERIC
);

CREATE TABLE employes (
    id INT PRIMARY KEY,
    nom TEXT,
    departement_id INT REFERENCES departements(id),
    poste TEXT,
    salaire NUMERIC,
    date_embauche DATE,
    date_depart DATE
);

CREATE TABLE performances (
    id INT PRIMARY KEY,
    employe_id INT REFERENCES employes(id),
    score NUMERIC,
    date_evaluation DATE,
    objectifs_atteints BOOLEAN
);

CREATE TABLE turnover (
    id INT PRIMARY KEY,
    employe_id INT REFERENCES employes(id),
    date_depart DATE,
    raison TEXT,
    type_depart TEXT, -- volontaire / involontaire
    anciennete INT
);

SELECT COUNT(*) AS employes_actifs
FROM employes
WHERE date_depart IS NULL;

SELECT COUNT(*) AS departs_12_mois
FROM turnover
WHERE date_depart >= CURRENT_DATE - INTERVAL '12 months';

SELECT d.nom,
       COUNT(t.id) AS nombre_departs
FROM turnover t
JOIN employes e ON t.employe_id = e.id
JOIN departements d ON e.departement_id = d.id
GROUP BY d.nom
ORDER BY nombre_departs DESC;

SELECT d.nom,
       AVG(e.salaire) AS salaire_moyen
FROM employes e
JOIN departements d ON e.departement_id = d.id
GROUP BY d.nom;

SELECT nom
FROM employes
WHERE AGE(CURRENT_DATE, date_embauche) >= INTERVAL '5 years';

SELECT d.nom,
       AVG(p.score) AS performance_moyenne
FROM performances p
JOIN employes e ON p.employe_id = e.id
JOIN departements d ON e.departement_id = d.id
GROUP BY d.nom
ORDER BY performance_moyenne DESC;

SELECT e.nom,
       AVG(p.score) AS score_moyen
FROM performances p
JOIN employes e ON p.employe_id = e.id
WHERE p.date_evaluation >= CURRENT_DATE - INTERVAL '9 months'
GROUP BY e.nom
ORDER BY score_moyen DESC
LIMIT 10;

SELECT e.nom,
       AVG(p.score) AS score_moyen
FROM performances p
JOIN employes e ON p.employe_id = e.id
WHERE p.date_evaluation >= CURRENT_DATE - INTERVAL '9 months'
GROUP BY e.nom
ORDER BY score_moyen DESC
LIMIT 10;

SELECT e.nom,
       d.nom AS departement,
       AVG(p.score) AS score_moyen
FROM performances p
JOIN employes e ON p.employe_id = e.id
JOIN departements d ON e.departement_id = d.id
GROUP BY e.nom, d.nom
HAVING AVG(p.score) < 50;

SELECT DATE_TRUNC('year', date_embauche) AS cohorte,
       AVG(anciennete) AS retention_moyenne
FROM turnover t
JOIN employes e ON t.employe_id = e.id
GROUP BY cohorte
ORDER BY cohorte;

SELECT d.nom,
       COUNT(e.id) AS recrutements
FROM employes e
JOIN departements d ON e.departement_id = d.id
GROUP BY d.nom
ORDER BY recrutements DESC;

SELECT type_depart,
       COUNT(*) * 100.0 / (SELECT COUNT(*) FROM turnover) AS pourcentage
FROM turnover
GROUP BY type_depart;

SELECT poste,
       COUNT(*) AS effectif
FROM employes
GROUP BY poste;

SELECT e.nom
FROM employes e
LEFT JOIN performances p 
ON e.id = p.employe_id
AND EXTRACT(YEAR FROM p.date_evaluation) = EXTRACT(YEAR FROM CURRENT_DATE)
WHERE p.id IS NULL;

SELECT e.nom,
CASE
    WHEN AVG(p.score) < 50 THEN 'Faible'
    WHEN AVG(p.score) BETWEEN 50 AND 75 THEN 'Moyen'
    ELSE 'Élevé'
END AS niveau_performance
FROM performances p
JOIN employes e ON p.employe_id = e.id
GROUP BY e.nom;

SELECT d.nom AS departement,
       DATE_TRUNC('year', e.date_embauche) AS cohorte,
       COUNT(e.id) AS effectif,
       AVG(e.salaire) AS salaire_moyen,
       AVG(p.score) AS performance_moyenne
FROM employes e
JOIN departements d ON e.departement_id = d.id
LEFT JOIN performances p ON e.id = p.employe_id
GROUP BY d.nom, cohorte
ORDER BY d.nom, cohorte;
