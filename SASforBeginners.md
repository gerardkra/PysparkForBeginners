### 🔹 Qu’est-ce qu’un programme SAS ?

Un programme **SAS** (Statistical Analysis System) suit un **flux séquentiel précis** :
il **charge des données**, **les analyse** et **affiche les résultats**.

Chaque programme SAS est composé de **3 grandes étapes** :

1. **DATA Step** → création ou lecture du jeu de données.
2. **PROC Step** → utilisation des **procédures SAS** pour analyser les données.
3. **OUTPUT Step** → affichage ou exportation des résultats.

> 💡 Chaque étape se termine obligatoirement par un **`RUN;`**, qui exécute la partie précédente du code.

---

## ⚙️ Structure d’un programme SAS

```
DATA Step       →       PROC Step    →     OUTPUT Step
(lecture / création)    (analyse)          (affichage)
```

---

## 🏗️ 1. DATA Step — Créer / Lire un jeu de données

### 🎯 Objectif

Charger les données en mémoire SAS, définir les variables (colonnes) et enregistrer les observations (lignes).
On peut aussi créer de nouvelles variables et leur attribuer des étiquettes.

### 🧩 Syntaxe

```sas
DATA data_set_name;       /* Nom du jeu de données */
INPUT var1 var2 var3;     /* Définition des variables */
new_var = expression;     /* Création de variables dérivées */
LABEL var = 'Label';      /* Ajout de labels lisibles */
DATALINES;                /* Données brutes à saisir */
...                       /* Données */
RUN;                      /* Exécution de l’étape */
```

### 💡 Exemple

```sas
DATA TEMP;
INPUT ID $ NAME $ SALARY DEPARTMENT $;
comm = SALARY * 0.25;
LABEL ID = 'Employee ID' comm = 'Commission';
DATALINES;
1 Rick 623.3 IT
2 Dan 515.2 Operations
3 Michelle 611 IT
4 Ryan 729 HR
5 Gary 843.25 Finance
6 Nina 578 IT
7 Simon 632.8 Operations
8 Guru 722.5 Finance
;
RUN;
```

📊 → Ce code crée le jeu de données **TEMP** avec 8 employés et ajoute une variable **comm** (commission).

---

## 🔍 2. PROC Step — Analyse des données

### 🎯 Objectif

Appeler une **procédure SAS (PROC)** intégrée pour exécuter une analyse statistique ou une transformation.

### 🧩 Syntaxe

```sas
PROC procedure_name options;
RUN;
```

### 💡 Exemple

```sas
PROC MEANS;
RUN;
```

📈 → Calcule la moyenne des variables numériques du dernier jeu de données utilisé (**TEMP** ici).

---

## 📤 3. OUTPUT Step — Afficher ou filtrer les résultats

### 🎯 Objectif

Afficher les données d’un jeu de données ou une sélection conditionnelle via **PROC PRINT**.

### 🧩 Syntaxe

```sas
PROC PRINT DATA = dataset_name;
WHERE condition;
RUN;
```

### 💡 Exemple

```sas
PROC PRINT DATA = TEMP;
WHERE SALARY > 700;
RUN;
```

📋 → Affiche uniquement les employés dont le salaire dépasse **700**.

---

## 🧱 Programme SAS complet

```sas
DATA TEMP;
INPUT ID $ NAME $ SALARY DEPARTMENT $;
comm = SALARY * 0.25;
LABEL ID = 'Employee ID' comm = 'Commission';
DATALINES;
1 Rick 623.3 IT
2 Dan 515.2 Operations
3 Michelle 611 IT
4 Ryan 729 HR
5 Gary 843.25 Finance
6 Nina 578 IT
7 Simon 632.8 Operations
8 Guru 722.5 Finance
;
RUN;

PROC MEANS;
RUN;

PROC PRINT DATA = TEMP;
WHERE SALARY > 700;
RUN;
```

---

## 🧾 SAS Cheatsheet — Étapes & Commandes essentielles

| Étape     | Objectif                                | Commande clé                        | Exemple                                     |
| --------- | --------------------------------------- | ----------------------------------- | ------------------------------------------- |
| **DATA**  | Créer ou lire un jeu de données         | `DATA`, `INPUT`, `DATALINES`, `RUN` | `DATA TEMP; INPUT ID $ NAME $ SALARY; RUN;` |
| **PROC**  | Exécuter une analyse ou une procédure   | `PROC procedure_name; RUN;`         | `PROC MEANS; RUN;`                          |
| **PRINT** | Afficher les données                    | `PROC PRINT DATA=...;`              | `PROC PRINT DATA=TEMP; RUN;`                |
| **WHERE** | Filtrer les observations                | `WHERE condition;`                  | `WHERE SALARY > 700;`                       |
| **LABEL** | Ajouter des noms lisibles aux variables | `LABEL var='label';`                | `LABEL ID='Employee ID';`                   |
| **RUN**   | Exécuter l’étape courante               | `RUN;`                              | (obligatoire à la fin de chaque bloc)       |

---

## 🧭 Résumé visuel du flux SAS

```
        ┌───────────────────────┐
        │      DATA Step        │
        │ Lire / créer données  │
        └─────────┬─────────────┘
                  │
                  ▼
        ┌───────────────────────┐
        │      PROC Step        │
        │ Analyser les données  │
        └─────────┬─────────────┘
                  │
                  ▼
        ┌───────────────────────┐
        │     OUTPUT Step       │
        │ Afficher les résultats│
        └───────────────────────┘
```
