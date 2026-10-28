Compris, on avance sur cette base. Ces directives changent plusieurs points de la structure.

## Ce que les directives tranchent

- **Version non anonyme.** Le lien GitHub va à la fin de l'abstract et le dépôt doit être partagé avec l'utilisateur GitHub `upaq`. On garde donc `[accepted]` et les noms d'auteur, et la question blind/accepted est réglée.
- **Longueur : les consignes se contredisent.** Les directives demandent « at least 8 pages » hors annexes, alors que le template ICML plafonne le corps à 8 pages. Je propose d'viser exactement 8 pages de corps, et de mettre les dérivations longues en annexes techniques.
- **Contenu exigé.** Introduction et travaux passés, énoncé du problème, graphiques de données (lois de puissance, distribution des notes), tous les modèles, et une longue section de résultats couvrant les Practicals 1 à 7.

## Structure proposée

Les pourcentages sont les points du barème et l'état est indiqué entre parenthèses.

**Title, Authors, Abstract** : 4 à 6 phrases en un paragraphe, qui se termine par le lien GitHub.

**1. Introduction** : motivation, travaux passés (Netflix Prize, Koren, Yahoo! Music, Xbox), contributions.

**2. Problem Statement and Data**
- 2.1 Problem statement et notation.
- 2.2 Datasets : le petit jeu pour le développement, le 25M comme jeu principal.
- 2.3 Indexation (Practical 1).
- 2.4 **Dataset plots (5 %)** : degrés en log-log et CCDF, ajustement de loi de puissance, **distribution des valeurs de notes** (partiel : je n'ai vu aucun histogramme des notes dans les cellules lues).
- 2.5 Train/test split (3 % + 3 % pour les résultats sur ton split) : 90/10 par utilisateur (fait).

**3. Models** (tous les modèles écrits)
- 3.1 Bias-only (fait). 3.2 Facteurs latents (fait).
- 3.3 Modèle avec **genres** (7 %, à faire) : fonction de perte modifiée et étapes d'optimisation.
- 3.4 Feedback implicite et BPR (7 %, à faire).
- 3.5 Variational Bayes (bonus 7 %, optionnel).

**4. Inference and Implementation**
- 4.1 Mises à jour ALS et Algorithm 1 (Cholesky en annexe).
- 4.2 Implémentation passante à l'échelle : CSR, lecture par blocs, numba parallèle (PDF 1 et 2).
- 4.3 Protocole d'évaluation : RMSE, Precision@k, Recall@k.

**5. Experiments and Results** (la section longue)
- 5.1 Bias-only, RMSE train/test (4 %).
- 5.2 Facteurs latents, RMSE et **courbes de loss monotones** (8 % + 3 %).
- 5.3 Nombre d'itérations et convergence.
- 5.4 Choix de λ et τ (4 %).
- 5.5 Choix de K et rendements décroissants (4 %).
- 5.6 Overfitting par groupe d'utilisateurs (PDF 3 et 4).
- 5.7 Entraînement sur toutes les notes avec implémentation optimisée (bonus 9 %) (PDF 5).
- 5.8 **Embeddings 2D** des vecteurs films (5 %, à faire).
- 5.9 Embeddings 2D des **genres** (inclus dans les 7 %).
- 5.10 Utilisateur fictif (5 %) (PDF 6).
- 5.11 Films polarisants (2 %) (PDF 7).
- 5.12 Feedback implicite : Precision@k et Recall@k pour k = 10, 20, 100 (à faire).
- 5.13 A/B test : système simulé (5 %) et test t simulé (3 %) (à faire).

**6. Discussion, Limitations and Future Work** ; **7. Conclusion**
**Software and Data** (GitHub, PEP 8), **References**, **Appendices** (dérivations, Cholesky, tableaux supplémentaires, hyperparamètres).

**Budget de pages (corps, 8 pages)** : abstract et introduction 0,9 ; problème et données 1,4 ; modèles 1,3 ; implémentation 0,7 ; résultats 3,0 ; discussion et conclusion 0,5. Avec 19 éléments notés, les figures devront être des multi-panneaux compacts.

**Ordre d'écriture** : d'abord 2.1 à 2.5, 3.1 à 3.2, 4.1 et 5.1 à 5.3 (tes Practicals 1 à 3 et le début du 4), puis le reste.

## Lacunes à combler (par rapport au barème)

- **À faire** : embeddings 2D, modèle avec genres, BPR et Precision/Recall@k, A/B test et test t, histogramme des notes.
- **K insuffisant** : on a testé K = 10 et 20. Les directives suggèrent K = 0, 2, 4, 8, 16, 32 pour montrer les rendements décroissants.
- **λ jamais varié** : on a fixé λ = 1 et varié τ. Il faudra argumenter dans le rapport que seul le rapport τ/λ compte.
- **Accélération à mesurer** : le bonus demande une implémentation plus rapide que la naïve. Il faudra une comparaison chronométrée (naïve contre numba, sur un sous-échantillon extrapolé par exemple).
- **25M contre 32M** : les directives parlent de 32M. Le pipeline CSR gère 32M sans difficulté. Sinon, précise dans le rapport que tu utilises 25M.
- **Bias-only et μ** : le Practical 2 n'inclut pas la moyenne globale μ (d'après la boucle lue), alors que le Practical 4 l'inclut. À unifier.
- **Code** : le barème donne 9 % à la lisibilité et 4 % au style PEP 8. Le code actuel des notebooks (indentation à 2 espaces, commentaires en français) devra être refactorisé en modules Python en anglais.

## Corrections pour tes PDF 6 et 7

Je compte **7 PDF** (5 générés et 2 envoyés). S'il y en a un 8e, envoie-le.

- **PDF 6** : des formules LaTeX sont mal rendues (ex. `\widehat r_{mn=...`). Les sorties du premier profil (les 3 films) ne sont pas montrées.
- **PDF 7** :
  - les en-têtes des tableaux imprimés sont en français (« Sans filtre », « titre ») ;
  - il manque les films **les moins polarisants**, demandés par le barème ;
  - il manque la réponse à « sans que l'overfitting soit l'explication » ;
  - les trilogies LOTR ont les plus grandes normes (8,7 à 8,8) avec un écart-type d'environ 1,0, donc dans la moyenne. La norme pourrait refléter l'intensité de signal ou la popularité plutôt que le désaccord. Le rapport doit en discuter.
- **Figures** : toutes sont à refaire en anglais, sans titre dans l'image, avec la légende dessous. Je peux traduire le code de tracé.

Si cette structure te convient, je prépare le squelette LaTeX basé sur le template (sections vides avec les mentions de marques), puis on rédige la phase 1.