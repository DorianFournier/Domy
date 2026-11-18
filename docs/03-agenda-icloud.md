# 3. Connecter l'agenda iCloud

Home Assistant lit iCloud grâce au protocole **CalDAV**, en lecture seule.

## Conseil : un calendrier dédié aux écoles

Si ce n'est pas déjà fait, crée dans iCloud un calendrier séparé **« Écoles »**
pour les interventions. Avantages :
- Domy distingue automatiquement les cours du reste ;
- ça servira directement au **suivi auto-entrepreneur** (heures par école,
  chiffre d'affaires, factures).

## Étape A — Mot de passe pour application

Ton mot de passe Apple ne doit pas être donné à Home Assistant. À la place :

1. Va sur <https://account.apple.com> → *Connexion et sécurité* →
   **Mots de passe pour application**.
2. Crée-en un nommé « Home Assistant » et copie-le.

## Étape B — Ajouter l'intégration

*Paramètres → Appareils et services → Ajouter une intégration →* **CalDAV** :

| Champ | Valeur |
| --- | --- |
| URL | `https://caldav.icloud.com` |
| Nom d'utilisateur | ton identifiant Apple (e-mail) |
| Mot de passe | le mot de passe pour application |
| Vérifier le certificat SSL | oui |

Chaque calendrier iCloud devient une entité `calendar.xxx`.

## Étape C — Renommer les entités

Domy attend deux identifiants précis. Dans *Paramètres → Entités*, ouvre chaque
calendrier → ⚙️ → *Identifiant d'entité* :

| Calendrier iCloud | Identifiant attendu |
| --- | --- |
| Ton calendrier personnel | `calendar.personnel` |
| Le calendrier des interventions | `calendar.ecoles` |

## Vérifier

- `sensor.prochaine_intervention_ecole` affiche le titre du prochain cours
  (attributs : `debut`, `lieu`).
- `sensor.heures_de_cours_sur_14_jours` donne le volume horaire à venir.
- Les capteurs se mettent à jour toutes les 15 minutes.

> 🎓 Pour apprendre : ouvre *Outils de développement → Actions*, choisis
> `calendar.get_events` sur `calendar.ecoles`, et regarde la réponse. C'est
> exactement ce que lit `packages/agenda.yaml`.
