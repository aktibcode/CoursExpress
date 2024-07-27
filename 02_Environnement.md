Chapitre : ENVIRONNEMENT

A-  Installation

    En supposant que vous ayez déjà installé Node.js, vous devez créer un répertoire pour contenir votre application et en faire votre répertoire de travail.

        $ mkdir myapp
        $ cd myapp
    
    Une fois que nous sommes dans le dossier du projet, nous pouvons créer un package.json manuellement dans un éditeur de texte ou avec la commande de terminal npm init.

        $ npm init
    
    Cette commande vous demande un certain nombre d'informations, telles que le nom et la version de votre application. Pour l'instant, vous pouvez simplement appuyer sur RETOUR pour accepter les valeurs par défaut de la plupart d'entre elles, à l'exception de l'exception suivante :

        entry point: (index.js)
    
    Entrez app.js ou le nom que vous souhaitez donner au fichier principal. Si vous souhaitez qu'il s'agisse d'index.js, appuyez sur RETOUR pour accepter le nom de fichier par défaut suggéré.
    Enfin, nous pouvons installer le module en utilisant NPM

        $ npm install express --save

B-  Générateur d'applications express

    Utilisez l'outil de génération d'applications, express-generator, pour créer rapidement un squelette d'application. Installez le générateur d'applications en tant que package npm global, puis lancez-le.

        $ npm install -g express-generator
        
        $ express

    Par exemple, ce qui suit crée une application Express nommée myapp. L'application sera créée dans un dossier nommé myapp dans le répertoire de travail actuel et le moteur d'affichage sera défini sur Pug :

        $ express --view=pug myapp
    
    L'application générée a la structure de répertoire suivante :

        .
        ├── app.js
        ├── bin
        │   └── www
        ├── package.json
        ├── public
        │   ├── images
        │   ├── javascripts
        │   └── stylesheets
        │       └── style.css
        ├── routes
        │   ├── index.js
        │   └── users.js
        └── views
            ├── error.pug
            ├── index.pug
            └── layout.pug

        7 directories, 9 files

    Ensuite, installez les dépendances :

        $ cd myapp
        $ npm install
    
    exécutez l'application avec cette commande :

        $ npm start
    
    Ensuite, chargez http://localhost:3000/ dans votre navigateur pour accéder à l'application, vous devriez voir cette réponse courante

        https://i.imgur.com/xZltV5z.png

        *   Options de ligne de commande
        
        Ce générateur peut également être configuré à l'aide d'indicateurs de commande :

            $ express -h

        Usage: express [options] [dir]

        Options:

            -h,  --help           output usage information
                --version      output the version number
            -e,  --ejs             add ejs engine support
                --hbs            add handlebars engine support
                --pug            and pug engine support
            -H, --hogan        and hogan.js engine support
                --no-view      generate without view engine
            -v,  --view <engine> add view <engine> support (ejs|hbs|hjs|jade|pug|twig|vash) (defaults to jade)
            -c, --css <engine>  add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
                --git           add .gitignore
            -f, --force         force on non-empty directory

C-  Surveiller les modifications de fichiers

    Les applications Node.js sont stockées en mémoire et si nous apportons des modifications au code source, nous devons redémarrer le processus, c'est-à-dire node.
    
    Donc, pour rendre notre processus de développement beaucoup plus facile, nous allons installer un outil de npm, nodemon. Cet outil redémarre notre serveur dès que nous apportons une modification à l'un de nos fichiers.
    
    Pour installer nodemon, utilisez la commande suivante :

        $ npm install -g nodemon

    Vous pouvez maintenant commencer à travailler sur Express.