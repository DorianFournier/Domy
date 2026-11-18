# Feuille de route

Principe : **une étape = une fonctionnalité utilisable**, sans frais tant que
ce n'est pas explicitement décidé.

## ✅ Étape 1 — Socle

- Home Assistant OS sur le PC Asus, configuration versionnée et déployée par git
- Mode maison (Présent / Absent / Nuit / Vacances)
- Agenda iCloud : prochaine intervention école, heures de cours à venir
- Tableau de bord mural de l'entrée (iPad)
- Validation automatique de la configuration à chaque push

## Étape 2 — Présence et notifications

- Présence via l'application iPhone → mode Absent/Présent automatique
- Notification la veille d'une intervention : heure, lieu, temps de trajet
- Résumé du matin sur l'iPad (agenda du jour, météo, rappels)

## Étape 3 — Sport

- Calendrier « Sport » dans iCloud, séances planifiées dans les créneaux libres
- Suivi de la régularité (séances par semaine, série en cours)
- Rappel quand une séance approche, avec un report possible en un tap

> 💭 À trancher : Home Assistant ne peut pas **écrire** dans iCloud via
> CalDAV. Il faudra soit que Domy propose des créneaux que tu ajoutes, soit un
> petit composant Python qui écrit en CalDAV.

## Étape 4 — Auto-entrepreneur (`custom_components/domy_ae`)

- Heures facturées par école et par mois, d'après le calendrier « Écoles »
- Chiffre d'affaires cumulé vs **plafond micro-entreprise**
- Rappels des déclarations URSSAF (mensuelles ou trimestrielles)
- Génération des factures PDF

## Étape 5 — Alimentation (Cookidoo)

- Intégration **Cookidoo** officielle de Home Assistant : liste de courses et
  planning des recettes
- Menu de la semaine sur l'iPad

> ⚠️ Cookidoo n'a pas d'API publique officielle : l'intégration repose sur une
> bibliothèque non officielle qui peut casser lors d'une mise à jour de
> Vorwerk. Rien d'essentiel ne doit en dépendre.

## Étape 6 — Domotique (dès les premiers objets)

- Clé **Zigbee** (≈ 25 €) : protocole local, sans cloud, capteurs peu chers
- Premiers achats conseillés : détecteurs d'ouverture et de mouvement,
  ampoules ou modules d'éclairage, prises connectées
- Éclairage selon le mode maison, chauffage selon la présence

## Idées en réserve

- **Assistant vocal local** (Assist de Home Assistant, gratuit) : commandes
  vocales sans cloud, avant d'envisager un LLM.
- **Suivi énergie** : consommation électrique (Linky via un module TIC) pour
  repérer les gaspillages.
- **Charge de l'iPad** pilotée entre 20 % et 80 % pour préserver la batterie.
- **Sauvegardes automatiques** de Home Assistant vers un stockage externe.
