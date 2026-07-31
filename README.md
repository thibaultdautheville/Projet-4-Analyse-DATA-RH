# Projet 4 — Identifiez les causes d'attrition au sein d'une ESN

## Contexte

Ce projet a été réalisé dans le cadre de ma formation **Data Scientist** avec OpenClassrooms, en parallèle de mon alternance au Pôle Économie du MEDEF. Il s'agit d'un exercice pédagogique basé sur un scénario professionnel fictif, indépendant de mon activité en entreprise.

Le scénario se présente ainsi : une ESN (Entreprise de Services du Numérique) fictive, **TechNova Partners**, spécialisée dans le conseil en transformation digitale et la vente d'applications SaaS, fait face à un taux de démission (turnover) anormalement élevé. Le département RH de l'entreprise mandate un consultant Data Scientist (moi, dans ce scénario) afin :

1. d'identifier les causes potentielles de ces départs ;
2. de proposer des leviers d'action concrets aux équipes RH.

## Objectif du projet

L'objectif n'est pas seulement de prédire si un employé va quitter l'entreprise, mais surtout de **comprendre pourquoi**. La finalité est actionnable : donner aux RH des clés de lecture claires, fiables et exploitables pour orienter leur politique de rétention des talents.

Pour cela, le projet s'appuie sur une démarche complète de Data Science :
- exploration et compréhension de la donnée ;
- préparation et nettoyage des données ;
- modélisation de type classification supervisée ;
- interprétation du modèle grâce aux techniques d'explicabilité (feature importance globale et locale, SHAP).

## Données utilisées

Le projet repose sur trois fichiers sources fournis par l'entreprise fictive, correspondant à trois angles différents sur chaque employé :

- **extrait_sirh.csv** : données administratives et de carrière (âge, ancienneté, poste, salaire, département, etc.) ;
- **extrait_eval.csv** : données liées aux évaluations de performance ;
- **extrait_sondage.csv** : données de satisfaction interne (environnement de travail, équilibre vie pro/perso, équipe, etc.).

Ces trois fichiers ont été rapprochés via une jointure sur un identifiant commun, afin de constituer un **DataFrame central** regroupant l'ensemble des informations disponibles sur chaque salarié.

## Démarche suivie

Le projet est structuré en plusieurs étapes, chacune correspondant à un livrable précis :

### Étape 1 — Analyse exploratoire des données (EDA)
Exploration statistique et visuelle des trois fichiers sources et du fichier central, dans le but de faire émerger des différences clés entre les employés ayant quitté l'entreprise et ceux qui sont restés (comparaison de distributions, corrélations, visualisations quanti/quali).

### Étape 2 — Préparation de la donnée pour la modélisation
Nettoyage, encodage des variables catégorielles (One-Hot Encoding, mapping ordinal), gestion des variables binaires, standardisation des variables numériques, et construction des jeux **X** (features) et **y** (variable cible : départ ou non de l'entreprise).

### Étape 3 — Modélisation de classification (première approche)
Entraînement et comparaison de trois modèles :
- un modèle **Dummy** (modèle étalon, aléatoire selon les proportions de classes) ;
- un modèle **linéaire** (régression logistique) ;
- un modèle **non-linéaire** à base d'arbres (**XGBoost**).

Évaluation de chaque modèle sur le jeu d'entraînement et le jeu de test à l'aide de métriques de classification (accuracy, précision, rappel, f1-score, matrice de confusion), avec une attention particulière portée à la détection de l'overfitting.

### Étape 4 — Amélioration de l'approche de classification
Fine-tuning du modèle non-linéaire retenu (recherche d'hyperparamètres), puis interprétation du modèle grâce à plusieurs méthodes de feature importance :
- **Feature Importance native** de XGBoost (gain moyen par variable) ;
- **Permutation Importance** (dégradation de la performance quand une variable est mélangée aléatoirement) ;
- **SHAP** (Shapley values), à la fois en vision **globale** (Beeswarm plot) et **locale** (Waterfall plot sur des employés précis), afin de comprendre l'impact de chaque variable sur la prédiction, au niveau du modèle entier comme au niveau d'un individu donné.

### Étape 5 — Consolidation des résultats
Croisement et mise en perspective critique des différentes méthodes d'interprétation, afin de dégager les causes d'attrition les plus robustes (convergentes entre plusieurs méthodes) et d'identifier les éventuelles limites du modèle (faux positifs, instabilité de certaines variables).

### Étape 6 — Formalisation des résultats
Restitution des travaux sous forme d'une présentation synthétique à destination des RH, traduisant les résultats techniques (feature importance globale et locale) en messages compréhensibles par une audience non-technique, avec des recommandations actionnables.

## Méthodologie et outils

- **Langage** : Python
- **Environnement** : géré via `pyproject.toml` (gestion des dépendances et de la version Python du projet)
- **Librairies principales** :
  - `pandas`, `numpy` pour la manipulation de données ;
  - `matplotlib`, `seaborn` pour la visualisation ;
  - `scikit-learn` pour la modélisation (Dummy, régression logistique, encodage, métriques) ;
  - `xgboost` pour le modèle non-linéaire ;
  - `shap` pour l'explicabilité du modèle.

## Pourquoi cette démarche ?

Un modèle de Machine Learning performant ne suffit pas dans un contexte RH : une prédiction brute ("cet employé a X % de chances de démissionner") n'est pas exploitable telle quelle par des équipes RH si elle n'est pas accompagnée d'une explication claire des facteurs qui la sous-tendent. C'est pourquoi ce projet met volontairement l'accent sur l'**explicabilité du modèle** (feature importance globale et locale) plutôt que sur la seule performance prédictive : l'enjeu final est de transformer un modèle statistique en **leviers d'action concrets** pour la politique RH de l'entreprise.

## Livrables du projet

- `pyproject.toml` : environnement virtuel et dépendances du projet ;
- Notebooks Jupyter (`.ipynb`) : code de nettoyage, d'exploration et de modélisation, organisés par étape ;
- Présentation `.pptx` : synthèse des résultats et recommandations à destination du département RH.

## Limites et pistes d'amélioration

- Le modèle présente un certain écart entre les performances sur le jeu d'entraînement et le jeu de test, révélant un phénomène d'overfitting à surveiller et à approfondir (régularisation, enrichissement du jeu de données, validation croisée plus poussée) ;
- Certains résultats de feature importance locale illustrent des cas de faux positifs, rappelant que le modèle reste un outil d'aide à la décision et non une vérité absolue ;
- Le croisement de plusieurs méthodes d'interprétation (native, permutation, SHAP) permet de gagner en robustesse dans les conclusions, mais certaines variables restent instables d'une méthode à l'autre et mériteraient une investigation complémentaire.