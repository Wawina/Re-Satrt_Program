LES EXERCICES TPS

Exercice 1: Serveur node js

Journalisation d'un site web (avec deux routes : home et help sur un serveur node js), sur un fichier .txt, à chaque visite du client.

RESOLUTION

Exercice 1 : Serveur Node.js avec journalisation
Objectif
Créer un serveur Node.js qui journalise les visites sur deux routes : / (home) et /help, en enregistrant chaque visite dans un fichier .txt.

Étapes de réalisation
1 Préparer l'environnement de développement :
Assurez-vous d'avoir installé Node.js sur votre machine.

2 Initialiser le projet :
Créez un répertoire pour le projet.
Ouvrez le terminal et naviguez vers ce répertoire.
Exécutez la commande suivante pour initialiser un projet Node.js :
```bash
npm init -y
```

3 Créer le serveur :
Créez un fichier nommé server.js.
Dans ce fichier, j'écrirais le code suivant :

```php
const http = require('http');
const fs = require('fs');
const path = require('path');

const logFilePath = path.join(__dirname, 'visits.txt');

const server = http.createServer((req, res) => {
    const currentTime = new Date().toISOString();
    const logEntry = `${currentTime} - Visite: ${req.url}\n`;

    // Journaliser la visite
    fs.appendFile(logFilePath, logEntry, (err) => {
        if (err) {
            console.error('Erreur d\'écriture dans le fichier:', err);
        }
    });

    if (req.url === '/') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end('<h1>Bienvenue sur la page d\'accueil!</h1>');
    } else if (req.url === '/help') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end('<h1>Voici la page d\'aide!</h1>');
    } else {
        res.writeHead(404, { 'Content-Type': 'text/html' });
        res.end('<h1>Page non trouvée!</h1>');
    }
});

const PORT = 3000;
server.listen(PORT, () => {
    console.log(`Serveur en cours d'exécution sur http://localhost:${PORT}`);
});
```

4 Exécuter le serveur :
Dans le terminal, lancez le serveur avec la commande suivante :
```bash
node server.js
```

5 Tester le serveur :
J'ouvrirais un navigateur et j'accéderais à http://localhost:3000/ pour voir la page d'accueil.
Ensuite, j'irais à http://localhost:3000/help pour voir la page d'aide.
Finalement, je vérifierais le fichier visits.txt pour voir les enregistrements des visites.


Exercice 2: Serveur angular

Journalisation d'un site web (avec deux routes : home et help sur un serveur angular), sur un fichier .txt, à chaque visite du client.7

RESOLUTION

Étapes de réalisation
1 Créer l'application Angular :
Assurez-vous d'avoir Angular CLI installé. Si ce n'est pas le cas, installez-le avec la commande suivante :
```bash
npm install -g @angular/cli
```

Créez une nouvelle application Angular :
```bash
ng new journalisation-app
```

Naviguez dans le répertoire de l'application :
```bash
cd journalisation-app
```

2 Créer des composants :
Créez deux composants pour les routes home et help :
```bash
ng generate component home
ng generate component help
```

Configurer le routage :
Ouvrez le fichier app-routing.module.ts et configurez les routes :
```php
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { HelpComponent } from './help/help.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'help', component: HelpComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

4 Créer un service pour la journalisation :
Créez un service pour envoyer les données de journalisation à un backend :
```bash
ng generate service logger
```

Dans logger.service.ts, ajoutez le code suivant :
```php
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class LoggerService {
  private logUrl = 'http://localhost:3000/log'; // URL de l'API

  constructor(private http: HttpClient) { }

  logVisit(route: string): Observable<any> {
    const logEntry = { route, timestamp: new Date() };
    return this.http.post(this.logUrl, logEntry);
  }
}
```

5 Utiliser le service de journalisation :
Dans les composants home et help, appelez le service pour enregistrer les visites :

```php
// home.component.ts
import { Component, OnInit } from '@angular/core';
import { LoggerService } from '../logger.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {
  constructor(private logger: LoggerService) { }

  ngOnInit(): void {
    this.logger.logVisit('/').subscribe();
  }
}

// help.component.ts
import { Component, OnInit } from '@angular/core';
import { LoggerService } from '../logger.service';

@Component({
  selector: 'app-help',
  templateUrl: './help.component.html',
  styleUrls: ['./help.component.css']
})
export class HelpComponent implements OnInit {
  constructor(private logger: LoggerService) { }

  ngOnInit(): void {
    this.logger.logVisit('/help').subscribe();
  }
}
```

6 Créer le backend pour journaliser les visites :
Créez un fichier server.js à la racine du projet Angular.
Installez les dépendances nécessaires :
```bash
npm install express body-parser cors fs
```

Ajoutez le code suivant à server.js :
```js
const express = require('express');
const bodyParser = require('body-parser');
const fs = require('fs');
const path = require('path');

const app = express();
const PORT = 3000;
const logFilePath = path.join(__dirname, 'visits.txt');

app.use(cors());
app.use(bodyParser.json());

app.post('/log', (req, res) => {
    const logEntry = `${new Date().toISOString()} - Visite: ${req.body.route}\n`;
    fs.appendFile(logFilePath, logEntry, (err) => {
        if (err) {
            console.error('Erreur d\'écriture dans le fichier:', err);
            return res.status(500).send('Erreur serveur');
        }
        res.status(200).send('Visite journalisée');
    });
});

app.listen(PORT, () => {
    console.log(`Serveur en cours d'exécution sur http://localhost:${PORT}`);
});
```

7 Démarrer le serveur :
Dans le terminal, exécutez la commande suivante pour démarrer le serveur :
```bash
node server.js
```

8 Démarrer l'application Angular :
Dans un autre terminal, exécutez :
```bash
ng serve
```



Exercice 3: Serveur apache

Journalisation d'un site web (avec deux pages : index.php et help.php sur un serveur apache), sur un fichier .txt, à chaque visite du client.


RESOLUTION
Étapes de réalisation
1 Installer Apache et PHP :
Assurez-vous d'avoir un serveur Apache installé avec PHP. Vous pouvez utiliser des solutions comme XAMPP, WAMP ou MAMP, ou installer Apache et PHP directement sur votre système.

2 Configurer le répertoire du serveur :
Créez un répertoire pour votre projet dans le dossier htdocs (ou l'équivalent pour votre installation Apache). Par exemple, créez un dossier nommé journalisation.

3 Créer les fichiers PHP :
Dans le dossier journalisation, créez les fichiers suivants :
index.php :
```php
<?php
$logFilePath = 'visits.txt';
$currentTime = date('Y-m-d H:i:s');
$logEntry = "$currentTime - Visite: /index.php\n";

// Journaliser la visite
file_put_contents($logFilePath, $logEntry, FILE_APPEND);
?>
```

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page d'accueil</title>
</head>
<body>
    <h1>Bienvenue sur la page d'accueil!</h1>
    <a href="help.php">Page d'aide</a>
</body>
</html>

help.php :
```php
<?php
$logFilePath = 'visits.txt';
$currentTime = date('Y-m-d H:i:s');
$logEntry = "$currentTime - Visite: /help.php\n";

// Journaliser la visite
file_put_contents($logFilePath, $logEntry, FILE_APPEND);
?>
```

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page d'aide</title>
</head>
<body>
    <h1>Voici la page d'aide!</h1>
    <a href="index.php">Retour à l'accueil</a>
</body>
</html>

4 Configurer Apache :
Assurez-vous que le module mod_rewrite est activé, bien que pour ce cas simple, cela ne soit pas strictement nécessaire. Vérifiez votre fichier httpd.conf ou apache2.conf pour activer ce module si besoin.

5 Démarrer Apache :
Lancez votre serveur Apache via votre interface de gestion (XAMPP, WAMP, etc.) ou en utilisant la ligne de commande.

6 Tester le site :
Ouvrez votre navigateur et accédez à http://localhost/journalisation/index.php pour visiter la page d'accueil.
Ensuite, accédez à http://localhost/journalisation/help.php pour visiter la page d'aide.
Vérifiez le fichier visits.txt dans le même répertoire pour voir les enregistrements des visites.


Exercice 4: Backup

L'automatisation de la journalisation et backup, pour chaque log de serveurs (php, node js, angular) dans des fichiers .txt.
A chaque fois que les logs atteindront 2ko de taille, copier leurs contenus dans fichiers backup et vider les log.

RESOLUTION
1. Serveur PHP
Code pour la journalisation et la sauvegarde :

Dans votre fichier index.php et help.php, ajoutez une fonction pour gérer la sauvegarde.
```php
<?php
function logVisit($route) {
    $logFilePath = 'visits.txt';
    $backupFilePath = 'backup_visits.txt';
    
    $currentTime = date('Y-m-d H:i:s');
    $logEntry = "$currentTime - Visite: $route\n";

    // Journaliser la visite
    file_put_contents($logFilePath, $logEntry, FILE_APPEND);
    manageBackup($logFilePath, $backupFilePath);
}

function manageBackup($logFilePath, $backupFilePath) {
    if (filesize($logFilePath) >= 2048) { // 2 Ko
        // Sauvegarder le contenu
        copy($logFilePath, $backupFilePath);
        // Vider le fichier log
        file_put_contents($logFilePath, '');
    }
}

// Appeler logVisit dans les pages
logVisit('/index.php'); // Pour index.php
//logVisit('/help.php'); // Pour help.php
?>
```

2. Serveur Node.js
Code pour la journalisation et la sauvegarde :

Dans votre fichier server.js, ajoutez une fonction similaire.
```js
const fs = require('fs');
const path = require('path');

const logFilePath = path.join(__dirname, 'visits.txt');
const backupFilePath = path.join(__dirname, 'backup_visits.txt');

