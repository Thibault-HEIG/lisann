# Lisann - Portail de Casting

Ce projet est un portail web pour les castings du spectacle "Lisann". Il permet aux candidats d'évaluer leur affinité avec les différents rôles du spectacle grâce à un algorithme de recommandation.

## Algorithme de Casting (test.html)

L'algorithme calcule un score d'affinité (0-100%) entre un utilisateur et chaque rôle en fonction de deux catégories de critères : les **compétences scéniques** (Performance) et les **traits de caractère** (Caractère).

### 1. Structure des Données
Chaque rôle possède un vecteur de 9 valeurs (0 à 10) correspondant aux critères suivants :
- Index 0-4 & 8 : Liberté, Énergie, Idéalisme, Autorité, Sensibilité, Romance (**Caractère**)
- Index 5-7 : Chant, Danse, Théâtre (**Performance**)

### 2. Filtrage par Genre
L'utilisateur peut filtrer les résultats par genre (Hommes, Femmes, ou Tous). Certains rôles sont marqués comme "All" et apparaissent dans tous les filtres.

### 3. Calcul du Score de Performance (Poids : 40%)
Le score de performance mesure si l'utilisateur possède le niveau technique requis pour le rôle.
- Pour chaque compétence (Chant, Danse, Théâtre) :
  - On calcule un ratio de "couverture" : `min(UserValue, RoleRequirement) / RoleRequirement`.
  - Si le rôle ne requiert aucune compétence (0), le ratio est de 1.
- Le score final de performance est la moyenne de ces ratios.

### 4. Calcul du Score de Caractère (Poids : 60%)
Le score de caractère utilise la **Similitude Cosinus** pour comparer le "profil psychologique" de l'utilisateur avec celui du rôle.
- **Centrage** : Les valeurs (0-10) sont centrées sur 5 (`valeur - 5`) pour obtenir des vecteurs allant de -5 à 5. Cela permet de distinguer les traits dominants des traits plus effacés.
- **Cosinus Similarity** : On calcule l'angle entre le vecteur utilisateur et le vecteur rôle.
  - Un score de 1 signifie que les profils sont parfaitement alignés.
  - Un score de -1 signifie qu'ils sont opposés.
- **Normalisation** : La similitude (-1 à 1) est convertie en un score de 0 à 1 : `(similarity + 1) / 2`.

### 5. Score Final et Affichage
Le score total est une moyenne pondérée :
`Total = (ScorePerformance * 0.4) + (ScoreCaractère * 0.6)`

Les rôles sont ensuite triés par score décroissant. Un badge "Rôle Idéal" est attribué si le score dépasse 90%.

---
© 2026 Lisann - Thibault Moret
