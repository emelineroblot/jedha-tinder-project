# Tinder - Analyse de Speed Dating 💘

## 🎯 Vue d'ensemble du projet

Ce projet analyse les données d'une **expérience de speed dating** menée pour Tinder afin de comprendre ce qui pousse les utilisateurs à s'intéresser les uns aux autres et à accepter un second rendez-vous. L'objectif est d'identifier les facteurs clés du matching pour améliorer l'algorithme de recommandation de Tinder.

**Approche** : Analyse exploratoire de données (EDA) avec statistiques descriptives et visualisations
**Dataset** : 8,378 rencontres de speed dating (2002-2004)
**Résultat clé** : **L'attractivité physique explique 48.7%** des décisions de match (bien que les utilisateurs déclarent lui accorder seulement 20% d'importance)

---

## 💼 Contexte business

### À propos de Tinder
**Tinder** est l'application de rencontres et de réseautage géosocial leader mondial :
- 📱 Lancement en 2012 par Sean Rad lors d'un hackathon
- 💑 **65 milliards de matchs** enregistrés en 2021
- 🌍 Présente dans plus de 190 pays
- 🔥 Fonctionnement : "swipe right" (j'aime) / "swipe left" (je n'aime pas)

### Problématique
L'équipe marketing de Tinder constate une **baisse du nombre de matchs** et cherche à comprendre :
- ✅ Qu'est-ce qui suscite l'intérêt mutuel entre utilisateurs ?
- ✅ Quels attributs sont réellement importants dans le choix d'un partenaire ?
- ✅ Y a-t-il un décalage entre préférences déclarées et comportement réel ?
- ✅ Comment optimiser le profil utilisateur pour maximiser les matchs ?

### Solution
Mener une **expérience contrôlée de speed dating** avec collecte de données exhaustives :
- Évaluations des partenaires sur 6 critères
- Questionnaires démographiques et psychologiques
- Décisions de match (oui/non pour un second rendez-vous)
- Analyse statistique pour identifier les patterns

---

## 📊 Jeu de données

### Structure
**Source** : Expériences de speed dating (2002-2004)
**Format** : CSV (`Speed+Dating+Data.csv`)
**Taille** : 8,378 rencontres (lignes) × 195 variables (colonnes)

### Processus de collecte
Au cours des événements de speed dating :
1. **4 minutes de rencontre** avec chaque participant du sexe opposé
2. **Évaluation sur 6 critères** : Attractivité, Sincérité, Intelligence, Fun, Ambition, Intérêts partagés
3. **Décision binaire** : Accepter ou refuser un second rendez-vous
4. **Questionnaires** à différents moments :
   - Avant l'événement : préférences, auto-évaluation
   - Pendant : évaluations des partenaires
   - Après : feedback sur l'expérience

### Variables clés

#### Variables de décision
- `dec` : Décision du participant (0 = non, 1 = oui)
- `dec_o` : Décision du partenaire (0 = non, 1 = oui)
- `match` : Match mutuel (1 si les deux disent oui, 0 sinon)

#### Évaluations données (notes sur 10)
- `attr` : Attractivité physique
- `sinc` : Sincérité
- `intel` : Intelligence
- `fun` : Sens de l'humour / Fun
- `amb` : Ambition
- `shar` : Intérêts partagés

#### Évaluations reçues (notes sur 10)
- `attr_o`, `sinc_o`, `intel_o`, `fun_o`, `amb_o`, `shar_o`

#### Préférences déclarées (avant l'événement)
- `attr1_1`, `sinc1_1`, `intel1_1`, `fun1_1`, `amb1_1`, `shar1_1`
- Distribution en % : Total = 100%

#### Données démographiques
- `gender` : Genre (0 = femme, 1 = homme)
- `age` : Âge du participant
- `race` : Origine ethnique
- `samerace` : Même origine que le partenaire (0/1)
- `field_cd` : Domaine d'études
- `income` : Revenu

#### Contexte de la rencontre
- `wave` : Numéro de session de speed dating (1-21)
- `round` : Nombre total de participants dans la session
- `order` : Position du rendez-vous dans la soirée (1er, 2ème, ...)
- `position` : Position physique dans la salle

### Distribution des données