function logVisit(route) {
    const currentTime = new Date().toISOString();
    const logEntry = `${currentTime} - Visite: ${route}\n`;

    fs.appendFile(logFilePath, logEntry, (err) => {
        if (err) console.error('Erreur d\'écriture dans le fichier:', err);
        manageBackup();
    });
}

function manageBackup() {
    fs.stat(logFilePath, (err, stats) => {
        if (err) return console.error('Erreur:', err);
        if (stats.size >= 2048) { // 2 Ko
            fs.copyFile(logFilePath, backupFilePath, (err) => {
                if (err) console.error('Erreur de sauvegarde:', err);
                fs.truncate(logFilePath, 0, (err) => {
                    if (err) console.error('Erreur de vidage:', err);
                });
            });
        }
    });
}

// Appeler logVisit dans les routes
logVisit('/'); // Pour la route home
//logVisit('/help'); // Pour la route help
```
3. Serveur Angular
Pour Angular, la journalisation est gérée par un service qui envoie les logs à un serveur backend. Voici comment vous pourriez gérer cela :

Backend en Express (server.js) :

Ajoutez le code de gestion de log et de sauvegarde dans votre fichier backend qui gère les logs.
```js
const express = require('express');
const fs = require('fs');
const path = require('path');

const app = express();
const logFilePath = path.join(__dirname, 'visits.txt');
const backupFilePath = path.join(__dirname, 'backup_visits.txt');

app.use(express.json());

app.post('/log', (req, res) => {
    const logEntry = `${new Date().toISOString()} - Visite: ${req.body.route}\n`;
    fs.appendFile(logFilePath, logEntry, (err) => {
        if (err) return res.status(500).send('Erreur serveur');
        manageBackup();
        res.status(200).send('Visite journalisée');
    });
});

function manageBackup() {
    fs.stat(logFilePath, (err, stats) => {
        if (err) return console.error('Erreur:', err);
        if (stats.size >= 2048) { // 2 Ko
            fs.copyFile(logFilePath, backupFilePath, (err) => {
                if (err) console.error('Erreur de sauvegarde:', err);
                fs.truncate(logFilePath, 0, (err) => {
                    if (err) console.error('Erreur de vidage:', err);
                });
            });
        }
    });
}

const PORT = 3000;
app.listen(PORT, () => {
    console.log(`Serveur en cours d'exécution sur http://localhost:${PORT}`);
});
```

Exercice 5: Crontab

Le script doit afficher Hello World
Journaliser l'exécution du fichier 
tester avec tail -f pour voir en temps réel les logs

RESOLUTION
Étapes de réalisation
1. Créer le script
Ouvrez un terminal.
Créez un fichier de script. Par exemple, nommons-le hello_world.sh :
```bash
nano hello_world.sh
```

Ajoutez le code suivant dans le fichier :
```sh
#!/bin/bash
```

# Fichier de log
```sh
LOGFILE="/path/to/your/logfile.log"
```

# Afficher Hello World
```sh
echo "Hello World"
```

# Journaliser l'exécution avec un timestamp
```sh
echo "$(date): Hello World executed" >> $LOGFILE
```

Remplacez /path/to/your/logfile.log par le chemin où vous souhaitez enregistrer le fichier de log, par exemple /home/user/hello_world.log.
Enregistrez et fermez le fichier (dans nano, utilisez CTRL + X, puis Y et Entrée).
Rendez le script exécutable :
```sh
chmod +x hello_world.sh
```

2. Configurer crontab
Ouvrez crontab pour l'utilisateur courant :
```sh
crontab -e
```

Ajoutez la ligne suivante pour exécuter le script toutes les minutes :
```sh
* * * * * /path/to/your/hello_world.sh
```

Remplacez /path/to/your/hello_world.sh par le chemin complet de votre script.

Enregistrez et fermez l'éditeur.
3. Tester avec tail -f
Dans un terminal, exécutez la commande suivante pour suivre le fichier de log en temps réel :
```sh
tail -f /path/to/your/logfile.log
```
Vous devriez voir les entrées de log apparaître chaque minute, indiquant que le script s'exécute et affiche "Hello World".


Exercice 6: Crontab

Le script doit exécuter google.com avec curl
Journaliser l'exécution du fichier 
tester avec tail -f pour voir en temps réel les logs

RESOLUTION
1. Créer le script
Ouvrez un terminal.
Créez un fichier de script, par exemple check_google.sh :
```sh
nano check_google.sh
```

Ajoutez le code suivant dans le fichier :
```sh
#!/bin/bash
```

# Fichier de log
```sh
LOGFILE="/path/to/your/logfile.log"
```

# Exécuter curl et stocker la réponse
```sh
RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" https://www.google.com)
```

# Journaliser la date, l'heure et le code de réponse
```sh
echo "$(date): Google.com responded with status code $RESPONSE" >> $LOGFILE
```

Remplacez /path/to/your/logfile.log par le chemin où vous souhaitez enregistrer le fichier de log, par exemple /home/user/google_check.log.
Enregistrez et fermez le fichier (dans nano, utilisez CTRL + X, puis Y et Entrée).
Rendez le script exécutable :
```sh
chmod +x check_google.sh
```

2. Configurer crontab
Ouvrez crontab pour l'utilisateur courant :
```sh
crontab -e
```

Ajoutez la ligne suivante pour exécuter le script toutes les minutes :
```sh
* * * * * /path/to/your/check_google.sh
```

Remplacez /path/to/your/check_google.sh par le chemin complet de votre script.

Enregistrez et fermez l'éditeur.

3. Tester avec tail -f
Dans un terminal, exécutez la commande suivante pour suivre le fichier de log en temps réel :
```sh
tail -f /path/to/your/logfile.log
```
Vous devriez voir les entrées de log apparaître chaque minute, indiquant que le script s'exécute et que Google répond avec un code de statut.


Exercice 7: Crontab

Le script doit vérifier si le fichier server.conf existe, sinon le télécharger avec git
Journaliser l'exécution du fichier 
tester avec tail -f pour voir en temps réel les logs

RESOLUTION

Étapes de réalisation
1. Créer le script
Ouvrez un terminal.
Créez un fichier de script, par exemple check_server_conf.sh :
```sh
nano check_server_conf.sh
```
Ajoutez le code suivant dans le fichier :
```sh
#!/bin/bash
```

# Chemin du fichier à vérifier
```sh
FILE="/path/to/your/server.conf"
```
# URL du dépôt Git
```sh
REPO_URL="https://github.com/yourusername/yourrepo.git"
```
# Répertoire de destination pour le téléchargement
```sh
DEST_DIR="/path/to/your/destination"
```

# Fichier de log
```sh
LOGFILE="/path/to/your/logfile.log"
```

# Vérifier si le fichier existe
```sh
if [ ! -f "$FILE" ]; then
    echo "$(date): $FILE n'existe pas. Téléchargement avec git..." >> $LOGFILE
    # Cloner le dépôt ou mettre à jour
    if [ ! -d "$DEST_DIR" ]; then
        git clone "$REPO_URL" "$DEST_DIR"
    else
        cd "$DEST_DIR" && git pull
    fi
    # Vérifier à nouveau après le téléchargement
    if [ -f "$FILE" ]; then
        echo "$(date): $FILE a été téléchargé avec succès." >> $LOGFILE
    else
        echo "$(date): Échec du téléchargement de $FILE." >> $LOGFILE
    fi
else
    echo "$(date): $FILE existe déjà." >> $LOGFILE
