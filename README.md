# Domy

Assistant personnel domestique basé sur **Home Assistant** : domotique, agenda,
sport, alimentation et suivi de l'activité d'auto-entrepreneur, le tout piloté
depuis un iPad fixé au mur de l'entrée.

Contrainte de départ : **zéro frais récurrent**. Tout tourne en local, sur une
machine de la maison.

## Architecture

```
            ┌─────────────────────────────┐
  iPad      │  PC Asus — Home Assistant OS │      iCloud (CalDAV)
  mural ───►│                             │◄──── Cookidoo (plus tard)
 (tableau   │  ┌───────────────────────┐  │
  de bord)  │  │ /config = ce dépôt git │  │◄──── Objets connectés
            │  │  packages/  dashboards/│  │      (Zigbee, Wi-Fi… plus tard)
  iPhone ──►│  │  custom_components/    │  │
 (appli HA) │  └───────────────────────┘  │
            └──────────────▲──────────────┘
                           │ add-on « Git pull »
                      GitHub (Domy)
```

- **Ce dépôt est le dossier `/config` de Home Assistant.** On développe ici,
  on pousse sur GitHub, et le serveur récupère les changements tout seul.
- **Un package par fonctionnalité** (`packages/`) : helpers, capteurs et
  automatisations d'un même domaine restent ensemble.
- **Les tableaux de bord sont en YAML** (`dashboards/`), donc versionnés.
- Ce que Home Assistant ne sait pas faire (ex. suivi auto-entrepreneur) sera
  développé en Python dans `custom_components/`.

## Structure

| Chemin | Rôle |
| --- | --- |
| `configuration.yaml` | Point d'entrée de Home Assistant |
| `packages/domy_core.yaml` | Mode maison (Présent / Absent / Nuit / Vacances) |
| `packages/agenda.yaml` | Prochaine intervention école, heures de cours |
| `dashboards/entree.yaml` | Tableau de bord de l'iPad mural |
| `themes/domy.yaml` | Thème sombre du tableau mural |
| `secrets.yaml.example` | Modèle des secrets (le vrai `secrets.yaml` n'est jamais versionné) |
| `docs/` | Guides d'installation et feuille de route |

## Démarrage

1. [Installer Home Assistant sur le PC Asus et brancher ce dépôt](docs/01-installation.md)
2. [Connecter l'agenda iCloud](docs/03-agenda-icloud.md)
3. [Transformer l'iPad en tableau mural](docs/02-ipad-mural.md)

La suite est dans la [feuille de route](docs/roadmap.md).

## Validation

Chaque push lance une vérification automatique (GitHub Actions) : `yamllint`
et `check_config` de Home Assistant (versions stable et bêta). Une
configuration cassée est donc repérée avant d'arriver sur le serveur.
