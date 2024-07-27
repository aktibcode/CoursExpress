Chapitre : PREMIERE APPLICATION

A-  Bonjour le monde Exemple (1/4)

    Créons notre première application en utilisant Express.

    inclure cette bibliothèque :

        const express = require('express');
    
    Nous pouvons maintenant créer une application :

        const app = express();
    
    L'application est un serveur Web qui s'exécutera localement sur le port 4000 :

        const port = 4000;
    
    Définissons une route générique (*) avec la fonction app.get() :

        app.get('*', function(req, res){  res.end('Hello World');  });
    
    La fonction app.get() ci-dessus accepte les expressions régulières des modèles d'URL dans un format de chaîne. Dans notre exemple, nous traitons toutes les URL avec le caractère générique *.

    Le deuxième paramètre de app.get() est un gestionnaire de requêtes. Un gestionnaire de requêtes Express.js typique est similaire à celui que nous transmettons comme rappel à la méthode http.createServer() native/core de Node.js.

    Enfin, nous démarrons le serveur Web Express.js et affichons un message de terminal convivial dans un rappel :

        app.listen(port, function() {

            console.log('The server is running, ' + 
                        ' please, open your browser at http://localhost:%s', 
                        port);

        });

B-  Bonjour le monde Exemple (2/4)
    
    Le code complet du fichier index.js

        const express = require('express');
        const app = express();
        const port = 4000;

        app.get('*', function(req, res){
            
            res.end('Hello World');
        
        });

        app.listen(port, function(){

        console.log('The server is running, ' +
            ' please, open your browser at http://localhost:%s', 
            port);

        });

        Pour exécuter le script, nous exécutons node index.js depuis le dossier du projet :

            $ node index

        Maintenant, si vous ouvrez votre navigateur à l' adresse http://localhost:4000 (identique à http://127.0.0.1:4000 ), vous devriez voir le message « Hello World »

            https://i.imgur.com/VZWKxaw.png


C-  Bonjour le monde Exemple (3/4)
    
    Nous pouvons rendre notre exemple un peu plus interactif en faisant écho au nom que nous fournissons au serveur avec la phrase « Bonjour ». Pour ce faire, ajoutez la route suivante avant la route englobante de l'exemple précédent.

    app.get('/name/:user_name', function(req,res) {

        res.status(200);

        res.set('Content-type', 'text/html');
        
        res.send('<html><body>' +
                 '<h1>Hello ' + req.params.user_name + '</h1>' +
                 '</body></html>'
        );
    });

    À l'intérieur de la route /name/:name_route, nous définissons le code d'état HTTP approprié (200 signifie ok), les en-têtes de réponse HTTP et enveloppons notre texte dynamique dans le corps HTML et les balises h1.

    res.send() est une méthode Express.js spéciale qui va bien au-delà de ce que fait notre vieil ami du module HTTP principal res.end(). Par exemple, le premier ajoute automatiquement un en-tête HTTP Content-Length pour nous. Il augmente également Content-Type en fonction des données qui lui sont fournies.

D-  Bonjour le monde Exemple (4/4)

    Le code source complet du fichier index.js :

    const express = require('express');
    const app = express();
    const port = 4000;

    app.get('/name/:user_name', function(req,res) {

        res.status(200);
        res.set('Content-type', 'text/html');
        res.send('<html><body>' +
                 '<h1>Hello ' + req.params.user_name + '</h1>' +
                 '</body></html>'
        );
    });
    
    app.get('*', function(req, res){

        res.end('Hello World');

    });

    app.listen(port, function(){

        console.log('The server is running, ' +
            ' please, open your browser at http://localhost:%s', 
            port);

    });

    Après avoir arrêté le serveur précédent et lancé le script index.js, vous pourrez voir la réponse dynamique, par exemple, en saisissant http://localhost:4000/name/Gomycode dans votre navigateur, cela donne :

        https://i.imgur.com/mQEIfGH.png