fi
```
Remplacez les chemins et l'URL par vos valeurs spécifiques :
/path/to/your/server.conf : chemin du fichier à vérifier.
https://github.com/yourusername/yourrepo.git : URL de votre dépôt Git.
/path/to/your/destination : répertoire où le dépôt sera téléchargé.
/path/to/your/logfile.log : chemin où vous souhaitez enregistrer le fichier de log.
Enregistrez et fermez le fichier (dans nano, utilisez CTRL + X, puis Y et Entrée).
Rendez le script exécutable :
```sh
chmod +x check_server_conf.sh
```
2. Configurer crontab
Ouvrez crontab pour l'utilisateur courant :
```sh
crontab -e
```
Ajoutez la ligne suivante pour exécuter le script toutes les minutes :
```sh
* * * * * /check_server_conf.sh
```

Enregistrez et fermez l'éditeur.
3. Tester avec tail -f
Dans un terminal, exécutez la commande suivante pour suivre le fichier de log en temps réel :
```sh
tail -f /path/to/your/logfile.log
```
Vous devriez voir les entrées de log apparaître chaque minute, indiquant si le fichier a été téléchargé ou s'il existe déjà.


Exercice 8: Sys Admin Mailing

Le script doit envoyer les performance du serveur au sys admin
La memoire, % CPU, Stockage disponible
Envoyer au Sys Admin

RESOLUTION

Étapes de réalisation
1. Installer mail (si nécessaire)
Assurez-vous que vous avez un utilitaire de messagerie installé. Sur les systèmes basés sur Debian/Ubuntu, vous pouvez installer mailutils :

sudo apt-get install mailutils
2. Créer le script
Ouvrez un terminal.
Créez un fichier de script, par exemple send_server_performance.sh :
nano send_server_performance.sh
Ajoutez le code suivant dans le fichier :
```sh
#!/bin/bash
```

# Adresse email de l'administrateur système
```sh
SYS_ADMIN="gabrielwawina@gmail.com"
```

# Collecter les informations de performance
```sh
MEMORY_INFO=$(free -h | grep Mem | awk '{print $3 "/" $2 " (" $3/$2*100 "%)"}')
CPU_INFO=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1 "%"}')
STORAGE_INFO=$(df -h | grep '^/dev/' | awk '{ print $1 ": " $4 " disponible" }')
```

# Préparer le message
```sh
MESSAGE="Performance du serveur :
-------------------------
Mémoire : $MEMORY_INFO
CPU : $CPU_INFO
Stockage : 
$STORAGE_INFO"
```

# Envoyer l'email
```sh
echo "$MESSAGE" | mail -s "Rapport de performance du serveur" "$SYS_ADMIN"
```
Enregistrez et fermez le fichier (dans nano, utilisez CTRL + X, puis Y et Entrée).
Rendez le script exécutable :
```sh
chmod +x send_server_performance.sh
```

3. Configurer crontab pour l'exécution régulière (facultatif)
Si vous souhaitez que ce script s'exécute régulièrement, ajoutez-le à crontab :

Ouvrez crontab pour l'utilisateur courant :
```sh
crontab -e
```
Ajoutez la ligne suivante pour exécuter le script tous les jours à 8h00 :
```sh
0 8 * * * /send_server_performance.sh
```

Enregistrez et fermez l'éditeur.
4. Tester le script
Exécutez le script manuellement pour vérifier qu'il fonctionne :
```sh
/send_server_performance.sh
```
Vérifiez la boîte de réception de l'administrateur système pour voir si l'e-mail a été reçu.


Exercice 9: Backup S3

Création des instances EC2 et S3 : Vous lancez une instance EC2 pour exécuter le script de backup du fichier archivé (contenant 4 fichiers). Vous créez un bucket S3 pour stocker les backups. Et arrêter une fois que nous atteignons 4 backups.

Configuration des permissions avec IAM : créez un rôle IAM avec des permissions d'accès complet à S3 (S3 Full Access) attaché à l'instance EC2.

Création du script de backup : Vous utilisé tar pour créer des archives de nos fichiers et dossiers.

RESOLUTION

Étapes de réalisation
1. Création de l'instance EC2
Connectez-vous à AWS Management Console.
Accédez à EC2 et lancez une nouvelle instance.
Choisissez une AMI (par exemple, Amazon Linux 2).
Sélectionnez un type d'instance (t2.micro est généralement suffisant pour les tests).
Configurez les détails de l'instance (acceptez les paramètres par défaut).
Configurez le groupe de sécurité pour autoriser le port SSH (22).
Lancez l'instance.
2. Création du bucket S3
Accédez à S3 dans AWS Management Console.
Cliquez sur Create bucket.
Entrez un nom unique pour le bucket.
Choisissez la région souhaitée.
Configurez les options par défaut et créez le bucket.
3. Configuration des permissions avec IAM
Accédez à IAM dans AWS Management Console.
Cliquez sur Roles puis sur Create role.
Sélectionnez AWS service et choisissez EC2.
Cliquez sur Next: Permissions.
Recherchez AmazonS3FullAccess et sélectionnez-le.
Cliquez sur Next: Tags puis sur Next: Review.
Donnez un nom au rôle (par exemple, S3FullAccessRole), puis créez le rôle.
Attachez le rôle à votre instance EC2 :
Allez dans Instances, sélectionnez votre instance, et cliquez sur Actions > Security > Modify IAM Role.
Sélectionnez le rôle que vous venez de créer et attachez-le.
4. Création du script de backup
Connectez-vous à votre instance EC2 via SSH :

ssh -i /your-key.pem ec2-user@your-instance-public-dns
Installez aws-cli si ce n'est pas déjà fait :
```sh
sudo yum install aws-cli -y
```

Créez le script de backup :
```sh
nano backup_script.sh
```
Ajoutez le code suivant au script :
```sh
#!/bin/bash
```

# Variables
```sh
BUCKET_NAME="your-bucket-name"
BACKUP_DIR="/path/to/your/files"
ARCHIVE_NAME="backup_$(date +'%Y%m%d_%H%M%S').tar.gz"
MAX_BACKUPS=4
```

# Créer une archive des fichiers
```sh
tar -czf "$ARCHIVE_NAME" -C "$BACKUP_DIR" .
```

# Télécharger l'archive dans S3
```sh
aws s3 cp "$ARCHIVE_NAME" "s3://$BUCKET_NAME/$ARCHIVE_NAME"
```

# Compter le nombre de backups dans S3
```sh
BACKUP_COUNT=$(aws s3 ls "s3://$BUCKET_NAME/" | wc -l)
```

# Supprimer les plus anciens backups si le nombre dépasse MAX_BACKUPS
if [ "$BACKUP_COUNT" -gt "$MAX_BACKUPS" ]; then
    EXCESS_BACKUPS=$((BACKUP_COUNT - MAX_BACKUPS))
    aws s3 ls "s3://$BUCKET_NAME/" | sort | head -n "$EXCESS_BACKUPS" | awk '{print $4}' | xargs -I {} aws s3 rm "s3://$BUCKET_NAME/{}"
fi

# Nettoyer l'archive locale
rm "$ARCHIVE_NAME"
Remplacez les variables :
your-bucket-name : nom de votre bucket S3.
/path/to/your/files : chemin vers le répertoire contenant les fichiers à archiver.
Rendez le script exécutable :
chmod +x backup_script.sh
5. Planification du script avec cron
Ouvrez le crontab pour l'utilisateur courant :
crontab -e
Ajoutez une ligne pour exécuter le script, par exemple, tous les jours à 2h00 :
0 2 * * * /path/to/your/backup_script.sh
6. Tester le script
Exécutez le script manuellement pour vérifier qu'il fonctionne :
./backup_script.sh


Exercice 10: Attaque DDoS

Vous créez deux instances EC2
Vous écrivez un script sur le premier EC2 qui va faire une attaque DDoS sur le deuxième EC2.
Et au niveau du deuxième EC2, vous allez écrire un script qui va voir l’historique des performances CPU chaque seconde et va les mettre sur un fichier .csv
A la fin de l’attaque, vous faites cat du fichier .csv et notez les variations des performances CPU pour PLOTER la courbe.

RESOLUTION
Exercice : Simulation d'une Attaque DDoS sur des Instances EC2
Dans ce tutoriel, nous allons simuler une attaque par déni de service distribué (DDoS) en utilisant deux instances EC2 sur AWS. Nous écrirons un script sur une instance pour simuler l'attaque et un autre script sur la deuxième instance pour surveiller les performances du processeur (CPU). Après l'attaque, les données de performance CPU seront sauvegardées dans un fichier .csv, et nous tracerons la courbe des performances.

Prérequis
Compte AWS avec accès pour créer des instances EC2.
Connaissances de base sur AWS EC2, le scripting Shell, et Python.
Groupes de sécurité configurés pour permettre la communication entre les deux instances (ouverture des ports SSH et HTTP).
Étape 1 : Créer Deux Instances EC2
Connectez-vous à votre console AWS.
Lancez deux instances t2.micro avec l'AMI Amazon Linux 2 (ou toute autre AMI basée sur Linux).
Assurez-vous que les deux instances sont dans le même groupe de sécurité et que l'accès SSH est activé (port 22).
Configurez un groupe de sécurité pour permettre le trafic entrant entre les deux instances.

# Depuis votre machine locale, connectez-vous aux instances
ssh -i "votre-clé.pem" ec2-user@<ip-publique-première-instance>
ssh -i "votre-clé.pem" ec2-user@<ip-publique-deuxième-instance>
Étape 2 : Installer les Dépendances
Sur les deux instances EC2, mettez à jour le système et installez les outils nécessaires comme Python et stress.

# Mettre à jour le système et installer Python, stress et autres utilitaires
sudo yum update -y
sudo yum install -y python3 stress
Étape 3 : Écrire un Script d'Attaque DDoS sur la Première Instance EC2
Sur la première instance EC2, créez un script Shell qui simulera une attaque DDoS en envoyant un grand nombre de requêtes à la deuxième instance.

# Créer le script d'attaque DDoS
vim attaque_ddos.sh
Ajoutez le code suivant :

#!/bin/bash

# IP de la deuxième instance EC2
IP_CIBLE=<ip-privée-deuxième-instance>

# Simuler une attaque DDoS en envoyant continuellement des requêtes HTTP
while true
do
    curl http://$IP_CIBLE
done
Rendez le script exécutable :

chmod +x attaque_ddos.sh
Étape 4 : Écrire un Script de Surveillance CPU sur la Deuxième Instance EC2
Sur la deuxième instance EC2, écrivez un script pour surveiller l'utilisation du CPU chaque seconde et sauvegardez ces données dans un fichier .csv.

# Créer le script de surveillance CPU
vim surveiller_cpu.sh
Ajoutez le code suivant :

#!/bin/bash

# Fichier CSV pour stocker les performances CPU
fichier_sortie="performance_cpu.csv"

# Écrire l'en-tête du fichier CSV
echo "Horodatage,Utilisation_CPU" > $fichier_sortie

# Surveiller l'utilisation CPU toutes les secondes
while true
do
    horodatage=$(date +"%Y-%m-%d %H:%M:%S")
    utilisation_cpu=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1}')
    echo "$horodatage,$utilisation_cpu" >> $fichier_sortie
    sleep 1
done
Rendez le script exécutable :

chmod +x surveiller_cpu.sh
Étape 5 : Exécuter les Scripts
Lancez le script de surveillance CPU sur la deuxième instance EC2.

./surveiller_cpu.sh
Lancez l'attaque DDoS sur la première instance EC2.

./attaque_ddos.sh
Étape 6 : Arrêter l'Attaque et Analyser les Données
Après avoir fait tourner l'attaque pendant un certain temps, arrêtez les deux scripts.

# Sur la première instance EC2 (attaquant)
ctrl + C

# Sur la deuxième instance EC2 (victime)
ctrl + C
Affichez les données de performance CPU :


cat performance_cpu.csv
Étape 7 : Tracer la Courbe des Performances CPU
Copiez le fichier .csv sur votre machine locale :

scp -i "votre-clé.pem" ec2-user@<ip-publique-deuxième-instance>:/home/ec2-user/performance_cpu.csv .
Utilisez Python pour tracer la courbe d'utilisation du CPU.
python

# Tracer les performances CPU avec Python
import pandas as pd
import matplotlib.pyplot as plt

# Lire le fichier CSV
df = pd.read_csv('performance_cpu.csv')

# Convertir Horodatage en datetime
df['Horodatage'] = pd.to_datetime(df['Horodatage'])

# Tracer l'utilisation du CPU
plt.plot(df['Horodatage'], df['Utilisation_CPU'])
plt.xlabel('Temps')
plt.ylabel('Utilisation CPU (%)')
plt.title('Utilisation CPU au cours du temps pendant l\'attaque DDoS')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()


Exercice 11 : Load Balancer avec nginx

Vous lancez un VPC dans lequel vous aurez 3 subnets (un public et deux privés) et chaque subnet sera dans son availability zone (donc nous aurons aussi 3 AZ)
Dans chaque subnet, vous lancez un ec2 où vous lancez un serveur Node.
Spécialement au niveau de l'availability zone 2 (où le subnet est public), vous installez nginx (avec l'algorithme round robin) qui va faire la balance des charges.
Dans chacun des deux ec2 des réseaux privés, vous lancez aussi un serveur Node.
Vous affichez une page qui affiche Hello World dans deux ports différents : 5000 et 5001
Mais au niveau de l'ec2 du subnet public où se trouve nginx vous affichez Hello World seulement au port 5000
Au total, vous devez voir naviguer 5 sites sur votre browser (pour mieux voir, vous pouvez jouez sur la couleur du background)

Supplément : écrire un script en prenant le nom du serveur et de l'hôte et automatiser son envoi sur GitHub 

RESOLUTION

Étapes de réalisation
1. Création du VPC et des sous-réseaux
Accédez à la console AWS.
Créez un VPC :
Allez dans "VPC" > "Your VPCs" > "Create VPC".
Donnez un nom et une plage CIDR (par exemple, 10.0.0.0/16).
Créez 3 sous-réseaux :
Sous-réseau public dans la zone de disponibilité 1 (AZ1) : 10.0.1.0/24.
Sous-réseau privé dans la zone de disponibilité 2 (AZ2) : 10.0.2.0/24.
Sous-réseau privé dans la zone de disponibilité 3 (AZ3) : 10.0.3.0/24.
Créez une passerelle Internet et attachez-la à votre VPC.
Modifiez la table de routage du sous-réseau public pour permettre l'accès à Internet.
2. Lancer les instances EC2
Lancez 3 instances EC2 dans les sous-réseaux :
Dans le sous-réseau public (AZ1), lancez une instance pour Nginx.
Dans les sous-réseaux privés (AZ2 et AZ3), lancez une instance pour chaque serveur Node.js.
Configuration du groupe de sécurité :
Pour l'instance Nginx, ouvrez le port 80 (HTTP).
Pour les instances Node.js, ouvrez les ports 5000 et 5001.
3. Installer Node.js sur les instances privées
Connectez-vous à chaque instance privée :

ssh -i /path/to/your-key.pem ec2-user@your-instance-private-dns
Installez Node.js :

sudo yum update -y
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
sudo yum install -y nodejs
Créez une application Node.js :
Créez un fichier app.js avec le contenu suivant sur chaque instance :

javascript


const http = require('http');

const hostname = '0.0.0.0';
const port = process.argv[2];

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/html');
  res.end(`<h1 style="background-color: ${port === '5000' ? 'lightblue' : 'lightgreen'}">Hello World from port ${port}</h1>`);
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
Démarrez les serveurs Node.js :

node app.js 5000 &  # sur la première instance
node app.js 5001 &  # sur la deuxième instance
4. Installer Nginx sur l'instance publique
Connectez-vous à l'instance Nginx :

ssh -i /path/to/your-key.pem ec2-user@your-nginx-instance-public-dns
Installez Nginx :

sudo yum install nginx -y
Configurez Nginx pour le load balancing :
Modifiez le fichier de configuration Nginx :


sudo nano /etc/nginx/nginx.conf
Ajoutez la configuration suivante dans le bloc http :

nginx


http {
    upstream myapp {
        server <private-instance-1-ip>:5000;
        server <private-instance-2-ip>:5001;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp;
        }
    }
}
Remplacez <private-instance-1-ip> et <private-instance-2-ip> par les adresses IP de vos instances privées.

Démarrez Nginx :

sudo systemctl start nginx
5. Accéder aux applications
Accédez à l'adresse IP publique de l'instance Nginx dans votre navigateur : http://<nginx-public-ip>.
Vous devriez voir "Hello World" avec le fond bleu ou vert selon le port.
6. Script d'automatisation pour GitHub
Créez un script pour automatiser le déploiement sur GitHub :


nano deploy_to_github.sh
Ajoutez le code suivant :


#!/bin/bash

# Variables
REPO_NAME="your-repo-name"
GITHUB_URL="https://github.com/yourusername/$REPO_NAME.git"

# Initialiser le dépôt
git init
git add .
git commit -m "Initial commit"

# Ajouter le remote
git remote add origin $GITHUB_URL

# Pousser vers GitHub
git push -u origin master
Assurez-vous d'avoir configuré vos credentials GitHub en local.

7. Rendez le script exécutable :

chmod +x deploy_to_github.sh


Exercice 12 : Balance des charges partie serveur avec réplication Master - Master 

Vous lancez 3 ec2 comme précédemment : un ec2 dans un réseau public où vous aurez un proxy (qui peut être nginx) qui va faire la balance des charges sur les bases de données MySQL 

Les deux autres ec2 sont dans deux réseaux où vous allez lancez MySQL
Et entre les deux bases de données vous faites la réplication Master-Master.

RESOLUTION

Étapes de réalisation
1. Création des instances EC2
Accédez à la console AWS.
Lancez trois instances EC2 :
Instance 1 (Public) : pour le proxy de charge (Nginx).
Instance 2 (Privée) : pour le premier serveur MySQL.
Instance 3 (Privée) : pour le deuxième serveur MySQL.
2. Configuration des groupes de sécurité
Instance Nginx :
Ouvrez le port 80 (HTTP) pour les requêtes entrantes.
Ouvrez les ports 3306 pour les connexions à MySQL.
Instances MySQL :
Ouvrez le port 3306 pour permettre les connexions de l'instance Nginx.
3. Installation de MySQL sur les instances privées
Connectez-vous à chaque instance MySQL :

ssh -i /path/to/your-key.pem ec2-user@your-mysql-instance-ip
Installez MySQL :

sudo yum update -y
sudo yum install mysql-server -y
Démarrez MySQL :

sudo systemctl start mysqld
Récupérez le mot de passe temporaire :

sudo grep 'temporary password' /var/log/mysqld.log
Sécurisez l'installation de MySQL :

sudo mysql_secure_installation
4. Configuration de la réplication Master-Master
Sur la première instance MySQL :
Connectez-vous à MySQL :

mysql -u root -p
Configurez les paramètres de réplication :
sql


-- Remplacez 'your_ip' par l'adresse IP de l'instance
SET GLOBAL server_id = 1;
CREATE USER 'replicator'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'replicator'@'%';
FLUSH PRIVILEGES;
Ajoutez la configuration de réplication :
sql

CHANGE MASTER TO
    MASTER_HOST='your_second_mysql_ip',
    MASTER_USER='replicator',
    MASTER_PASSWORD='password',
    MASTER_LOG_FILE='mysql-bin.000001',
    MASTER_LOG_POS=0;
Démarrez la réplication :
sql


START SLAVE;
Sur la deuxième instance MySQL :
Connectez-vous à MySQL :

mysql -u root -p
Configurez les paramètres de réplication :
sql


SET GLOBAL server_id = 2;
CREATE USER 'replicator'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'replicator'@'%';
FLUSH PRIVILEGES;
Ajoutez la configuration de réplication :
sql


CHANGE MASTER TO
    MASTER_HOST='your_first_mysql_ip',
    MASTER_USER='replicator',
    MASTER_PASSWORD='password',
    MASTER_LOG_FILE='mysql-bin.000001',
    MASTER_LOG_POS=0;
Démarrez la réplication :

START SLAVE;
5. Installation de Nginx sur l'instance publique
Connectez-vous à l'instance Nginx :

ssh -i /path/to/your-key.pem ec2-user@your-nginx-instance-ip
Installez Nginx :

sudo yum install nginx -y
Configurez Nginx pour la balance des charges : Modifiez le fichier de configuration Nginx :

sudo nano /etc/nginx/nginx.conf
Ajoutez la configuration suivante :
nginx

http {
    upstream mysql_backend {
        server your_first_mysql_ip;
        server your_second_mysql_ip;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://mysql_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
Démarrez Nginx :

sudo systemctl start nginx
6. Test de la configuration
Accédez à l'adresse IP publique de l'instance Nginx dans votre navigateur.
Vérifiez que Nginx équilibre la charge entre vos deux bases MySQL en effectuant des requêtes.


Exercice 13 : Balance des charges partie serveur avec réplication Master - slave

Vous lancez 3 ec2 comme précédemment : un ec2 dans un réseau public où vous aurez un proxy (qui peut être nginx) qui va faire la balance des charges sur les bases de données MySQL

Les deux autres ec2 sont dans deux réseaux où vous allez lancez MySQL
Et entre les deux bases de données vous faites la réplication Master-slave.

RESOLUTION

Objectif
Configurer une architecture avec un proxy de charge (Nginx) pour équilibrer les requêtes sur une base de données MySQL en réplication Master-Slave, en utilisant trois instances EC2.

Prérequis
Compte AWS avec accès à la console.
Connaissance de base des commandes Linux et de MySQL.

Étapes de réalisation

1. Création des instances EC2
Accédez à la console AWS.
Lancez trois instances EC2 :
Instance 1 (Public) : pour le proxy de charge (Nginx).
Instance 2 (Privée) : pour le serveur MySQL Master.
Instance 3 (Privée) : pour le serveur MySQL Slave.

2. Configuration des groupes de sécurité
Instance Nginx (Public) :
Ouvrez le port 80 (HTTP) pour les requêtes entrantes.
Ouvrez le port 3306 pour les connexions MySQL.
Instances MySQL (Privées) :
Ouvrez le port 3306 pour permettre les connexions de l'instance Nginx.

3. Installation de MySQL sur les instances privées
Sur l'instance Master
Connectez-vous à l'instance Master :
bash
ssh -i /path/to/your-key.pem ec2-user@your-master-instance-ip
Installez MySQL :
bash
sudo yum update -y
sudo yum install mysql-server -y
Démarrez MySQL :
bash
sudo systemctl start mysqld
Récupérez le mot de passe temporaire :
bash
sudo grep 'temporary password' /var/log/mysqld.log
Sécurisez l'installation de MySQL :
bash
sudo mysql_secure_installation
Sur l'instance Slave
Connectez-vous à l'instance Slave :
bash
ssh -i /path/to/your-key.pem ec2-user@your-slave-instance-ip
Installez MySQL :
bash
sudo yum update -y
sudo yum install mysql-server -y
Démarrez MySQL :
bash
sudo systemctl start mysqld

4. Configuration de la réplication Master-Slave
Sur le serveur Master
Connectez-vous à MySQL :
bash
mysql -u root -p
Configurez les paramètres de réplication :
sql
SET GLOBAL server_id = 1;
CREATE USER 'replicator'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'replicator'@'%';
FLUSH PRIVILEGES;
Ajoutez la configuration de réplication :
sql
SHOW MASTER STATUS;
Notez le File et le Position retournés.
Configurez le Slave :
sql
CHANGE MASTER TO
    MASTER_HOST='your-slave-instance-ip',
    MASTER_USER='replicator',
    MASTER_PASSWORD='password',
    MASTER_LOG_FILE='mysql-bin.000001', -- Remplacez par le nom du fichier
    MASTER_LOG_POS=0; -- Remplacez par la position
Démarrez la réplication :
sql

START SLAVE;
Sur le serveur Slave
Connectez-vous à MySQL :
bash
mysql -u root -p

Configurez le serveur Slave :
sql

SET GLOBAL server_id = 2;
Vérifiez l'état de la réplication :
sql

SHOW SLAVE STATUS\G;

5. Installation de Nginx sur l'instance publique
Connectez-vous à l'instance Nginx :

ssh -i /path/to/your-key.pem ec2-user@your-nginx-instance-ip
Installez Nginx :

sudo yum install nginx -y
Configurez Nginx pour la balance des charges : Modifiez le fichier de configuration Nginx :

sudo nano /etc/nginx/nginx.conf
Ajoutez la configuration suivante dans le bloc http :
nginx


http {
    upstream mysql_backend {
        server your_master_instance_ip;
        server your_slave_instance_ip;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://mysql_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
Démarrez Nginx :

sudo systemctl start nginx

6. Test de la configuration
Accédez à l'adresse IP publique de l'instance Nginx dans votre navigateur.
Vérifiez que Nginx équilibre la charge entre votre base de données Master et Slave.


Exercice 14 : API gateway avec Lambda

Vous créez votre website avec deux pages (login et register), vous l'héberger dans S3 (S3 a un nom de domaine).
Vous automatisez son déploiement dans l'ec2 que vous aurez préalablement lancé.
Vous lancez MySQL dans l'ec2
Vous lancez aussi Secret Manager et vous y mettez les identifiants 

Ensuite vous lancez API gateway (qui aura un nom de domaine) dans deux Lambda.
Le premier lambda est pour la première page /login (avec un POST method)
Le deuxième lambda est pour la deuxième page /register (avec un POST method)

Vous écrivez un algorithme pour télécharger les identifiants au niveau de secret Manager pour se connecter à MySQL qui se trouve dans l'ec2

Veuillez à régler le problème de Cross-Origin Resource Sharing (CORS)
 
RESOLUTION
Objectif
Configurer une architecture avec Amazon S3 pour héberger un site web, une API Gateway avec des fonctions Lambda pour gérer les pages de connexion et d'inscription, ainsi que MySQL sur EC2, tout en utilisant AWS Secrets Manager pour stocker les identifiants de la base de données.

Prérequis
Compte AWS avec accès à la console.
Connaissance de base des services AWS (S3, Lambda, API Gateway, MySQL, Secrets Manager).
Étapes de réalisation
1. Création du site web et hébergement dans S3
Créez un bucket S3 :
Accédez à S3 et créez un nouveau bucket avec un nom unique (par exemple, my-website-bucket).
Activez l'hébergement de site statique dans les paramètres du bucket.
Créez les fichiers HTML :
login.html :

html

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>
    <h1>Login</h1>
    <form id="loginForm">
        <input type="text" id="username" placeholder="Username" required>
        <input type="password" id="password" placeholder="Password" required>
        <button type="submit">Login</button>
    </form>
    <script>
        document.getElementById('loginForm').onsubmit = async (e) => {
            e.preventDefault();
            const response = await fetch('https://your-api-id.execute-api.region.amazonaws.com/prod/login', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    username: document.getElementById('username').value,
                    password: document.getElementById('password').value
                })
            });
            const data = await response.json();
            alert(data.message);
        };
    </script>
</body>
</html>

register.html :
html

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Register</title>
</head>
<body>
    <h1>Register</h1>
    <form id="registerForm">
        <input type="text" id="username" placeholder="Username" required>
        <input type="password" id="password" placeholder="Password" required>
        <button type="submit">Register</button>
    </form>
    <script>
        document.getElementById('registerForm').onsubmit = async (e) => {
            e.preventDefault();
            const response = await fetch('https://your-api-id.execute-api.region.amazonaws.com/prod/register', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    username: document.getElementById('username').value,
                    password: document.getElementById('password').value
                })
            });
            const data = await response.json();
            alert(data.message);
        };
    </script>
</body>
</html>

Téléchargez les fichiers sur S3 :
Accédez à votre bucket S3 et téléchargez login.html et register.html.
Rendez le bucket public :
Modifiez la politique de votre bucket pour permettre l'accès public aux fichiers.
2. Lancer MySQL sur EC2
Lancez une instance EC2 :
Installez MySQL en suivant les étapes habituelles.
Configurez les groupes de sécurité pour autoriser les connexions sur le port 3306.
Sécurisez MySQL :
Configurez un utilisateur et un mot de passe.
3. Stocker les identifiants dans Secrets Manager
Accédez à Secrets Manager :
Créez un nouveau secret pour stocker les identifiants de MySQL (username et password).
4. Créer les fonctions Lambda
Accédez à AWS Lambda :
Créez deux nouvelles fonctions Lambda : loginFunction et registerFunction.
Ajoutez le code pour la fonction loginFunction :
javascript

const AWS = require('aws-sdk');
const mysql = require('mysql');

exports.handler = async (event) => {
    const secretName = "your_secret_name";
    const region = "your_region";

    const client = new AWS.SecretsManager({
        region: region
    });

    let secret;
    try {
        const data = await client.getSecretValue({ SecretId: secretName }).promise();
        secret = JSON.parse(data.SecretString);
    } catch (err) {
        return { statusCode: 500, body: "Error retrieving secret" };
    }

    const connection = mysql.createConnection({
        host: 'your-ec2-instance-ip',
        user: secret.username,
        password: secret.password,
        database: 'your_database'
    });

    const { username, password } = JSON.parse(event.body);

    return new Promise((resolve, reject) => {
        connection.query('SELECT * FROM users WHERE username = ? AND password = ?', [username, password], (error, results) => {
            if (error) {
                reject(error);
            } else {
                resolve({
                    statusCode: 200,
                    body: JSON.stringify({ message: results.length > 0 ? "Login successful" : "Invalid credentials" })
                });
            }
        });
    });
};
Ajoutez le code pour la fonction registerFunction :
javascript

const AWS = require('aws-sdk');
const mysql = require('mysql');

exports.handler = async (event) => {
    const secretName = "your_secret_name";
    const region = "your_region";

    const client = new AWS.SecretsManager({
        region: region
    });

    let secret;
    try {
        const data = await client.getSecretValue({ SecretId: secretName }).promise();
        secret = JSON.parse(data.SecretString);
    } catch (err) {
        return { statusCode: 500, body: "Error retrieving secret" };
    }

    const connection = mysql.createConnection({
        host: 'your-ec2-instance-ip',
        user: secret.username,
        password: secret.password,
        database: 'your_database'
    });

    const { username, password } = JSON.parse(event.body);

    return new Promise((resolve, reject) => {
        connection.query('INSERT INTO users (username, password) VALUES (?, ?)', [username, password], (error, results) => {
            if (error) {
                reject(error);
            } else {
                resolve({
                    statusCode: 200,
                    body: JSON.stringify({ message: "Registration successful" })
                });
            }
        });
    });
};
5. Créer l'API Gateway
Accédez à API Gateway :
Créez une nouvelle API REST.
Créez les ressources :
Créez deux ressources /login et /register.
Configurez les méthodes :
Pour /login, configurez une méthode POST et liez-la à loginFunction.
Pour /register, configurez une méthode POST et liez-la à registerFunction.
Activer CORS :
Pour chaque méthode, activez CORS et configurez les en-têtes appropriés :
plaintext

Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Content-Type
Déployez l'API :
Créez un nouvel environnement de déploiement (par exemple, prod).
6. Tester l’application
Accédez à votre site web :
Ouvrez http://your-bucket-name.s3-website-region.amazonaws.com/login.html pour tester la page de connexion.
Ouvrez http://your-bucket-name.s3-website-region.amazonaws.com/register.html pour tester la page d'inscription.
Vérifiez les connexions :
Testez les fonctionnalités de connexion et d'inscription pour vous assurer que tout fonctionne correctement.


Exercice 15 : Application Load Balancer (ALB)

Vous lancez un Application Load Balancer (ALB) 
Vous devez déployer Deux target groups (chacun avec son auto scale) et vous devez rédiriger le traffic par rapport aux paramètres de l'URL.
Vous avez deux paramètres dans URL : /user et /admin
Nodejs avec express

RESOLUTION

Objectif
Configurer un Application Load Balancer (ALB) avec deux groupes de cibles (target groups) pour rediriger le trafic en fonction des paramètres d'URL /user et /admin. Chaque groupe de cibles aura son propre groupe d'auto-scaling.

Prérequis
Compte AWS avec accès à la console.
Connaissance de base de Node.js et Express.
Connaissance des concepts AWS tels que ALB, Auto Scaling, et EC2.
Étapes de réalisation
1. Créer les instances EC2 avec Node.js et Express
A. Créer une application Node.js
Lancez deux instances EC2 (dans la même zone de disponibilité ou dans des zones différentes pour la redondance).
Connectez-vous à chaque instance EC2 :
bash
ssh -i /path/to/your-key.pem ec2-user@your-instance-ip
Installez Node.js et npm :
bash

sudo yum update -y
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
sudo yum install -y nodejs
Créez un répertoire pour votre application :
bash
mkdir my-app
cd my-app
Initialisez un projet Node.js :
bash

npm init -y
Installez Express :
bash

npm install express
Créez un fichier app.js :
javascript
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/user', (req, res) => {
    res.send('<h1>User Page</h1>');
});

app.get('/admin', (req, res) => {
    res.send('<h1>Admin Page</h1>');
});

app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});
Démarrez votre application :
bash

node app.js &
2. Créer un Application Load Balancer (ALB)
Accédez à la console AWS et allez dans le service EC2.
Créez un ALB :
Cliquez sur "Load Balancers" dans le menu de gauche et sélectionnez "Create Load Balancer".
Choisissez "Application Load Balancer".
Configurez les paramètres suivants :
Nom : my-alb
Schéma : Internet-facing
VPC : Sélectionnez votre VPC
Availability Zones : Sélectionnez les zones appropriées.
Configurer les groupes cibles :
Créez deux groupes de cibles :
Groupe 1 (User Target Group) :
Nom : user-target-group
Protocole : HTTP
Port : 3000
Groupe 2 (Admin Target Group) :
Nom : admin-target-group
Protocole : HTTP
Port : 3000
3. Configurer les règles d'écoute
Configurer les règles d'écoute de l'ALB :
Ajoutez un écouteur pour le port 80 (HTTP).
Configurez les règles suivantes :
Règle 1 : Si l'URL contient /user, redirigez vers le user-target-group.
Règle 2 : Si l'URL contient /admin, redirigez vers le admin-target-group.
4. Configurer l'Auto Scaling
Créer deux groupes d'auto-scaling :
Accédez à la section "Auto Scaling Groups" dans la console EC2.
Créez un groupe d'auto-scaling pour chaque groupe cible.
Configurer le groupe d'auto-scaling pour user-target-group :
Sélectionnez le modèle de lancement approprié.
Configurez la taille minimale, maximale et souhaitée.
Assurez-vous que le groupe est associé au user-target-group.
Configurer le groupe d'auto-scaling pour admin-target-group :
Répétez les mêmes étapes que ci-dessus, mais associez-le au admin-target-group.
5. Test de l'ALB
Récupérez l'adresse DNS de l'ALB dans la console AWS.
Testez les URL suivantes :
Accédez à http://your-alb-dns/user pour vérifier la page utilisateur.
Accédez à http://your-alb-dns/admin pour vérifier la page admin.


Exercice 16 : Application Load Balancer (ALB)

Application Load Balancer (ALB)
Vous mettez un auto scale qui sera attaché à un target group
Votre instance doit écouter le port 5000 (un site Nodejs express)
Et l'auto Scale parle à l'ALB au port 80

RESOLUTION
Objectif
Configurer un Application Load Balancer (ALB) avec un groupe d'auto-scaling qui redirige le trafic vers une application Node.js utilisant Express, écoutant sur le port 5000. L'ALB écoutera sur le port 80.

Prérequis
Compte AWS avec accès à la console.
Connaissance de base de Node.js et Express.
Connaissance des concepts AWS tels que ALB, Auto Scaling, et EC2.
Étapes de réalisation
1. Créer une application Node.js
A. Lancer une instance EC2
Lancez une instance EC2 :
Choisissez une Amazon Machine Image (AMI) appropriée (par exemple, Amazon Linux 2).
Configurez le type d'instance et les paramètres de réseau.
N'oubliez pas de créer un groupe de sécurité qui autorise le trafic sur les ports 5000 et 80.
B. Installer Node.js et Express
Connectez-vous à l'instance EC2 :
bash

ssh -i /path/to/your-key.pem ec2-user@your-instance-ip
Installez Node.js et npm :
bash

sudo yum update -y
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
sudo yum install -y nodejs
Créez un répertoire pour votre application :
bash

mkdir my-node-app
cd my-node-app
Initialisez un projet Node.js :
bash

npm init -y
Installez Express :
bash

npm install express
Créez un fichier app.js :
javascript

const express = require('express');
const app = express();
const port = 5000;

app.get('/', (req, res) => {
    res.send('<h1>Hello from Node.js on port 5000!</h1>');
});

app.listen(port, () => {
    console.log(`Server is running on http://localhost:${port}`);
});
Démarrez votre application :
bash

