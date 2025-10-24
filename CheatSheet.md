### 🔹 Qu’est-ce qu’un RDD ?

**RDD** (Resilient Distributed Dataset) est la structure de base de Spark.
C’est un **ensemble distribué d’éléments** répartis sur plusieurs nœuds du cluster, permettant un **traitement parallèle**.

Caractéristiques :

* **Immuable** → une fois créé, on ne peut pas le modifier.
* **Tolérant aux pannes** → il se régénère automatiquement en cas d’erreur.
* **Distribué** → les calculs se font en parallèle sur plusieurs machines.

---

### 🔹 Types d’opérations sur les RDD

Il existe **deux grandes familles** d’opérations :

1. **Transformations** → créent un **nouvel RDD** à partir d’un existant.
   ⚙️ *Exemples :* `map()`, `filter()`, `groupBy()`, `join()`

2. **Actions** → déclenchent l’exécution des transformations et **renvoient un résultat** au driver.
   📤 *Exemples :* `count()`, `collect()`, `reduce()`, `foreach()`

> 💡 Les transformations sont **paresseuses** (lazy) : elles ne s’exécutent que lorsqu’une action est appelée.

---

## ⚡ PySpark RDD Cheatsheet

### 🏗️ Création d’un RDD

```python
from pyspark import SparkContext
sc = SparkContext("local", "app")
words = sc.parallelize([
   "scala", "java", "hadoop", "spark", "akka",
   "spark vs hadoop", "pyspark", "pyspark and spark"
])
```
``sc.parallelize()``
crée un RDD à partir d'une collection Python locale (comme une liste).

---

### 🔄 TRANSFORMATIONS (créent un nouveau RDD)

| Opération        | Description                                     | Exemple                               | Résultat                                |
| ---------------- | ----------------------------------------------- | ------------------------------------- | --------------------------------------- |
| `map(f)`         | Applique une fonction à chaque élément          | `rdd.map(lambda x: (x, 1))`           | `[("scala",1), ("java",1), ...]`        |
| `filter(f)`      | Garde les éléments qui respectent une condition | `rdd.filter(lambda x: 'spark' in x)`  | `["spark", "pyspark", ...]`             |
| `groupByKey()`   | Regroupe les valeurs par clé                    | `rdd.groupByKey()`                    | `{clé: [valeurs]}`                      |
| `reduceByKey(f)` | Combine les valeurs ayant la même clé           | `rdd.reduceByKey(lambda a,b: a+b)`    | `[(clé, somme)]`                        |
| `join(rdd2)`     | Joint deux RDD par clé                          | `x.join(y)`                           | `[("spark", (1,2)), ("hadoop", (4,5))]` |
| `flatMap(f)`     | Comme `map`, mais “aplati” les résultats        | `rdd.flatMap(lambda x: x.split(" "))` | liste d’éléments individuels            |

---

### 🚀 ACTIONS (déclenchent l’exécution et renvoient un résultat)

| Opération    | Description                                            | Exemple              | Résultat                      |
| ------------ | ------------------------------------------------------ | -------------------- | ----------------------------- |
| `count()`    | Nombre d’éléments                                      | `rdd.count()`        | `8`                           |
| `collect()`  | Retourne tous les éléments du RDD                      | `rdd.collect()`      | `["scala", "java", ...]`      |
| `foreach(f)` | Applique une fonction à chaque élément (pas de retour) | `rdd.foreach(print)` | affiche chaque mot            |
| `reduce(f)`  | Combine les éléments via une fonction binaire          | `nums.reduce(add)`   | `15`                          |
| `first()`    | Premier élément du RDD                                 | `rdd.first()`        | `"scala"`                     |
| `take(n)`    | Retourne les `n` premiers éléments                     | `rdd.take(3)`        | `["scala", "java", "hadoop"]` |

---

### 💾 PERSISTENCE

| Opération   | Description                                                     | Exemple         | Résultat         |
| ----------- | --------------------------------------------------------------- | --------------- | ---------------- |
| `cache()`   | Stocke le RDD en mémoire (niveau par défaut)                    | `rdd.cache()`   | RDD mis en cache |
| `persist()` | Stocke avec un niveau choisi (`MEMORY_ONLY`, `DISK_ONLY`, etc.) | `rdd.persist()` | RDD persistant   |
| `is_cached` | Vérifie si le RDD est en cache                                  | `rdd.is_cached` | `True / False`   |

---

### 🧩 Commandes Spark

| Action                     | Commande                                 |
| -------------------------- | ---------------------------------------- |
| Exécuter un script PySpark | `$SPARK_HOME/bin/spark-submit script.py` |

---

### 💡 Exemple complet

```python
from pyspark import SparkContext
from operator import add

sc = SparkContext("local", "Example App")
words = sc.parallelize(["spark", "hadoop", "pyspark", "java"])

# Transformation
filtered = words.filter(lambda x: 'spark' in x)
mapped = filtered.map(lambda x: (x, 1))

# Action
print("Filtered:", filtered.collect())
print("Mapped:", mapped.collect())
print("Count:", mapped.count())

nums = sc.parallelize([1, 2, 3, 4, 5])
print("Sum:", nums.reduce(add))
```
