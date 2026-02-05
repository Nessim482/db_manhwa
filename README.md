# Analyse Éditoriale des Manhwas
# Classement qualité pour plateforme de streaming

Ce projet vise à produire un classement éditorial objectif des manhwas pour une plateforme de streaming. L'objectif est de prioriser les œuvres de haute qualité dès leur publication, en s'appuyant uniquement sur des métriques existantes (score, rang, popularité, favoris), sans attendre les données d'engagement utilisateurs qui nécessitent plusieurs mois de collecte.

# Objectif Métier

Pour une plateforme de streaming de manhwas :
- Problématique : Comment identifier les "bijoux" éditoriaux dès le lancement ?
- Contrainte : Pas de données utilisateurs disponibles au lancement
- Solution : Classement multi-critères basé sur métriques objectives
- Résultat : 90% de corrélation entre score seul et classement complet

# Pipeline Complet du Notebook

# 1. Chargement et Inspection Initiale
J'importe pandas et matplotlib
Je charge le fichier manhwa.csv (dataset brut ~1300 manhwas)
J'affiche les 5 premières lignes + infos colonnes pour comprendre la structure

# 2. Nettoyage Approfondi des Données
Je supprime les colonnes inutiles : synopsis, type, publish_time, chapters, etc.
Je vire les lignes sans score (pas analysables)
J'exclus les genres sensibles : Erotica, Ecchi, Unknown
ATTENTION : Le fichier original est écrasé ! (je le signale clairement)

# 3. Conversion Numérique Robuste
Les colonnes rank, popularity, favorites arrivent en STRING avec des "#", "," et "Unknown"
Je les nettoie : str.replace('#', '').str.replace(',', '').astype(float)
Résultat : Dataset propre 1298×8, prêt pour l'analyse

# 4. Analyse Exploratoire (EDA)
Score moyen : 7.08/10 (médiane 7.03)
Corrélation score/popularité : -0.58 (intéressant !)
Graphique Top10 horizontal pour visualiser les leaders

# 5. Classement Éditorial Principal
Mon algo multi-critères (dans l'ordre) :
1. score DESC (poids principal)
2. rank ASC (plus bas = mieux)  
3. popularity ASC (moins populaire = mieux)
4. favorites DESC (bonus qualité)

Export : manhwa_ranking.csv avec colonne "classement" numérotée

# 6. Test de Stabilité (H1)
Je teste : "score seul = multi-critères ?"
→ Export manhwa_ranking_score_only.csv
→ Comparaison Top10 : 9/10 identiques = 90% stabilité
→ H1 VALIDÉE

# Résultats Clés

Top 3 confirmé :
1. Solo Leveling (8.68, rank #7, 40k favoris)
2. The Horizon (8.67, rank #187)  
3. Wind Breaker (8.58, Publishing)

Corrélation score/popularité = -0.58 → Les "blockbusters" populaires scorent moins bien

# Limites Identifiées et Solutions

Problème : Écrasement CSV original → Impact : Perte traçabilité → Solution : Signalé en GROS + copie recommandée
Problème : KeyError suppression colonnes → Impact : Erreur si colonne déjà supprimée → Solution : errors='ignore'
Problème : Genres multi-valeurs → Impact : Analyse biaisée → Solution : split(), explode()
Problème : Colonnes texte → numérique → Impact : Tri impossible → Solution : Nettoyage systématique

# Prérequis et Installation

pip install pandas matplotlib jupyter

Fichiers nécessaires : manhwa.csv dans le même dossier

Fichiers générés :
manhwa.csv (nettoyé)  
manhwa_ranking.csv
manhwa_ranking_score_only.csv

# Comment J'exécute le Projet

1. Copie de sécurité du CSV original → OBLIGATOIRE
2. Jupyter Notebook → ouvrir mon fichier
3. Exécuter TOUTES les cellules dans l'ordre (1→fin)
4. Vérifier les 3 CSV générés dans le dossier
5. Analyser les graphiques Top10

# Mes Bonnes Pratiques

À faire absolument :
- Copie CSV original avant exécution
- Exécution séquentielle des cellules
- Vérification des types de données après nettoyage

À éviter :
- Sauter le nettoyage → crash sur tri numérique
- Modifier l'ordre d'exécution des cellules
- Oublier la copie de sauvegarde du CSV

# Ce que j'ai Appris

1. Gestion des formats hétérogènes : "#7", "40,014", "Unknown" → float
2. Pipeline ETL reproductible : Du brut au classement en 6 étapes
3. Validation scientifique : Test H1 avec 90% de corrélation

# Deliverables Produits

Dataset nettoyé (1298 manhwas × 8 colonnes)
Classement éditorial principal (manhwa_ranking.csv)
Classement score seul (manhwa_ranking_score_only.csv)  
Visualisations Top10 (matplotlib)
Rapport complet avec stats descriptives
