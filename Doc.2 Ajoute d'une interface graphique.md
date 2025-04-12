# Documentation pour le Contrôle du Serveur ARK avec Apache

Cette documentation décrit comment configurer un serveur Apache pour contrôler un serveur ARK via une interface web.

## Étape 1 : Installation d'Apache

Mettez à jour votre liste de paquets et installez Apache :

```bash
sudo apt update
sudo apt upgrade
sudo apt install apache2
```

## Étape 2 : Création de l'interface de contrôle

On va utiliser le fichier index.html de base :

```bash
sudo nano /var/www/html/index.html
```
Ajout le code HTML
```html
<!DOCTYPE html>
<html lang="fr">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Serveur ARK: Survival Evolved</title>
    <link rel="stylesheet" href="style.css">
    <link rel="shortcut icon" href="logo ARK.png">
    <script>
        // programme lancer et stop server
        function executeServerAction(scriptPath) {
            const xhr = new XMLHttpRequest();
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4 && xhr.status === 200) {
                    document.getElementById("messageContent").innerHTML = xhr.responseText;
                }
            };
            xhr.open("GET", scriptPath, true);
            xhr.send();
        }
        // programme status server
        function updateServerStatus() {
            var xhr = new XMLHttpRequest();
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4 && xhr.status === 200) {
                    document.getElementById("serverStatus").innerHTML = xhr.responseText;
                }
            };
            xhr.open("GET", "/cgi-bin/statusserver.sh", true);
            xhr.send();
        }
        // Rafraîchir le statut toutes les 10 secondes
        setInterval(updateServerStatus, 10000);
        window.onload = updateServerStatus;
    </script>
</head>

<body>
    <header>
        <div class="container">
            <h1><img src="logo ARK.png" alt="Logo ARK" class="ark-logo"> Serveur ARK: Survival Evolved 🌋</h1>
            <p>Gérez votre serveur facilement</p>
        </div>
    </header>

    <main class="container">
        <section class="server-info">
            <div class="server-control">
                <h2>🎮 Contrôle du serveur</h2>
                <div class="button-group">
                    <button onclick="executeServerAction('/cgi-bin/startserver.sh')" class="start-btn">Démarrer</button>
                    <button onclick="executeServerAction('/cgi-bin/stopserver.sh')" class="stop-btn">Arrêter</button>
                </div>
                <br>
                <div class="server-status">
                    <h2>📡 Statut du serveur</h2>
                    <div id="serverStatus" class="status-box">
                        Récupération en cours...
                    </div>
                </div>
            </div>
        </section>

        <section class="server-messages">
            <h2>📢 Messages du serveur</h2>
            <div id="messageContent" class="message-box">
                Aucune action pour le moment.
            </div>
        </section>
    </main>

    <footer>
        <p>&copy; <strong>Vaylz Serveur ARK 2025</strong> - Interface Web personnalisée</p>
    </footer>
</body>

</html>
```
Creer un fichier pour le CSS
```bash
sudo nano /var/www/html/style.css
```
Ajout le code CSS
```css
body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #0b0c10;
    color: #c5c6c7;
}

header {
    background-color: #1c1e25;
    color: #66fcf1;
    padding: 10px 20px;
    border-bottom: 3px solid #45a29e;
}

header .container {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 5px;
}

header h1 {
    display: flex;
    align-items: center;
    font-size: 1.8rem;
    margin: 0;
}

header p {
    margin: 0;
    font-size: 0.9rem;
    color: #c5c6c7;
}

.ark-logo {
    width: 50px;
    height: auto;
    margin-right: 10px;
    vertical-align: middle;
}


.ark-logo {
    max-width: 150px;
    height: auto;
    margin-bottom: 1rem;
}

.container {
    max-width: 1000px;
    margin: 0 auto;
    padding: 1rem;
}

main {
    padding: 2rem 1rem;
}

h1, h2 {
    margin-bottom: 1rem;
}

.server-control, .server-messages {
    background: #1f2833;
    padding: 1.5rem;
    border-radius: 10px;
    margin-bottom: 2rem;
    box-shadow: 0 0 10px rgba(102, 252, 241, 0.2);
}

.button-group {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
}

button {
    padding: 1rem 2rem;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: bold;
    cursor: pointer;
    transition: 0.3s ease;
}

.start-btn {
    background: #45a29e;
    color: white;
}

.start-btn:hover {
    background: #66fcf1;
    color: #0b0c10;
}

.stop-btn {
    background: #c3073f;
    color: white;
}

.stop-btn:hover {
    background: #950740;
}

.message-box {
    background: #0b0c10;
    border: 1px solid #66fcf1;
    padding: 1rem;
    border-radius: 5px;
    min-height: 100px;
    white-space: pre-wrap;
    word-wrap: break-word;
}

footer {
    text-align: center;
    padding: 0.2rem;
    background: #1f2833;
    color: #45a29e;
}


.server-info {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 2rem;
}

.server-control, .server-status {
    flex: 1 1 45%;
    background: #1f2833;
    padding: 1.5rem;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(102, 252, 241, 0.2);
}

.status-box {
    background-color: #0b0c10;
    border: 2px solid #45a29e;
    padding: 1rem;
    border-radius: 8px;
    color: #66fcf1;
    font-weight: bold;
    text-align: center;
}

```
## Étape 3 : Création des scripts CGI