**Statistiques globales** :
- **Taux de match** : 16.47%
- **Taux de "oui" moyen** : 42% (hommes et femmes confondus)
- **Nombre moyen de rencontres par soirée** : 16-18 personnes
- **Âge moyen** : 26 ans (écart-type : 4 ans)

**Déséquilibres** :
- 50.06% d'hommes, 49.94% de femmes (équilibré)
- 83 colonnes avec >30% de valeurs manquantes (données follow-up après événement)
- Dataset nettoyé : 79 colonnes conservées (focus sur Time 1)

---

## 🛠️ Stack technique

### Bibliothèques Python
```python
# Data Processing
pandas, numpy

# Visualisation interactive
plotly.express
plotly.graph_objects

# Analyse statistique
scipy.stats (si utilisé)
```

### Environnement
- **Python** : 3.8+
- **Jupyter Notebook** : Pour l'analyse interactive
- **Plotly** : Visualisations interactives (graphiques, heatmaps, radar charts)

---

## 📁 Structure du projet

```
tinder_project/
├── README.md                          # Documentation du projet
├── Projet_Tinder.txt                  # Brief du projet
├── 01-Speed_Dating.ipynb              # Notebook d'analyse principal
├── Speed+Dating+Data.csv              # Dataset brut (8,378 lignes, 195 colonnes)
├── SpeedDating_clean.csv              # Dataset nettoyé (79 colonnes)
└── Speed+Dating+Data+Key.doc          # Documentation du dataset
```

---

## 🔍 Méthodologie d'analyse

### Étape 1 : Nettoyage des données
✅ **Identification des valeurs manquantes** : 83 colonnes avec >30% de valeurs manquantes
✅ **Catégorisation des colonnes** :
- Complètes (0% manquant) : 13 colonnes
- Utilisables (<30% manquant) : 99 colonnes
- Problématiques (≥30% manquant) : 83 colonnes

