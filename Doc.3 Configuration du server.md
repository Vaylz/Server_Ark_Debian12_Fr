# Configuration du server ARK ! 
Il faut savoir qu'il y a deux fichiers pour configurer votre server ARK, le `Game.ini` et le `GameUserSetting.ini` 
- **Game.ini** : Ce fichier est plus avancé. Il est utilisé pour les modifications de spawn, personnalisation des niveaux de dinos, et les engrammes.
- **GameUserSetting.ini** : Ce fichier contient les paramètres principaux du serveur, comme l’XP, la difficulté, le taming speed, etc.

Pour ajouter, modifier ou supprimer des commandes, il éditer les fichier :
```bash
nano /home/arkserverserver/ShooterGame/Config/DefaultGame.ini
```
Voici un tableau des différentes commande pour le fichier Game.init
| 🛠️commande | 🧩 Description / Effet |
|--- |--- |--- |
| OverrideOfficialDifficulty=5.0 | Définit le niveau max des dinos sauvages (5.0 × 30 = niveau 150). |
| bDisableStructurePlacementCollision=true | Autorise la construction sans collision (plus de liberté dans les bases). |

```bash
nano /home/arkserverserver/ShooterGame/Config/DefaultGameUserSetting.ini
```
Voici un tableau des différentes commande pour le fichier GameUserSetting.init
| 🛠️commande | 🧩 Description / Effet |
|--- |--- |--- |
| OverrideOfficialDifficulty=5.0 | Définit le niveau max des dinos sauvages (5.0 × 30 = niveau 150). |
| bDisableStructurePlacementCollision=true | Autorise la construction sans collision (plus de liberté dans les bases). |