node app.js &
2. Configurer un Application Load Balancer (ALB)
Accédez à la console AWS et allez dans le service EC2.
Créer un ALB :
Cliquez sur "Load Balancers" dans le menu de gauche et sélectionnez "Create Load Balancer".
Choisissez "Application Load Balancer".
Configurez les paramètres suivants :
Nom : my-alb
Schéma : Internet-facing
VPC : Sélectionnez votre VPC
Availability Zones : Sélectionnez les zones appropriées.
Configurer les groupes cibles :
Créez un groupe de cibles :
Nom : my-target-group
Protocole : HTTP
Port : 5000
Health check path : /
3. Configurer les règles d'écoute
Configurer l'écouteur de l'ALB :
Ajoutez un écouteur pour le port 80 (HTTP).
Configurez le groupe cible pour rediriger vers my-target-group.
4. Configurer l'Auto Scaling
Créer un groupe d'auto-scaling :
Accédez à la section "Auto Scaling Groups" dans la console EC2.
Cliquez sur "Create Auto Scaling group".
Sélectionnez un modèle de lancement qui utilise l'AMI de votre instance.
Configurez les paramètres de votre groupe d'auto-scaling :
Nom : my-auto-scaling-group
VPC : Sélectionnez votre VPC
Availability Zones : Choisissez les zones de disponibilité appropriées.
Configurer les paramètres de scaling :
Définissez la taille minimale, maximale et souhaitée du groupe.
Assurez-vous de l'associer au groupe cible my-target-group.
5. Tester l'application
Récupérez l'adresse DNS de l'ALB dans la console AWS.
Accédez à l'ALB :
Ouvrez votre navigateur et accédez à http://your-alb-dns pour tester l'application.


