# THE NINTH AGE

**Campagne de tests fonctionnels automatisés en Robot Framework Selenium pour le site web : [The Ninth Age](https://www.the-ninth-age.com/)**

## INSTALLATION DU PROJET EN LOCAL

Python >= 3.8 doit être installé au préalable sur votre machine.

1. **Configurer Visual Studio Code**

    Vérifier que votre IDE VS Studio Code dispose bien des 2 plugins ci-dessous pour pouvoir exécuter des fichiers robots.

    [VS Code plugin Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)

    [VS Code plugin Robocorp](https://marketplace.visualstudio.com/items?itemName=robocorp.robotframework-lsp)


1. **Répertoire de travail**

    Créer un répertoire dans %LOCALAPPDATA% qui sera votre répertoire de travail pour vos tests et dans lequel vous pourrez installer tous vos différents projets en plus de T9A. Dans mon cas il s'agira du répertoire **TEST_FACTORY**.

    `%LOCALAPPDATA%\TEST_FACTORY`

    > Dans ce répertoire nous configurerons les répertoires et fichiers suivants :

    `bin` | contiendra les bins chromedriver.exe et geckodriver.exe

    `jdk-11.0.16.1+1` | client de java qui permettra l'exécution de l'ordonnanceur Jenkins.

    `JENKINS` | client de Jenkins.

    `test_projects` | répertoire qui contient nos projets dont T9A.

    `JENKINS_CONFIG_JOB.bat` | fichier de configurations des jobs dans JENKINS.

    `JENKINS_LANCEUR.bat` | fichier pour lancer Jenkins. 


3. **Récupération du projet**

    Cloner le [projet_T9A](https://github.com/PHGL666/T9A_TNR) sur votre machine dans le répertoire **%LOCALAPPDATA%\TEST_FACTORY\test_projects\T9A**


4. **Création de l’environnement virtuel python**

    A la racine du projet \T9A installer l'environnement virtuel python afin de pouvoir exécuter nos tests dans un environnement disposant des librairies adéquates.

    Commandes :

    `py -m venv venv` | utilise votre python par défaut. Utilise le module venv ayant comme paramètre venv qui est le nom du dossier qui va contenir votre environnement virtuel.

    `venv\Scripts\activate` | actier votre environnement virtuel. Tous les binaires sous venv\Scripts sont désormais connus par votre PATH.

    `py -0p` | affiche vos différents environnements python. Votre environnement venv apparait en première ligne avec un * à la fin. Toutes vos commande py pointent désormais sur votre venv.

    `py -m pip install --upgrade pip setuptools wheel` | mise à jour des outils d'installation des packages.

    `venv\Scripts\deactivate` | ne pas utiliser immédiatement, gardez votre terminal ouvert ! Désactive l'environnement virtuel de votre terminal.

    **Remarque** : le répertoire venv et son contenu peuvent être supprimés manuellement et re-initialisé avec les commandes ci-dessus.

5. **Installation des librairies Python**

    Toujours dans l'environnement virtuel, exécuter la commande :

    `python -m pip install -r requirements.txt` | installe toutes les librairies recensées dans le fichier requirements.txt que vous avez cloné avec le projet T9A.


6. **Installation des webdrivers**

    L'architecture Selenium est client/serveur.

    La couche client étant installé dans le langage/Framework choisi, il reste la couche serveur. 

    Le client démare l'exécutable WEBDRIVER qui expose des ressources API REST permettant d'interagir avec le navigateur.

    L'emplacement de l'exécutable WEBDRIVER doit être connu de votre système (PATH).

    Nous pouvons créer un répertoire `%LOCALAPPDATA%\TEST_FACTORY\bin` et ajouter ce chemin dans la variable d'environnement PATH de l'utilisateur (obs : relancer le terminal ou VS Code pour prise en compte de la modification du PATH).

    **Liens des webdrivers**

    [CHROME](https://chromedriver.chromium.org/downloads) | la version majeure (premier chiffre) du chromedriver doit correspondre à votre version de chrome. Pour connaître la version de Chrome, saisir dans l'url de chrome l'instruction suivante : **chrome://version** et notez le premier numéro de version (exemple 106). Téléchargé le ZIP du chromedriver avec la version voulue et pour le système **win32**.

    [FIREFOX](https://github.com/mozilla/geckodriver/releases) | Pas de lien direct entre les versions de Firefox et geckodriver, il faut lire les releases notes. Généralement si votre Firefox est récent prenez la dernière version de geckodriver pour le système win64 

    !! Attention à date du support la version 0.32.0 n’avait pas le zip win64, partez plutôt sur la version 0.31.0

    [EDGE](https://developer.microsoft.com/fr-fr/microsoft-edge/tools/webdriver/) | si besoin d'Edge (version chromium).

    ==> Déposez les exécutables chromedriver.exe et geckodriver.exe dans votre dossier **bin**.


7. **Installation de Jenkins**

    Prérequis : avoir JVM 11 max pour lancer Jenkins, nous allons utiliser un JDK en ZIP qui sera portable et indépendant de notre système : 

    Télécharger le [JDK](https://adoptium.net/temurin/releases/?version=11), par exemple la version 11.0.16.1+1 et dézipper le dans notre workspace `%LOCALAPPDATA%\TEST_FACTORY`.

    Créer un répertoire JENKINS dans notre workspace `%LOCALAPPDATA%\TEST_FACTORY\JENKINS`.

    Télécharger un client récent de [Jenkins LTS](https://www.jenkins.io/download/), par exemple la version 2.361.2 LTS. Nous utiliserons le fichier WAR qui sera portable et lui aussi indépendant de notre système. 

    **Lancer jenkins** : vérifier que le JENKINS_LANCEUR.bat se trouve bien à la racine `%LOCALAPPDATA%\TEST_FACTORY` et que les variables des paths sont conformes à celles que vous avez défini préalablement. Exécuter le fichier qui va générer un token à utiliser pour se connecter sur l'url http://localhost:9090

    **Plugins** : installez les plugins Test Results Analyser et Robot Framework. 

    Dans le job que nous copierons le contenu du fichier JENKINS_CONFIG_JOB.bat dans :
    >Build Steps > Execute Windows batch command :

    Pour le reporting ajouter l'étape dans la section :
    
    >Post Build > Publish JUnit test result report, et copier y le texte suivant : OUTPUT\test-results.xml

    >Post Build > Publish Robot Framework test results
    >> OUTPUT
    >>> 80
    >>>> 90

    Nous pouvons paramétrer nos jobs sur jenkins :)


8. **Robot Framework**

    **[Documentation librairies natives de Robot Framework](https://robotframework.org/#resources)**

    **[Documentation librairies externes de Robot Framework](https://robotframework.org/?tab=libraries#resources)**

    **[Variables d'environnement du système](https://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#environment-variables)** | RF permet d'utiliser des variables d'environnement dans les données de test à l'aide de la syntaxe %{ENV_VAR_NAME}, (string uniquement).


9. **Selenium**


    **[Keywords RF Selenium](https://robotframework.org/SeleniumLibrary/SeleniumLibrary.html)**

## EXECUTER LES TESTS DE NON REGRESSIONS
Ici comment lancer les tests en mode CLI ou VS Code

1. Initialiser PROJECT_FOLDER

    Depuis le terminal du venv nous devons initialiser notre variable d'environnement PROJECT_FOLDER avec le dossier en cours. 

    `SET PROJECT_FOLDER=%CD%`

2. Exécuter les tests en CLI

    Ces deux commandes permettent de lancer les tests soit sur chrome soit sur firefox. Les outils CLI (Command Line Interface) utilisent les doubles tirets pour les options au format long. Le simple tirer pour le format court. On peut lancer robot par son module ou par son binaire. 

    `py -m robot --outputdir output --variable BROWSER:chrome --include web tests_T9A\testsuites`

    `robot -d output -v BROWSER:firefox -i web tests_T9A\testsuites`

3. Exécuter les tests avec le bouton Run

    Vérifier que le code ci-dessous est bien présent dans le fichier .vscode\launch.json

        "args": [
        "--nostatusrc",
        "--outputdir", "output",
        "--variable", "BROWSER:chrome",
        "--include", "tnr",
        ],

4. Exécuter les tests via Jenkins