Créez les scripts `startserver.sh` et `stopserver.sh` dans `/usr/lib/cgi-bin/` :
```bash
sudo nano /usr/lib/cgi-bin/startserver.sh
```

Ajoutez le code suivant dans `startserver.sh` :
```bash
#!/bin/bash

# En-têtes HTTP
echo "Content-type: text/html"
echo ""

# Chemin vers le dossier ARK
ARK_DIR="/home/arkserver/server/ShooterGame/Binaries/Linux"

# Nom de la carte
MAP_NAME="TheIsland"
# Nombre de slots disponibles
PLAYER_SLOTS=5

# Vérification si le serveur est déjà en cours d'exécution
if pgrep -f "ShooterGameServer" > /dev/null; then
    echo "<html><body>"
    echo "<h1>Contrôle du Serveur</h1>"
    echo "<p>Le serveur ARK est déjà en cours d'exécution.</p>"
    echo "</body></html>"
    exit 1
fi

# Paramètres ulimit
#ulimit -n 100000

# Se déplacer dans le répertoire contenant ShooterGameServer
cd "$ARK_DIR" || {
    echo "<html><body>"
    echo "<h1>Erreur</h1>"
    echo "<p>Impossible de trouver le répertoire ARK à $ARK_DIR.</p>"
    echo "</body></html>"
    exit 1
}

# Vérification de l'existence du fichier ShooterGameServer
if [ ! -f "./ShooterGameServer" ]; then
    echo "<html><body>"
    echo "<h1>Erreur</h1>"
    echo "<p>Le fichier 'ShooterGameServer' n'a pas été trouvé dans le répertoire $ARK_DIR.</p>"
    echo "</body></html>"
    exit 1
fi

# Démarrage du serveur ARK
./ShooterGameServer "$MAP_NAME?listen?MaxPlayers=$PLAYER_SLOTS" -server -log &

# Récupérer le PID du serveur
SERVER_PID=$!

# Vérifier si le serveur a démarré avec succès
sleep 5

echo "<html><body>"

if ps -p $SERVER_PID > /dev/null; then
    echo "<h1>Contrôle du Serveur</h1>"
    echo "<p>Serveur ARK démarré avec succès (PID: $SERVER_PID).</p>"
else
    echo "<h1>Contrôle du Serveur</h1>"
    echo "<p>Échec du démarrage du serveur ARK.</p>"
fi

echo "</body></html>"
exit 0
```

Puis, créez `stopserver.sh` :
```bash
sudo nano /usr/lib/cgi-bin/stopserver.sh
```

Ajoutez le code suivant dans `stopserver.sh` :
```bash
#!/bin/bash

# En-têtes HTTP
echo "Content-type: text/html"
echo ""

# Forcer le serveur à sauvegarder avant de l'arrêter
echo "<html><body>"
echo "<h1>Contrôle du Serveur</h1>"
echo "<p>Forçage de la sauvegarde du serveur...</p>"

# Fichier de sauvegarde (vous pouvez spécifier un chemin complet si nécessaire)
backup_file="/path/to/your/directory/backup.log"

# Simule la commande RCON pour sauvegarder le monde
echo "Exécution de la commande de sauvegarde..."
# Remplacez 'rcon_command' par votre commande RCON pour sauvegarder le monde
rcon_command "SaveWorld" >> "$backup_file" 2>&1

# Attendre que la sauvegarde soit terminée
sleep 10

# Récupérer le PID du serveur
PID=$(pgrep -f "ShooterGameServer")

if [ -z "$PID" ]; then
    echo "<p>Le serveur ARK n'est pas en cours d'exécution.</p>"
else
    kill -SIGTERM "$PID"
    echo "<p>Serveur ARK arrêté.</p>"
fi

echo "</body></html>"
exit 0
```

rendre les script  executible
```bash
sudo chmod +x /usr/lib/cgi-bin/startserver.sh /usr/lib/cgi-bin/stopserver.sh
```

## Étape 4 : Configuration d'Apache pour les scripts CGI

Activez le module CGI d'Apache :
```bash
sudo a2enmod cgi
```

Ouvrez le fichier de configuration d'Apache :
```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```
Ajoutez la configuration suivante dans le fichier `000-default.conf` :
```apache
<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options ExecCGI
    Order allow,deny
    Allow from all
</Directory>
```

## Étape 5 : Creation d'un group ARKgroup

On va créer un groupe appelé `arkgroup`
```bash
sudo groupadd arkgroup
```

Ajouter les utilisateurs arkserver et www-data au groupe
```bash
sudo usermod -aG arkgroup arkserver
sudo usermod -aG arkgroup www-data
```

Ensuite, il faut changer le groupe propriétaire du répertoire ARK pour qu'il soit sous la gestion de arkgroup. Cela va permettre à tous les utilisateurs dans ce groupe d'accéder au dossier.
```bash
sudo chown -R arkserver:arkgroup /home/arkserver
sudo chmod -R 770 /home/arkserver
```
Attention de bien vérifier si le group arkgroup a les droits sur l'utilisateur arkserver

## Étape 6 : Accédez à votre interface

Puis redémarrez Apache :
```bash
sudo systemctl restart apache2
```

Vous pouvez maintenant accéder à votre site via l'URL suivante :
```
http://votre-serveur/ark_server_control/index.html
```