Exercice 17 : Firewall (Pare-feu)

Vous créez un firewall que vous allez customiser pour surveiller les traffics entrant et sortant en bloquant :
- les ports
- les attaques de déni de service (DoS) : HTTP flood, Application layer attacks, Ping flood, Volumetric attacks, Protocol attacks, Smurf, Slowloris, NTP amplification, Buffer overflow, Teardrop, Misused application attack, Distributed denial of service attacks (DDoS), Ping of Death, UDP attacks
- etc.

RESOLUTION

Dans cet exercice, nous allons créer un pare-feu (firewall) et le personnaliser pour surveiller le trafic entrant et sortant. L'objectif est de bloquer les ports et les types d'attaques comme les attaques par déni de service (DoS et DDoS) ainsi que d'autres menaces spécifiques.

Prérequis
Serveur Linux ou une instance EC2 sur AWS avec les bonnes permissions.
Connaissance de base sur le réseau, le firewall (ex : iptables, ufw), et la gestion des ports.
Accès root ou administrateur pour gérer les règles du pare-feu.
Concepts Clés
Attaque par déni de service (DoS) : surcharge d'un serveur en inondant celui-ci de requêtes, empêchant un fonctionnement normal.
DDoS : attaque DoS distribuée via plusieurs machines.
Filtres basés sur les ports : bloquer l'accès à des ports spécifiques pour éviter l'exposition de certains services.
Étape 1 : Configuration de Base du Pare-feu
Dans cet exemple, nous utiliserons iptables, qui est une solution robuste pour créer des pare-feu sur Linux. Vous pouvez aussi utiliser ufw pour une configuration plus simple si nécessaire.

