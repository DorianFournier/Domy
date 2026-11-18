# 2. L'iPad en tableau mural

## Un compte dédié

Crée un utilisateur Home Assistant **« iPad entrée »** (*Paramètres →
Personnes → Ajouter*) **sans** droits administrateur. Si quelqu'un manipule
l'iPad, il ne pourra rien casser dans la configuration.

## Afficher le tableau de bord

Selon la version d'iOS de l'iPad :

- **iPadOS récent** : installe l'application **Home Assistant**, connecte-toi
  avec le compte « iPad entrée ».
- **iPad ancien** (si l'application n'est plus proposée) : ouvre Safari sur
  `http://<ip-du-serveur>:8123`, connecte-toi, puis *Partager → Sur l'écran
  d'accueil*. Le tableau de bord s'ouvrira en plein écran comme une
  application.

Ensuite, sur l'iPad, dans le **profil** de l'utilisateur : *Tableau de bord par
défaut* → **Entrée**. Mets aussi le thème du profil sur **Domy mural**.

## Le verrouiller en mode kiosque

1. *Réglages → Luminosité et affichage → Verrouillage automatique* → **Jamais**.
2. *Réglages → Accessibilité → Accès guidé* → activé, avec un code.
3. Ouvre le tableau de bord, puis appuie **3 fois sur le bouton** (latéral ou
   principal) → *Démarrer*. L'iPad reste bloqué sur Home Assistant.

## Côté matériel

- **Alimentation permanente** : une prise derrière le mur ou une goulotte.
  Pour préserver la batterie, une prise connectée pourra plus tard couper la
  charge entre 20 % et 80 % (automatisation à venir).
- **Support** : un support mural magnétique ou un cadre imprimé en 3D.

## Plus tard

- Masquer la barre du haut et la barre latérale (carte *kiosk-mode*, via HACS).
- Allumer l'écran quand quelqu'un passe la porte (détecteur de mouvement).
- Baisser la luminosité la nuit en fonction du mode maison.
