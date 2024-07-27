Chapitre :  MIDDLEWARE

A-  Presentation du Middleware

    Les fonctions middleware sont des fonctions qui ont accès à l' objet de requête (req), à l'objet de réponse (res) et à la fonction suivante dans le cycle de requête-réponse de l'application.

    La fonction suivante est une fonction du routeur Express qui, lorsqu'elle est invoquée, exécute le middleware qui succède au middleware actuel.
    Les fonctions middleware peuvent effectuer les tâches suivantes :

        *   Exécutez n'importe quel code.
        *   Apportez des modifications aux objets de demande et de réponse.
        *   Mettre fin au cycle demande-réponse.
        *   Appelez le middleware suivant dans la pile.
    Si la fonction middleware actuelle ne termine pas le cycle requête-réponse, elle doit appeler next() pour passer le contrôle à la fonction middleware suivante. Dans le cas contraire, la requête restera en suspens.

B-  Écriture de middleware (1/2)

    Voici un exemple simple d'une fonction middleware

        //Simple request time logger
    
    const myLogger = function (req, res, next) {
    
        console.log("A new request received at " + Date.now());
        
        next();
    
    }

    Pour charger la fonction middleware, appelez app.use() en spécifiant quelle fonction middleware. Par exemple, le code suivant charge la fonction middleware myLogger avant la route vers le chemin racine (/).

        const express = require('express');
        
        const app = express();

        //Simple request time logger
        
        const myLogger = function (req, res, next) {
        
            console.log("A new request received at " + Date.now());
        
            next();
        
        }

        app.use(myLogger);

        app.get('/', function (req, res) {
        
            res.send('Hello World!')
        
        })

        app.listen(3000);

        Le middleware ci-dessus est appelé pour chaque requête sur le serveur. Ainsi, après chaque requête, nous obtiendrons le message suivant dans la console :

        A new request received at 1584954785016


C-  Écriture de middleware (2/2)

    Pour le restreindre à un itinéraire spécifique (et à tous ses sous-itinéraires), indiquez cet itinéraire comme premier argument de app.use(). Par exemple,

        const express = require('express');
        
        const app = express();

        //Middleware function to log request protocol
        
        app.use('/things', function(req, res, next){
            
            console.log("A request for things received at " + Date.now());
            
            next();
        
        });


        // Route handler that sends the response
        
        app.get('/things', function(req, res){
        
            res.send('Things');
        
        });

        app.listen(3000);

    Désormais, chaque fois que vous demandez une sous-route de « /things », ce sera l'instance où l'heure sera enregistrée.

D-  Middleware de gestion des erreurs

    Express JS est fourni avec des paramètres de gestion des erreurs par défaut. Nous pouvons définir des fonctions middleware de gestion des erreurs de la même manière que d'autres fonctions middleware, sauf que les fonctions de gestion des erreurs ont quatre arguments au lieu de trois :

        app.use(function (err, req, res, next) {

            console.error(err.stack)

            res.status(500).send('Something broke!')

        })

    Pour appeler un middleware de gestion des erreurs, il suffit de transmettre l'erreur à next(), comme ceci :

        app.get('/', (req, res, next) => {

            next(new Error('I am passing you an error!'));

        });

    Si vous transmettez quelque chose à la fonction next() (à l'exception de la chaîne « route »), Express considère la requête actuelle comme étant une erreur et ignorera toutes les fonctions de routage et de middleware restantes ne gérant pas les erreurs.

E-  Middleware Tiers

    Une liste de middlewares tiers pour Express est disponible ici . Voici quelques-uns des middlewares les plus couramment utilisés. Nous apprendrons également comment les monter et les utiliser.

    *   body-parser : analyse les corps des requêtes entrantes dans un middleware avant vos gestionnaires. Il est disponible sous la propriété req.body.
    Pour monter l'analyseur de corps, nous devons l'installer à l'aide de

        npm install --save body-parser
    
    et pour le monter, incluez les lignes suivantes dans votre index.js

        const bodyParser = require('body-parser');

        //To parse URL encoded data
        app.use(bodyParser.urlencoded({ extended: false }))

        //To parse json data
        app.use(bodyParser.json())

    Pour voir toutes les options disponibles pour le middleware body-parser, visitez sa page GitHub.

    *   cookie-parser : il analyse l'en-tête Cookie et remplit req.cookies avec des objets qui ont des noms de cookies comme clés. Pour monter l'analyseur de cookies, nous devons l'installer à l'aide de

        npm install --save cookie-parser 
    
    et pour le monter, incluez les lignes suivantes dans votre index.js

        var cookieParser = require('cookie-parser');

        app.use(cookieParser())


F-  L'ordre des Middleware est important

    Lorsqu'une demande est reçue par Express, chaque middleware correspondant à la demande est exécuté dans l'ordre dans lequel il est initialisé jusqu'à ce qu'une action de conclusion se produise (comme l'envoi d'une réponse).

        https://i.imgur.com/sIr251D.png

    Ainsi, si une erreur se produit, tous les middlewares assignés à la gestion des erreurs seront appelés dans l'ordre jusqu'à ce que l'un d'entre eux n'appelle pas la fonction next().


