============================================================
TP NLP – PRÉDICTION MULTI-LABEL D’ABSTRACTS MÉDICAUX
LSTM / BiLSTM + Word2Vec
============================================================

# DESCRIPTION DU TP
---------------------
Ce projet correspond à un TP de Traitement Automatique du Langage Naturel (NLP).
L’objectif est de réaliser une classification multi-label d’articles médicaux
à partir de leurs abstracts, en utilisant des réseaux de neurones LSTM / BiLSTM
et des embeddings Word2Vec entraînés sur le corpus.

Chaque abstract peut appartenir à plusieurs catégories médicales simultanément.


# JEU DE DONNÉES
--------------
Fichier utilisé :
    PubMed-multi-label-dataset.csv

Texte :
- Colonne utilisée : Abstract / abstractText
- Contenu : résumés (abstracts) d’articles médicaux PubMed

Labels (multi-label) :
Chaque article est annoté avec 14 labels binaires (0/1) :

A : Anatomy
B : Organisms
C : Diseases
D : Chemicals and Drugs
E : Analytical, Diagnostic and Therapeutic Techniques
F : Psychiatry and Psychology
G : Phenomena and Processes
H : Disciplines and Occupations
I : Anthropology, Education, Sociology
J : Technology, Industry, Agriculture
L : Information Science
M : Named Groups
N : Health Care
Z : Geographicals

Un article peut appartenir à plusieurs catégories à la fois.


# PIPELINE DU TP
------------------
Abstracts médicaux
        ↓
Prétraitement du texte
        ↓
Word2Vec entraîné sur le corpus
        ↓
Embedding Layer initialisée avec Word2Vec
        ↓
LSTM / BiLSTM
        ↓
Sortie sigmoid (14 labels)
        ↓
Évaluation avec Macro F1-score


# MÉTHODOLOGIE
------------
1) PRÉPARATION DES DONNÉES
- Utilisation exclusive de la colonne Abstract
- Labels encodés sous forme de vecteurs binaires (n_samples, 14)
- Séparation train / test (sans stratification, non applicable en multi-label)

2) WORD2VEC
- Word2Vec entraîné from scratch sur les abstracts
- Embeddings spécifiques au domaine médical
- Dimension des vecteurs : 100
- Aucun modèle pré-entraîné (ex. Google Word2Vec) n’est utilisé

3) MODÈLES
- LSTM simple
- BiLSTM simple (merge des deux directions par concaténation)

Architecture BiLSTM :
- Embedding (Word2Vec)
- BiLSTM (100 unités par direction, merge_mode='concat')
- Dropout (0.5)
- Dense(14, activation='sigmoid')

4) APPRENTISSAGE MULTI-LABEL
- Fonction de perte : binary_crossentropy
- Activation de sortie : sigmoid
- Optimiseur : Adam

5) ÉVALUATION
- Métrique principale : Macro F1-score
- Évaluation sur le jeu de test
- Vérification du surapprentissage via comparaison Train / Test


# RÉSULTATS
---------
Exemple de résultats obtenus avec le modèle BiLSTM :

- Macro F1 (Train) ≈ 0.93
- Macro F1 (Test)  ≈ 0.89

L’écart limité entre les performances d’entraînement et de test indique
une bonne généralisation et l’absence de surapprentissage significatif.


# JUSTIFICATION DES CHOIX
----------------------
- Word2Vec entraîné sur le corpus : capture du vocabulaire médical spécifique
- BiLSTM : prise en compte du contexte gauche et droit
- Sigmoid + binary cross-entropy : indispensable pour le multi-label
- Macro F1-score : adapté aux classes déséquilibrées


TECHNOLOGIES UTILISÉES
---------------------
- Python
- Pandas, NumPy
- Gensim (Word2Vec)
- Scikit-learn
- TensorFlow / Keras



AUTEUR
------
Nom : [Niama CHIBANI]

Année : 2025–2026


REMARQUE FINALE
---------------
Ce projet illustre une approche complète de classification multi-label
en NLP, combinant des représentations distribuées (Word2Vec) et des
modèles séquentiels (LSTM / BiLSTM) sur des données médicales réelles.
