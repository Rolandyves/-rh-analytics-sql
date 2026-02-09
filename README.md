# -rh-analytics-sql
Projet RH Analytics – Analyse des effectifs, performance et turnover avec SQL
rh-analytics-sql/
│
├── data/
│   ├── employes.csv
│   ├── departements.csv
│   ├── performances.csv
│   └── turnover.csv
│
├── sql/
│   ├── schema.sql
│   ├── insert_data.sql
│   └── analysis.sql
│
├── report/
│   └── insights_rh.pdf   (optionnel plus tard)
│
└── README.md
# RH Analytics — Analyse SQL des effectifs, performance et turnover

## 🎯 Objectif
Ce projet vise à fournir une vue complète des ressources humaines :
- effectifs actifs
- turnover
- performance des employés
- KPIs RH par département et cohorte d’embauche

## 🗂 Données
- employes (id, nom, departement_id, poste, salaire, date_embauche, date_depart)
- departements (id, nom, manager, budget)
- performances (employe_id, score, date_evaluation, objectifs_atteints)
- turnover (employe_id, date_depart, raison, type_depart, anciennete)

## 🛠 Outils
- SQL (PostgreSQL)
- (Optionnel) Python pour l’analyse avancée

## ❓ Questions business
- Effectifs actifs
- Turnover 12 mois
- Départements à risque
- Salaire moyen
- Top performers
- Rétention par cohorte

## 📊 Résultats attendus
- Identification des départements à fort turnover
- Classement des départements par performance
- Segmentation des employés par performance
- KPIs RH par cohorte

## 🚀 Auteur
Yves Roland PATTO  
Data Analyst Junior  
LinkedIn :yves roland patto  
Email : pyvesroland@gmail.com
git init
git add .
git commit -m "Initial commit - RH Analytics SQL project"
git branch -M main
git remote add origin https://github.com/TON_USERNAME/rh-analytics-sql.git
git push -u origin main