Installer iptables (si non installé) :
bash
# Mise à jour du système
sudo apt update

# Installation d'iptables
sudo apt install iptables
Affichage des règles par défaut :
bash
sudo iptables -L
Étape 2 : Bloquer les Ports Inutilisés
Pour empêcher les attaques via des ports non sécurisés ou non utilisés, nous allons bloquer tous les ports et n'autoriser que ceux qui sont nécessaires.

Réinitialiser les règles par défaut pour s'assurer qu'il n'y a pas de règles conflictuelles :
bash
# Réinitialiser les règles par défaut
sudo iptables -F
Bloquer tous les ports par défaut (stratégie "deny all") :
bash
# Bloquer toutes les connexions entrantes par défaut
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
Autoriser uniquement certains ports comme le port 22 pour SSH et le port 80 pour HTTP :
bash
# Autoriser SSH (port 22)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Autoriser HTTP (port 80)
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Autoriser HTTPS (port 443)
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
Étape 3 : Bloquer les Attaques de Type DoS/DDoS
Protection contre les attaques SYN Flood (inondation de requêtes SYN) :
bash
# Bloquer les attaques SYN Flood
sudo iptables -A INPUT -p tcp ! --syn -m state --state NEW -j DROP
sudo iptables -A INPUT -p tcp --tcp-flags ALL SYN,ACK -m limit --limit 1/s -j ACCEPT
Limiter les connexions HTTP pour éviter les attaques par HTTP flood :
bash
# Limiter le nombre de connexions HTTP pour éviter un flood
sudo iptables -A INPUT -p tcp --dport 80 -m connlimit --connlimit-above 50 -j DROP
Bloquer les attaques Ping Flood (inondation ICMP) :
bash
# Limiter le ping pour éviter un Ping Flood
sudo iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/s -j ACCEPT
Protection contre les attaques Slowloris :
bash
# Limiter le nombre de connexions TCP simultanées à un serveur
sudo iptables -A INPUT -p tcp --syn -m connlimit --connlimit-above 10 --connlimit-mask 32 -j DROP
Prévenir les attaques de type UDP Flood :
bash
# Bloquer les inondations UDP
sudo iptables -A INPUT -p udp --dport 80 -m limit --limit 10/s -j ACCEPT
Étape 4 : Protection Contre d'Autres Types d'Attaques
Protection contre le Ping of Death :
bash
# Bloquer les paquets ICMP de taille anormale
sudo iptables -A INPUT -p icmp --icmp-type echo-request -m length --length 1000:65535 -j DROP
Attaque NTP amplification (exploitation des serveurs NTP) :
bash
# Bloquer les paquets NTP inhabituels
sudo iptables -A INPUT -p udp --sport 123 -m u32 --u32 "0>>22&0x3C@12&0xFFFFFF00=0x17000100" -j DROP
Buffer Overflow :
bash
# Bloquer les paquets TCP/IP trop volumineux
sudo iptables -A INPUT -p tcp -m length --length 65535 -j DROP
Teardrop Attack (envoi de paquets fragmentés incorrects) :
bash
# Bloquer les fragments suspects
sudo iptables -A INPUT -f -j DROP
Étape 5 : Surveiller le Trafic avec des Logs
Ajouter des règles de logging pour surveiller le trafic suspect :
bash
# Loguer les paquets suspects
sudo iptables -A INPUT -p tcp --dport 80 -m connlimit --connlimit-above 50 -j LOG --log-prefix "HTTP Flood: "

