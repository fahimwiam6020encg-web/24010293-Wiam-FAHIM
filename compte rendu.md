#                                    WIAM FAHIM
![IMG_5902](https://github.com/user-attachments/assets/94db96e6-f9b1-422c-9a2c-5840eed035b8)


# COMPTE RENDU DE PROJET : DÉPISTAGE DU CANCER DU POUMON


## 1. Le Contexte Métier et la Mission

### Le Problème (Business Case)
Dans le domaine de la santé, le diagnostic précoce est crucial pour le traitement du cancer du poumon. L'objectif est de développer un modèle d'apprentissage automatique capable de prédire la présence d'un cancer du poumon en se basant sur une série de symptômes et d'habitudes de vie.

* **Objectif :** Créer un outil d'aide à la décision pour évaluer le risque de cancer du poumon chez un patient.
* **L'Enjeu critique :** La matrice des coûts d'erreur est asymétrique. **Il est vital de minimiser les Faux Négatifs** (patient malade déclaré sain), car cela entraîne un retard de traitement fatal. Notre métrique d'évaluation principale sera le **Rappel (Recall / Sensibilité)** sur la classe positive (`LUNG_CANCER` = YES).

### Les Données (L'Input)
Nous utilisons le jeu de données **`survey lung cancer.csv`**.

* **X (Features) :** Âge, Genre, et 13 symptômes/habitudes (Tabagisme, Anxiété, Toux, Difficulté à avaler, etc.).
* **y (Target) :** Binaire. Variable `LUNG_CANCER` avec les valeurs **'YES'** (Cancer) ou **'NO'** (Sain).

---

## 2. Le Code Python (Laboratoire)

### A. Préparation de l'environnement
L'environnement a été configuré avec les bibliothèques standards de Data Science (`pandas`, `matplotlib`, `seaborn`, `scikit-learn`).

### B. Chargement et Nettoyage Initial
Un nettoyage essentiel a été effectué pour garantir la qualité de l'analyse :

* **Détection et Suppression des Duplicatas :** 33 enregistrements dupliqués ont été identifiés et supprimés du jeu de données initial (passant de 309 à 276 observations uniques).
    * *Justification :* Les duplicatas faussent les statistiques descriptives et entraînent une sur-estimation des performances du modèle.

---

## 3. L'Exploration de Données (EDA)

### A. Distribution du Genre
La distribution des genres dans le jeu de données nettoyé est la suivante :

| Genre | Nombre d'individus | Proportion |
| :---: | :---: | :---: |
| **M** | 142 | 51.45% |
| **F** | 134 | 48.55% |
<img width="571" height="455" alt="téléchargement (4)" src="https://github.com/user-attachments/assets/25dedfe5-b48d-43d4-923f-74c3ee2b66f0" />

### B. Analyse de la Cible (`LUNG_CANCER`)
Le jeu de données présente un **déséquilibre de classe**, avec une majorit<img width="571" height="455" alt="téléchargement (3)" src="https://github.com/user-attachments/assets/8c316e3b-43b8-4a56-a8dc-49cbc11b6b46" />
é de cas de cancer ('YES'). Cette observation confirme la nécessité d'utiliser des métriques robustes (Recall) plutôt que l'Accuracy.

### C. Analyse des Fréquences de Symptômes par Genre
Les données binaires ont été encodées en `0`/`1` pour calculer la fréquence moyenne des symptômes pour chaque genre, permettant d'identifier les facteurs de risque spécifiques.

---

## 4. Le Pré-Traitement et Feature Engineering

### A. Encodage des Variables
Toutes les variables binaires, y compris la variable cible `LUNG_CANCER`, ont été converties en format numérique (`0` et `1`) :

* **Variables (symptômes) :** `2` / `YES` → **1** (Présent) ; `1` / `NO` → **0** (Absent).
* **Cible (`LUNG_CANCER`)** : `YES` → **1** (Cancer) ; `NO` → **0** (Sain).
* **Genre (`GENDER`)** : Doit être encodé (ex: `M`=0, `F`=1 ou One-Hot Encoding).

### B. Scaling des Données
La variable `AGE` est sur une échelle différente (numérique) des variables binaires (0/1). Un **Standard Scaler** devra être appliqué pour normaliser cette colonne afin d'éviter qu'elle ne domine le calcul de distance dans certains algorithmes.

---

## 5. La Modélisation et l'Évaluation<img width="696" height="504" alt="téléchargement (5)" src="https://github.com/user-attachments/assets/26e42f15-df77-4541-a594-52bce15be776" />


### A. Choix des Modèles (Pipeline d'Analyse)
Les modèles de classification suivants sont recommandés pour ce problème :
1.  **Random Forest Classifier**
2.  **XGBoost / Gradient Boosting**
3.  **Support Vector Machine (SVM)** ou **Régression Logistique**

Un processus de **Cross-Validation (k-fold)** est nécessaire pour valider la généralisation des performances.

### B. Évaluation des Résultats : La Métrique Critique

La métrique d'évaluation est strictement axée sur la minimisation des risques vitaux. 

1.  **Le Rappel (Recall / Sensibilité) - La "Puissance du filet"**
    $$Recall = TP / (TP + FN)$$
    * **Objectif :** Maximiser le Recall sur la classe positive ('YES'). Cela mesure la capacité du modèle à ne laisser passer aucun cas réel de cancer.

2.  **La Précision (Precision) - La "Qualité de l'alarme"**
    $$Precision = TP / (TP + FP)$$
    * La précision est secondaire, mais doit rester acceptable pour limiter les examens inutiles (Faux Positifs).

---
<img width="960" height="528" alt="téléchargement (6)" src="https://github.com/user-attachments/assets/e84a5fc3-4686-4ab5-9f57-ce8d33aa3511" />

## 6. Conclusion et Prochaines Étapes

**Conclusion de l'Analyse Préliminaire :**
L'analyse a identifié l'enjeu critique (maximisation du Recall) et a préparé un jeu de données nettoyé et encodé pour la modélisation.

