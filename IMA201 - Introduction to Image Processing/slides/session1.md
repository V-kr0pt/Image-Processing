# Modele classiques pour le bruit
- Bruit additif
- Bruit impulsionnel
- Bruit multiplicatif

# Les causes du bruit
- Bruit shot noise: Nombre de photons emis par la source: loi de POisson de moyenne $C_\tau$ avec $C$ radiance (nb de photons par unité de temps et $\tau$ temps d'exposition )

- Courant d'obscurité (Dark Current): Emission résiduelle d'électrons d'origine thermique: Poisson de moyenne $d_{\tau}$ dépendant de $\tau$

- Bruit de lecture (readout noise): Erreurs lors de la lecture des électrons (par rapport à un voltage de référence)

- Non uniformités spatiales
de la réponse des photo-senseurs (PRNU)
du bruit thermique (DCNU)

----
## Annotation (Poisson)
$P(x=k) = \frac{\lbd^k e^{-\lbd}}{k!}$
---

# Modèle Complet
$I_{noise} = f([g(Poiss(C_{\tau} + Poiss(d_\tau)))] + N_{out}) + Q$

- On considère des imagens RAW, pour lesquelles $f=Id$ (réponse linéaire au nombre de photons)
- Le bruit de quantification Q est négligeable devant le bruit de lecture
- Le dark current est négligeable pour temps d'exposition courts (<1s)

Alors,

$ I_noise = g Poiss(C_{\tau}) + N_{out}$

Pour $\lbd$ grand (quelques dizaines) on a $P(\lbd) \approx N(\lbd, \lbd)$