# Loguer les paquets bloqués
sudo iptables -A INPUT -j LOG --log-prefix "Blocked: "
Vérifier les logs pour examiner les activités bloquées :
bash
# Voir les logs des activités
sudo tail -f /var/log/syslog
Étape 6 : Sauvegarder et Restaurer les Règles
Sauvegarder les règles actuelles :
bash
# Sauvegarder les règles iptables
sudo iptables-save > /etc/iptables/rules.v4
Restaurer les règles après un redémarrage :
bash
# Restaurer les règles iptables au démarrage
sudo iptables-restore < /etc/iptables/rules.v4
Étape 7 : Mettre à Jour sur GitHub
Initialiser un dépôt Git local :
bash
git init
git add .
git commit -m "Ajout du tutoriel pare-feu personnalisé"
Pousser le dépôt vers GitHub :
bash
git remote add origin <url-de-votre-repo>
git push -u origin main


Exercice  18 : Web scraping

Créer une base des données 
Vous faites un web scraping qui va prendre les données et les enregistrer dans votre base des données 
Si les données existent déjà, vous n'enregistrez plus.
Si ce sont les nouvelles données, vous enregistrez et vous envoyez le message par mail 

RESOLUTION

Objectif
Créer une base de données pour enregistrer des données récupérées par web scraping. Si les données n'existent pas, les enregistrer et envoyer un message par email.

