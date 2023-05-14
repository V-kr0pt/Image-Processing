# Radiométrie et contraste

## Intensité lumineuse et vision humaine
La perception du contenu d'une image varie peu si l'on applique une fonction croissante à l'image ex: $x |-> x^2$ ou $x \sqrt{x}$ 

Faux si la fonction n'est pas une fonction croissante: cas par ex du négatif d'une photo $x |-> 1-x$.

Pourquoi? Puis que nous somme très sensibles aux contrastes locaux. bien plus qu'aux contrastes globaux dans l'image.

---
## Histogrammes et changements de contraste
On appelle changement de contraste une fonction g: R |-> R croisssante. Ce changement de contraste transforme l'image u en $g(u)$ (operation non linéaire en général globale sur l'image)

---
- Luminosité, contraste: $u |-> ku + C$ avec k (contraste) et C (luminosité) constant

- Contrast Stretching: $u |-> ku + C$ avec k, C de manière à utiliser toute la dynamique

- Transformation gamma: $u |-> u^{\gamma}$

- Seuillage $u |-> I(u > \lambda)$ devient binaire. Méthode élémentaire d'extraction de formes (texte scanné)

- Négatif: utile pour images médicales (zones lumineuses = zones les moins denses)

- Echelle logarithmique: utile si l'image possède une dynamique trÈs étalée

---
## Correction Gamma

---

## Egalisation d'histogramme
Le changement de contraste qui consistent à prendre $H_u$ pour fonction croissante g s'appelle égalisation d'histogramme.

On parle d’égalisation car l’histogramme cumul  ́e de l’image g(u) ainsi obtenue se rapproche “le plus possible” de l’identité

## Augmentation du bruit de quantification
---

## Spécification d'histogramme
on cherche un changement de contraste g tel que l’histogramme cumul  ́e de
g(u) soit le plus proche possible d’une certaine fonction F strictement croissante et
continue de [0,1] sur [0,1]

---
# Transformations d'histrogrammes locales

## Modifications locales du contraste
Traitement par blocs (sous-parties carrées de l'image) méthode CLAHE. Les blocs doivent se chevaucher pour éviter les artefacts.

*ipol = image processing online*

# Effet sur l'histogramme de l'ajout de bruit sur l'image

Bruit additif: ajout d'un bruit b à la variable aléatoire u, $u_b = u + b$. u + b variable aléatoire de densité $h_u * h_b$.

- b bruit Gaussien $N(0,\sigma^2)$
- b bruit uniforme -> histogramme convolué avec une fonction porte

bruit impulsionnel: ub = (1 −X )u + XY où X suit une loi de Bernouilli de paramètre
θ et Y une loi uniforme sur {0,...,255}.

---

## Imagerie HDR (High Dynamic Range)

Stratégie classique: le multi-images en different temps d'exposition.

## Dithering
améliorer le rendu de la quantification en ajoutant du bruit à l’image avant de la quantifier.