✅ **Stratégie de sélection** :
- Conservation des colonnes du **Time 1** (avant/pendant l'événement)
- Élimination des colonnes du **Time 2 et 3** (follow-up avec 70-93% de données manquantes)
- Focus sur les 79 colonnes essentielles

### Étape 2 : Analyse descriptive
✅ Statistiques globales (taux de match, distributions)
✅ Comparaisons hommes vs femmes
✅ Corrélations entre critères et décisions
✅ Analyse de l'impact de l'ordre des rencontres

### Étape 3 : Visualisations
✅ Distributions (âges, notes, décisions)
✅ Comparaisons (match vs non-match)
✅ Matrices de corrélation (heatmaps)
✅ Évolutions temporelles (effet fatigue)
✅ Analyses par session (radar charts)

### Étape 4 : Insights et recommandations
✅ Identification des facteurs clés de matching
✅ Écarts préférences déclarées vs comportement réel
✅ Recommandations pour l'algorithme Tinder

---

## 📈 Résultats & Insights

### 1. L'attractivité domine (mais les utilisateurs le nient) 🔥

**Résultat clé** :
- **Corrélation attractivité-décision** : **0.487** (la plus élevée des 6 critères)
- Les utilisateurs **déclarent** accorder 22.5% d'importance à l'attractivité
- Mais l'attractivité **explique ~49%** de leur décision réelle
- **Écart de 2.4 points** dans les notes d'attractivité entre matchs (7.2/10) et non-matchs (4.8/10)

**Hiérarchie réelle des critères** (corrélation avec décision) :
| Rang | Critère | Corrélation | Importance déclarée |
|------|---------|-------------|---------------------|
| 1 | **Attractivité** | **0.487** | 22.5% |
| 2 | **Fun** | **0.414** | 17.5% |
| 3 | **Intérêts partagés** | **0.401** | 11.8% |
| 4 | **Intelligence** | 0.217 | 20.3% |
| 5 | **Sincérité** | 0.210 | 17.4% |
| 6 | **Ambition** | 0.184 | 10.7% |

**Interprétation** :
- ✅ Les utilisateurs **sous-estiment massivement** l'impact de l'attractivité physique
- ✅ L'**intelligence** est surestimée : importance déclarée 20.3% vs corrélation réelle 0.217
- ✅ Les **intérêts partagés** sont sous-estimés : importance déclarée 11.8% vs corrélation réelle 0.401

### 2. Différences hommes vs femmes 👨👩

**Taux de décision positive** :
- **Hommes** : 47.0% de "oui"
- **Femmes** : 37.0% de "oui"
- **Ratio** : Les hommes disent oui **1.27x plus souvent** que les femmes

**Notes moyennes données** :
| Critère | Femmes | Hommes | Écart |
|---------|--------|--------|-------|
| **Attractivité** | 5.92 | 6.46 | **+0.54** (hommes plus généreux) |
| **Sincérité** | 7.10 | 7.25 | +0.15 |
| **Intelligence** | 7.45 | 7.29 | -0.16 |
| **Fun** | 6.28 | 6.52 | +0.24 |
| **Ambition** | 6.95 | 6.60 | **-0.35** (femmes valorisent plus) |
| **Intérêts partagés** | 5.41 | 5.54 | +0.13 |

**Insights** :
- ✅ Les **femmes sont plus sélectives** (10 points de différence dans les taux de "oui")
- ✅ Les **hommes accordent plus d'importance à l'attractivité** (notes +0.54 plus élevées)
- ✅ Les **femmes valorisent davantage l'ambition** (notes -0.35 plus sévères)

### 3. L'effet fatigue de décision 😴

**Impact de la position dans la soirée** :
| Position | Taux de "oui" | Évolution |
|----------|---------------|-----------|
| **1er rendez-vous** | 48% | Baseline |
| **Rendez-vous 8-12** | 42-44% | -4 à -6 points |
| **Dernier rendez-vous** | 38-40% | **-8 à -10 points** |

**Impact de la taille du groupe** :
- Sessions avec **10-14 participants** : Taux de match **25-31%**
- Sessions avec **18-22 participants** : Taux de match **14-18%**
- **Corrélation** : -0.15 à -0.25 (légèrement négative)

**Variation entre sessions** :
- Meilleure session (Wave 1) : **31.0%** de matchs
- Pire session (Wave 18) : **8.3%** de matchs
- **Facteur multiplicateur** : **3.7x** entre meilleure et pire session

**Explication** :
- ✅ **Fatigue cognitive** : Plus la soirée avance, plus les standards augmentent
- ✅ **Effet de comparaison** : Les derniers partenaires sont comparés à tous les précédents
- ✅ **Paradoxe du choix** : Trop d'options réduisent la satisfaction et les décisions positives

### 4. Comparaison : Match vs Non-Match 💚❌

**Notes moyennes dans les cas de match vs non-match** :
| Critère | Non-Match | Match | Écart |
|---------|-----------|-------|-------|
| **Attractivité** | 5.80 | **8.20** | **+2.40** 🔥 |
| **Fun** | 6.10 | **8.30** | **+2.20** |
| **Intérêts partagés** | 5.00 | **7.80** | **+2.80** 🔥 |
| **Intelligence** | 7.20 | **8.10** | +0.90 |
| **Sincérité** | 7.00 | **7.90** | +0.90 |
| **Ambition** | 6.70 | **7.50** | +0.80 |

**Insight clé** :
- ✅ Les 3 critères avec le **plus grand écart** sont : **Intérêts partagés (+2.80)**, **Attractivité (+2.40)**, **Fun (+2.20)**
- ✅ Ces critères sont les **meilleurs prédicteurs** de match mutuel
- ✅ L'**ambition** a le plus faible impact (+0.80)

### 5. Auto-évaluation vs Réalité 🤔

**Précision de l'auto-évaluation** :
- **Auto-évaluation moyenne attractivité** : 7.0/10
- **Note moyenne reçue** : 6.5/10
- **Écart** : +0.5 point (surestimation légère)
- **Corrélation** auto-évaluation / notes reçues : **0.25-0.30** (faible)

**Prédiction du nombre de matchs** :
- **Prédiction moyenne** : 4-5 matchs par soirée
- **Réalité** : 2-3 matchs par soirée
- **Surestimation** : **60-80%**

**Interprétation** :
- ✅ Les participants **surestiment leur attractivité** et leur succès
- ✅ Faible capacité à prédire leur **valeur perçue** sur le marché des rencontres
- ✅ Biais d'**optimisme** généralisé

### 6. L'origine ethnique commune 🌍

**Impact sur les matchs** :
- **Même origine ethnique** : Présente dans **40%** des rencontres
- **Corrélation** avec décision : **0.08-0.12** (faible mais positive)
- **Intérêts partagés** : Corrélation **0.40** (3-4x plus importante)

**Conclusion** :
- ✅ Les **intérêts communs** sont **nettement plus importants** que l'origine ethnique commune
- ✅ L'origine ethnique a un impact **marginal** comparé aux autres critères

---

## 💡 Recommandations pour Tinder

### 1. Optimisation de l'algorithme de matching

**Pondérations suggérées** (basées sur les corrélations réelles) :
```
Attractivité :         48% (0.487)
Fun :                  41% (0.414)
Intérêts partagés :    40% (0.401)
Intelligence :         22% (0.217)
Sincérité :            21% (0.210)
Ambition :             18% (0.184)
```

**Actionnable** :
- ✅ Prioriser les profils avec **compatibilité photo** élevée (attractivité)
- ✅ Mettre en avant les **intérêts communs** dans les suggestions
- ✅ Valoriser les profils **fun** (humor, spontanéité dans la bio)

### 2. Amélioration du profil utilisateur

**Conseils aux utilisateurs** :
1. **Photos de qualité** : Impact majeur (+49% de corrélation avec match)
2. **Bio fun et légère** : Humour > sérieux professionnel
3. **Mise en avant des hobbies** : Intérêts partagés = +40% de corrélation
4. **Authenticité** : Sincérité et intelligence moins critiques mais importants pour relation durable

**À éviter** :
- ❌ Surestimer l'importance de l'ambition/carrière (faible corrélation 0.18)
- ❌ Profils trop sérieux/formels (fun corrélation 0.41)
- ❌ Absence de centres d'intérêt dans la bio

### 3. Gestion de l'expérience utilisateur

**Limiter la fatigue de décision** :
- ✅ **Limiter à 12-15 profils par session** de swipe (après, baisse de 10% du taux de match)
- ✅ **Espacer les sessions** : éviter le marathon de swipe
- ✅ **Notifications stratégiques** : relancer en début de session (taux de oui +10%)

**Optimisation temporelle** :
- ✅ Montrer les **meilleurs matchs en premier** (effet de primauté)
- ✅ Éviter de montrer 20+ profils d'affilée (effet de comparaison négatif)

### 4. Fonctionnalités à développer

**Basées sur les insights** :
1. **Score de compatibilité d'intérêts** : Calculer % d'intérêts communs
2. **Filtre "Fun"** : Identifier profils avec bio humoristique/légère
3. **Limite de swipe quotidienne** : 15-20 max pour maintenir la qualité des décisions
4. **Feedback post-match** : Améliorer prédiction des préférences réelles vs déclarées

---

## 🚀 Installation & Utilisation

### Prérequis
```bash
# Python 3.8 ou supérieur
python --version
```

### Étape 1 : Cloner le repository
```bash
git clone <repository-url>
cd tinder_project
```

### Étape 2 : Installer les dépendances
```bash
# Installer les packages requis
pip install pandas numpy plotly jupyter

# Vérifier l'installation
python -c "import plotly; print(plotly.__version__)"
```

### Étape 3 : Lancer le notebook
```bash
# Démarrer Jupyter Notebook
jupyter notebook 01-Speed_Dating.ipynb
```

### Étape 4 : Exécuter l'analyse
Exécuter toutes les cellules séquentiellement pour :
1. Nettoyer les données (79 colonnes conservées)
2. Générer les statistiques descriptives
3. Créer les visualisations interactives
4. Afficher les insights clés

---

## 📊 Visualisations clés

### 1. Distribution des décisions par genre
**Type** : Bar chart
**Insight** : Hommes 47% de "oui" vs Femmes 37%

### 2. Préférences déclarées vs Comportement réel
**Type** : Bar + Line chart (axes doubles)
**Insight** : Attractivité sous-estimée, Intelligence surestimée

### 3. Impact de l'ordre dans la soirée
**Type** : Line chart
**Insight** : Baisse de 10 points entre 1er et dernier rendez-vous

### 4. Matrice de corrélation des critères
**Type** : Heatmap
**Insight** : Attractivité, Fun, Intérêts partagés fortement corrélés entre eux

### 5. Comparaison Match vs Non-Match
**Type** : Grouped bar chart
**Insight** : Écarts de +2.4 à +2.8 points sur critères clés

### 6. Profil des sessions succès vs échec
**Type** : Radar chart
**Insight** : Sessions <15 participants ont 2x plus de matchs