Prérequis
Python installé (version 3.x).
Bibliothèques Python : requests, BeautifulSoup, sqlite3 (ou un autre SGBD), smtplib.
Accès à une base de données (SQLite pour simplifier l'exemple).
Compte de messagerie pour l'envoi d'emails.
Étapes de réalisation
1. Créer la base de données
A. Utiliser SQLite pour la simplicité
Créez un fichier Python pour la base de données :
python
import sqlite3

# Connexion à la base de données (ou création si elle n'existe pas)
conn = sqlite3.connect('scraping_data.db')
cursor = conn.cursor()

# Créer une table pour les données
cursor.execute('''
CREATE TABLE IF NOT EXISTS data (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT UNIQUE,
    content TEXT
)
''')

# Sauvegarder les modifications et fermer la connexion
conn.commit()
conn.close()
2. Web Scraping
A. Écrire une fonction pour le scraping
Installez les bibliothèques nécessaires :
bash
pip install requests beautifulsoup4
Écrivez la fonction de scraping :
python

import requests
from bs4 import BeautifulSoup

def scrape_data(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')

    # Exemple d'extraction : changez en fonction de la structure de la page
    titles = soup.find_all('h2')  # Changez le sélecteur selon vos besoins
    return [(title.text, title.find_next('p').text) for title in titles]
3. Enregistrer les données dans la base de données
Écrire la fonction pour enregistrer les données :
python
def save_data(data):
    conn = sqlite3.connect('scraping_data.db')
    cursor = conn.cursor()

    for title, content in data:
        try:
            cursor.execute('INSERT INTO data (title, content) VALUES (?, ?)', (title, content))
            conn.commit()
            send_email(title, content)  # Appelle la fonction d'envoi d'email
        except sqlite3.IntegrityError:
            print(f"Le titre '{title}' existe déjà. Pas d'enregistrement.")
    
    conn.close()
4. Envoyer un email
A. Configurer l'envoi d'email
Écrire la fonction d'envoi d'email :
python
import smtplib
from email.mime.text import MIMEText

def send_email(title, content):
    sender = 'your_email@example.com'
    receiver = 'recipient@example.com'
    subject = f'Nouveau contenu ajouté : {title}'
    body = f'Contenu : {content}'

    msg = MIMEText(body)
    msg['Subject'] = subject
    msg['From'] = sender
    msg['To'] = receiver

    try:
        with smtplib.SMTP('smtp.example.com', 587) as server:
            server.starttls()
            server.login(sender, 'your_password')
            server.sendmail(sender, receiver, msg.as_string())
        print(f"Email envoyé pour le titre : {title}")
    except Exception as e:
        print(f"Erreur lors de l'envoi de l'email : {e}")
5. Mettre tout ensemble
Écrire le script principal :
python

if __name__ == "__main__":
    url = 'http://example.com'  # Remplacez par l'URL cible
    scraped_data = scrape_data(url)
    save_data(scraped_data)
6. Exécution et tests
Exécutez le script Python et vérifiez que les données sont correctement extraites, enregistrées dans la base de données, et que les emails sont envoyés pour les nouvelles entrées.


Exercice  19 : web scraping sur l'or

Faites du web scraping sur les données qui peuvent impacter la chute ou la hausse de l'or.
Et vous envoyez le message par mail en précisant l'heure et la date de l'information obtenue

RESOLUTION

Objectif
Effectuer du web scraping pour collecter des données susceptibles d'influencer le prix de l'or, puis envoyer un email contenant l'heure et la date de l'information obtenue.

Prérequis
Python installé (version 3.x).
Bibliothèques Python : requests, BeautifulSoup, smtplib.
Accès à une base de données (facultatif) si vous souhaitez stocker les données.
Compte de messagerie pour l'envoi d'emails.
Étapes de réalisation
1. Installer les bibliothèques nécessaires
Exécutez la commande suivante pour installer les bibliothèques requises :

bash
pip install requests beautifulsoup4
2. Écrire une fonction de scraping
Voici un exemple de code pour scraper un site qui fournit des informations sur l'or. Remplacez l'URL par un site pertinent.

python
import requests
from bs4 import BeautifulSoup
from datetime import datetime

def scrape_gold_data(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')

    # Exemple d'extraction : adaptez selon la structure de la page
    # Supposons que l'information sur l'or est dans une balise <div> avec la classe "gold-info"
    gold_info = soup.find('div', class_='gold-info')

    if gold_info:
        return gold_info.text.strip()
    return None
3. Envoyer un email avec les informations obtenues
Écrivez une fonction pour envoyer un email contenant les informations obtenues :

python
import smtplib
from email.mime.text import MIMEText

def send_email(gold_data):
    sender = 'your_email@example.com'
    receiver = 'recipient@example.com'
    subject = 'Mise à jour sur le prix de l\'or'
    timestamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    body = f'Données recueillies sur l\'or : {gold_data}\n\nHeure de l\'information : {timestamp}'

    msg = MIMEText(body)
    msg['Subject'] = subject
    msg['From'] = sender
    msg['To'] = receiver

    try:
        with smtplib.SMTP('smtp.example.com', 587) as server:
            server.starttls()
            server.login(sender, 'your_password')
            server.sendmail(sender, receiver, msg.as_string())
        print(f"Email envoyé avec succès à {receiver}.")
    except Exception as e:
        print(f"Erreur lors de l'envoi de l'email : {e}")
4. Mettre tout ensemble
Écrivez le script principal qui coordonne le scraping et l'envoi d'email :

python
if __name__ == "__main__":
    url = 'http://example.com/gold-prices'  # Remplacez par l'URL pertinente
    gold_data = scrape_gold_data(url)

    if gold_data:
        send_email(gold_data)
    else:
        print("Aucune donnée trouvée sur le prix de l'or.")
5. Exécution et tests
Exécutez le script Python pour tester la fonctionnalité.
Vérifiez que les données sont correctement extraites et que l'email est envoyé avec l'heure et la date de l'information.


Exercice 20 : Gestion des Privilèges d'utilisateur dans une Base de Données


Vous êtes un administrateur de base des données et vous créez les tables RESET, EMPLOYES, DEPARTMENTS, TÂCHES. 

Vous implémentez les tables à votre convenance mais tenez à ce que la table EMPLOYES ait la colonne PASSWORD 

En tant qu'administrateur, veuillez à ce qu'un nouvel utilisateur Victoria (amie de lynx) ait les restrictions suivantes :
- Ne pas voir la table RESET
- Pouvoir Voir la liste des employés mais pas la colonne PASSWORD 
- Pouvoir lister tous les départements mais pas INSERT, ni UPDATE 
- Pouvoir lister et insérer dans la table TACHES, mais pas UPDATE, ni DELETE

RESOLUTION

Objectif
Créer des tables dans une base de données et gérer les privilèges d'un utilisateur nommé Victoria, tout en respectant les restrictions spécifiées.

Étapes de réalisation
1. Création des tables
Nous allons créer quatre tables : RESET, EMPLOYES, DEPARTMENTS, et TACHES. Voici un exemple de script SQL pour créer ces tables :

sql
CREATE TABLE RESET (
    id INT PRIMARY KEY AUTO_INCREMENT,
    reset_code VARCHAR(255) NOT NULL
);

CREATE TABLE EMPLOYES (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);

CREATE TABLE DEPARTMENTS (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL
);

CREATE TABLE TACHES (
    id INT PRIMARY KEY AUTO_INCREMENT,
    description VARCHAR(255) NOT NULL,
    employe_id INT,
    FOREIGN KEY (employe_id) REFERENCES EMPLOYES(id)
);
2. Création de l'utilisateur Victoria
Ensuite, nous créerons l'utilisateur Victoria et définirons ses privilèges.

sql
-- Création de l'utilisateur
CREATE USER 'victoria'@'localhost' IDENTIFIED BY 'password123';

-- Accordez les privilèges nécessaires
GRANT SELECT ON EMPLOYES TO 'victoria'@'localhost';
GRANT SELECT (nom, email) ON EMPLOYES TO 'victoria'@'localhost';  -- Pas de PASSWORD
GRANT SELECT ON DEPARTMENTS TO 'victoria'@'localhost';
GRANT INSERT ON TACHES TO 'victoria'@'localhost';
GRANT SELECT, INSERT ON TACHES TO 'victoria'@'localhost';  -- Pour TACHES seulement
3. Restriction des privilèges
Pour restreindre l'accès de Victoria aux tables et colonnes spécifiques, nous allons utiliser des commandes SQL pour gérer les privilèges :

sql
-- Restreindre l'accès à la table RESET
REVOKE ALL PRIVILEGES ON RESET FROM 'victoria'@'localhost';

-- Restreindre les privilèges d'UPDATE et DELETE sur TACHES
REVOKE UPDATE, DELETE ON TACHES FROM 'victoria'@'localhost';

-- S'assurer que Victoria ne peut pas voir la colonne PASSWORD
REVOKE SELECT(password) ON EMPLOYES FROM 'victoria'@'localhost';
4. Vérification des privilèges
Pour vérifier les privilèges de l'utilisateur Victoria, exécutez la commande suivante :

sql
SHOW GRANTS FOR 'victoria'@'localhost';


Exercice 21 : Mise en Place d’un Compteur de Visites sur un Site Web Hébergé dans une Instance EC2
Dans cet exercice, vous allez configurer un compteur de visites sur un site web hébergé sur une instance EC2. Chaque visite sera enregistrée et affichée sur la page d'accueil du site, permettant de suivre le nombre total de visiteurs. Vous utiliserez un simple serveur web, un langage de script comme PHP ou Python, et une base de données ou un fichier pour stocker le nombre de visites.

RESOLUTION

Objectif
Configurer un compteur de visites sur un site web hébergé sur une instance EC2. Chaque visite sera enregistrée et affichée sur la page d'accueil du site.

Prérequis
Compte AWS avec accès à EC2.
Une instance EC2 configurée (par exemple, avec Amazon Linux ou Ubuntu).
Serveur web installé (Apache ou Nginx).
Langage de script (PHP ou Python) installé.

Étapes de réalisation
1. Lancer une Instance EC2
Connectez-vous à votre compte AWS et lancez une instance EC2.
Sélectionnez une AMI (par exemple, Amazon Linux 2 ou Ubuntu).
Configurez le groupe de sécurité pour autoriser le trafic HTTP (port 80).
Connectez-vous à l'instance via SSH.
2. Installer un Serveur Web
Pour cet exemple, nous allons utiliser Apache avec PHP.

A. Installer Apache et PHP
Pour une instance Amazon Linux 2 :

bash
sudo yum update -y
sudo yum install httpd php -y
Pour une instance Ubuntu :

bash
sudo apt update
sudo apt install apache2 php libapache2-mod-php -y
B. Démarrer le serveur Apache

bash
sudo systemctl start httpd  # Pour Amazon Linux
# ou
sudo systemctl start apache2  # Pour Ubuntu

3. Créer le Compteur de Visites
A. Créer un fichier PHP
Accédez au répertoire racine de votre serveur web :
bash
cd /var/www/html
Créer un fichier index.php :

php
<?php
// Fichier pour stocker le nombre de visites
$counterFile = 'counter.txt';

// Vérifiez si le fichier existe, sinon créez-le

if (!file_exists($counterFile)) {
    file_put_contents($counterFile, 0);
}

// Lire le nombre actuel de visites
$count = (int)file_get_contents($counterFile);

// Incrémentez le compteur
$count++;

// Écrire le nouveau nombre de visites dans le fichier
file_put_contents($counterFile, $count);

// Afficher le nombre de visites
echo "<h1>Bienvenue sur mon site web !</h1>";
echo "<p>Nombre de visites : $count</p>";
?>

4. Configurer les Permissions
Assurez-vous que le fichier counter.txt est accessible en écriture par le serveur web.

bash
sudo touch counter.txt
sudo chmod 666 counter.txt

5. Tester le Compteur de Visites
Accédez à l'adresse IP publique de votre instance EC2 dans un navigateur (par exemple, http://your-ec2-public-ip/index.php).
Rafraîchissez la page pour voir le compteur de visites augmenter.

6. (Optionnel) Configurer un Nom de Domaine
Si vous avez un nom de domaine, vous pouvez le configurer pour pointer vers l'adresse IP de votre instance EC2. Cela vous permettra d'accéder à votre site via un nom de domaine convivial.


Exercice 22 : Déploiement d'une Infrastructure avec du Code Utilisant CloudFormation (2 EC2 et un Bucket S3)
Dans cet exercice, vous allez automatiser le déploiement d'une infrastructure sur AWS en utilisant CloudFormation. Cette infrastructure inclura deux instances EC2 et un bucket S3. Grâce à CloudFormation, vous pouvez définir et provisionner ces ressources via des fichiers YAML ou JSON, permettant une gestion de l'infrastructure en tant que code (IaC).

RESOLUTION

Objectif
Automatiser le déploiement d'une infrastructure sur AWS comprenant deux instances EC2 et un bucket S3, en utilisant AWS CloudFormation.

Étapes de réalisation

1. Créer un fichier CloudFormation
Nous allons créer un fichier YAML qui définira notre infrastructure. Ce fichier inclura deux instances EC2 et un bucket S3.

A. Créer le fichier infrastructure.yaml
yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Infrastructure avec 2 instances EC2 et un bucket S3

Resources:

  MyS3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: !Sub 'my-s3-bucket-${AWS::AccountId}'

  MyEC2Instance1:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0  # Remplacez par une AMI valide dans votre région
      KeyName: my-key-pair  # Remplacez par le nom de votre paire de clés
      SecurityGroupIds:
        - !Ref InstanceSecurityGroup

  MyEC2Instance2:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0  # Remplacez par une AMI valide dans votre région
      KeyName: my-key-pair  # Remplacez par le nom de votre paire de clés
      SecurityGroupIds:
        - !Ref InstanceSecurityGroup

  InstanceSecurityGroup:
    Type: 'AWS::EC2::SecurityGroup'
    Properties:
      GroupDescription: 'Allow SSH and HTTP'
      VpcId: !Ref MyVPC  # Remplacez par l'ID de votre VPC

  MyVPC:
    Type: 'AWS::EC2::VPC'
    Properties:
      CidrBlock: '10.0.0.0/16'
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: MyVPC

Outputs:
  S3BucketURL:
    Description: "URL du bucket S3"
    Value: !Sub 'http://${MyS3Bucket}.s3.amazonaws.com/'
2. Déployer le modèle CloudFormation
Pour déployer votre modèle CloudFormation, vous pouvez utiliser la console AWS ou AWS CLI.

A. Utiliser AWS CLI
Ouvrez votre terminal.

Exécutez la commande suivante pour créer la pile :
bash
aws cloudformation create-stack --stack-name MyInfrastructureStack --template-body file://infrastructure.yaml --capabilities CAPABILITY_NAMED_IAM

3. Vérifier le déploiement
Accédez à la console AWS CloudFormation.
Sélectionnez la pile que vous venez de créer pour suivre l'état de son déploiement.
Une fois le déploiement terminé, vous devriez voir vos deux instances EC2 et votre bucket S3 créés.

4. Tester l'infrastructure
Vérifiez le bucket S3 pour vous assurer qu'il a été créé avec succès.
Accédez à vos instances EC2 pour vérifier qu'elles fonctionnent correctement.

