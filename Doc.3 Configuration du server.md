# Configuration du server ARK ! 
Il faut savoir qu'il y a deux fichiers pour configurer votre server ARK, le `Game.ini` et le `GameUserSetting.ini` 
- **Game.ini** : Ce fichier est plus avancé. Il est utilisé pour les modifications de spawn, personnalisation des niveaux de dinos, et les engrammes.
   ```bash
      nano /home/arkserverserver/ShooterGame/Config/DefaultGame.ini
   ```
   Voici un tableau des différentes commande pour le fichier Game.init
   | 🛠️Commande                               |🧩 Description                                             |
   |-------------------------------------------|------------------------------------------------------------|
   | OverrideOfficialDifficulty=5.0 | Définit le niveau max des dinos sauvages (5.0 × 30 = niveau 150).|
   | bDisableStructurePlacementCollision=true | Construction sans collision |

- **GameUserSetting.ini** : Ce fichier contient les paramètres principaux du serveur, comme l’XP, la difficulté, le taming speed, etc.
   ```bash
     nano /home/arkserverserver/ShooterGame/Config/DefaultGameUserSetting.ini
   ```
   Voici un tableau des différentes commande pour le fichier GameUserSetting.init
   | 🛠️Commande                               |🧩 Description                                             |
   |-------------------------------------------|------------------------------------------------------------|
   | ShowFloatingDamageText=True | Affiche les dégâts au-dessus des créatures quand on les attaque. |
   | DifficultyOffset=1.0 | Active la difficulté maximale (à combiner avec `OverrideOfficialDifficulty`) |
   | TamingSpeedMultiplier=10.0 | Taming 10 fois plus rapide. |
   | EnablePvPGamma=True | Permet aux joueurs d’utiliser les commandes gamma (luminosité). |
   | ServerCrosshair=True | Affiche un viseur à l’écran (utile pour les tirs). |
   | ShowMapPlayerLocation=True | Affiche la position du joueur sur la carte. |
   | AutoSavePeriodMinutes=15 | Sauvegarde automatique du serveur toutes les 15 minutes. |

Pour ajouter, modifier ou supprimer des commandes, il éditer les deux fichier avec vos préféracence 


