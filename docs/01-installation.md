# 1. Installer Home Assistant et brancher le dépôt

## Quelle machine ?

**Le PC Asus.** Le Raspberry Pi 3 n'a que 1 Go de RAM et démarre sur carte
SD. Ça suffit pour démarrer, mais ça devient lent dès qu'on ajoute des add-ons,
et les cartes SD finissent par lâcher à force d'écritures. Le PC est
gratuit, plus rapide, et a un vrai disque.

Prérequis :
- un processeur 64 bits (tout PC de moins de ~12 ans) ;
- le démarrage en **UEFI** activé dans le BIOS ;
- branché **en Ethernet** sur la box (plus fiable que le Wi-Fi pour un serveur) ;
- ⚠️ le disque du PC sera **entièrement effacé**.

> Il n'existe pas d'hébergement en ligne gratuit qui convienne : la domotique
> doit tourner *dans* la maison pour parler aux objets, même sans Internet.

## Étape A — Installer Home Assistant OS

1. Sur ton ordinateur habituel, télécharge :
   - l'image **Generic x86-64** de Home Assistant OS : <https://www.home-assistant.io/installation/generic-x86-64>
   - [balenaEtcher](https://etcher.balena.io/) et une image **Ubuntu Desktop** (en clé live).
2. Crée une clé USB Ubuntu avec Etcher, démarre le PC Asus dessus en
   choisissant « Essayer Ubuntu » (sans installer).
3. Depuis Ubuntu live, ouvre Firefox, télécharge l'image Home Assistant OS,
   puis utilise l'outil **Disques** → disque interne → « Restaurer l'image
   disque » avec le fichier téléchargé.
4. Éteins, retire la clé, rallume. Après quelques minutes, ouvre
   <http://homeassistant.local:8123> depuis un autre appareil du réseau.
5. Suis l'assistant : crée ton compte, nomme la maison **Maison**, place-la
   sur la carte.

> Le guide officiel détaillé, avec photos : <https://www.home-assistant.io/installation/generic-x86-64>

Dans l'interface de ta box internet, réserve une **adresse IP fixe** pour le PC.

## Étape B — Installer les add-ons utiles

*Paramètres → Modules complémentaires → Boutique* :

| Add-on | Pourquoi |
| --- | --- |
| **Terminal & SSH** | Ligne de commande sur le serveur |
| **File editor** | Éditer `secrets.yaml` depuis le navigateur |
| **Git pull** | Récupère automatiquement ce dépôt |
| **Tailscale** | Accès sécurisé depuis l'extérieur, gratuit (plus tard) |

Active aussi le **mode avancé** dans ton profil utilisateur (en bas de la page
profil), sinon certains réglages restent cachés.

## Étape C — Relier `/config` à ce dépôt

Le dépôt est privé : le serveur a besoin d'une **clé de déploiement**
(une clé SSH qui ne peut que lire ce seul dépôt).

1. Ouvre **Terminal & SSH** et génère la clé :
   ```bash
   ssh-keygen -t ed25519 -N "" -f /config/.ssh/id_ed25519 -C domy-server
   cat /config/.ssh/id_ed25519.pub
   ```
2. Sur GitHub : dépôt **Domy** → *Settings* → *Deploy keys* → *Add deploy key*.
   Colle la clé publique, **sans** cocher « Allow write access ».
3. Fais une **sauvegarde** de Home Assistant (*Paramètres → Système →
   Sauvegardes*) par précaution.
4. Toujours dans le terminal, initialise le dépôt dans `/config` :
   ```bash
   cd /config
   git init -b main
   git remote add origin git@github.com:DorianFournier/Domy.git
   GIT_SSH_COMMAND="ssh -i /config/.ssh/id_ed25519 -o StrictHostKeyChecking=accept-new" git fetch origin main
   git checkout -f main
   ```
   Les fichiers propres au serveur (base de données, `.storage/` qui contient
   ton compte, etc.) sont ignorés par git et ne sont pas touchés.
5. Crée les secrets :
   ```bash
   cp secrets.yaml.example secrets.yaml
   ```
   puis ouvre `secrets.yaml` avec **File editor** et mets les vraies
   coordonnées de la maison.
6. *Outils de développement → YAML → Vérifier la configuration*, puis
   **Redémarrer**.

## Étape D — Mises à jour automatiques avec Git pull

Configuration de l'add-on **Git pull** (onglet *Configuration*, en YAML) :

```yaml
git_branch: main
git_command: pull
git_remote: origin
git_prune: false
repository: git@github.com:DorianFournier/Domy.git
auto_restart: true
restart_ignore:
  - dashboards/
  - docs/
  - README.md
repeat:
  active: true
  interval: 300
deployment_user: ""
deployment_password: ""
deployment_key:
  - "-----BEGIN OPENSSH PRIVATE KEY-----"
  - "… une ligne par ligne du fichier /config/.ssh/id_ed25519 …"
  - "-----END OPENSSH PRIVATE KEY-----"
deployment_key_protocol: ed25519
```

Démarre l'add-on. Toutes les 5 minutes, il récupère les nouveautés,
**vérifie la configuration**, et ne redémarre Home Assistant que si elle est
valide. Les tableaux de bord YAML n'ont pas besoin de redémarrage : un simple
rafraîchissement de la page suffit.

## Étape E — L'application mobile (gratuit, et très utile)

Installe **Home Assistant** sur ton iPhone (App Store) et connecte-toi. En plus
de piloter la maison, l'application donne à Home Assistant **ta présence**
(à la maison / absent) : ce sera la base des automatisations du mode maison,
sans acheter de capteur.

## Vérifier que tout marche

- Le tableau de bord **Entrée** apparaît dans la barre latérale.
- *Paramètres → Entités* : `input_select.domy_mode_maison` et
  `sensor.salutation` existent.
- L'entité météo s'appelle `weather.forecast_maison`. Si elle a un autre nom
  (ex. `weather.forecast_home`), renomme son identifiant d'entité en
  `weather.forecast_maison` (*Paramètres → Entités → la météo → ⚙️*).
