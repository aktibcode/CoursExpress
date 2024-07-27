Chapitre : ROUTAGE

A-  Présentation des itinéraires
    
    Les itinéraires sont l'un des concepts de base d'Express et l'un des éléments qui le rendent vraiment utile.
    Le routage consiste à déterminer la manière dont une application répond à une demande client, à un point de terminaison particulier, appelé URI (ou chemin) et une méthode de demande HTTP spécifique (GET, POST, etc.).
    Chaque itinéraire peut avoir une ou plusieurs fonctions de gestionnaire qui sont exécutées lorsque l'itinéraire correspond.
    La définition d'itinéraire adopte la structure suivante :

        app.METHOD(PATH, HANDLER)

        *   app est une instance de express.
        *   METHOD est une méthode de requête HTTP , en minuscules.
        *   PATH est un chemin sur le serveur.
        *   HANDLER est la fonction exécutée lorsque l'itinéraire correspond.

B-  Méthodes de parcours (1/2)

    Une méthode de route est dérivée de l’une des méthodes HTTP et elle est attachée à une instance de la classe express.

    Le code suivant est un exemple d’itinéraires définis pour les méthodes GET et POST vers la racine de l’application.

        // GET method route
        app.get('/', function (req, res) {

            res.send('GET request to the homepage');

        })



        // POST method route 
        app.post('/', function (req, res) {

            res.send('POST request to the homepage');

        })

C-  Méthodes de parcours (2/2)
    
    Il existe une méthode de routage spéciale, app.all(), utilisée pour charger des fonctions middleware sur un chemin pour toutes les méthodes de requête HTTP. Par exemple, le gestionnaire suivant est exécuté pour les requêtes sur la route « /secret », que vous utilisiez GET, POST, PUT, DELETE ou toute autre méthode de requête HTTP prise en charge dans le module HTTP.

        app.all('/secret', function (req, res, next) {

            console.log('Accessing the secret section ...');

            next(); // pass control to the next handler

        })

    Express nous permet d'y attacher les méthodes.

        app.get('/', function (req, res) {

            res.send('GET request to the homepage');

        }).post('/', function (req, res) {

            res.send('POST request to the homepage');

        }).all('/secret', function (req, res, next) {

            console.log('Accessing the secret section ...');
            
            next(); // pass control to the next handler
        
        }).use(function(req, res, next){
            
            res.status(404).send('Page introuvable !');
        
        });

D-  Itinéraires de l'itinéraire (1/2)

    Le chemin d'accès est utilisé pour définir un point de terminaison où les requêtes peuvent être effectuées. Il s'agit d'un point de terminaison backend. Dans Express, les chemins d'accès peuvent être :

        *   chaîne

        *   modèles de cordes

        *   expressions régulières.


    Les caractères ?, +, * et () sont des sous-ensembles de leurs équivalents d'expression régulière.
    Le trait d'union (-) et le point (.) sont interprétés littéralement par des chemins basés sur des chaînes.

    Si vous devez utiliser le caractère dollar ($) dans une chaîne de chemin, placez-le entre ([ et ]). Par exemple, la chaîne de chemin pour les requêtes sur « /data/$book » serait « /data/([$])book ».

        *   Voici quelques exemples de chemins d'itinéraire basés sur des chaînes.

    Ce chemin d'itinéraire correspondra aux requêtes vers / à propos de .

        app.get('/about', function (req, res) {
    
            res.send('about');
    
        })

    Ce chemin d'itinéraire correspondra aux requêtes adressées à / random.text .

        app.get('/random.text', function (req, res) {  

            res.send('random.text');

        })


E-  Itinéraires de l'itinéraire (2/2)

    *   Voici quelques exemples de chemins d'itinéraire basés sur des modèles de chaîne.
    
    Ce chemin d'itinéraire correspondra à acd et abcd.

        app.get('/ab?cd', function (req, res) {
            
            res.send('ab?cd');
        
        })
    
    Ce chemin d'itinéraire correspondra à abcd, abbcd, abbbcd, etc.

        app.get('/ab+cd', function (req, res) {
        
            res.send('ab+cd');
        
        })

    Ce chemin d'itinéraire correspondra à abcd, abxcd, abRANDOMcd, ab123cd, etc.

        app.get('/ab*cd', function (req, res) {
            
            res.send('ab*cd');
        
        })
    
    Ce chemin d'itinéraire correspondra à /abe et /abcde.

        app.get('/ab(cd)?e', function (req, res) {
            
            res.send('ab(cd)?e');
        
        })

    *   Exemples de chemins d'itinéraire basés sur des expressions régulières :

    Ce chemin d’itinéraire correspondra à tout ce qui contient un « a ».

        app.get(/a/, function (req, res) {
            
            res.send('/a/');
        
        })

    Ce chemin correspondra au papillon et à la libellule, mais pas à l'homme-papillon, à l'homme-libellule, etc.

        app.get(/.*fly$/, function (req, res) {
            
            res.send('/.*fly$/');
        
        })


F-  Paramètres de l'itinéraire

    Les paramètres d'itinéraire sont utilisés pour capturer les valeurs attribuées à une position particulière dans l'URL. Ils sont appelés segments d'URL.

    Les valeurs obtenues sont rendues disponibles dans un objet req.params, en utilisant le nom du paramètre de route spécifié dans le chemin comme clés des valeurs.

    Route path: /users/:userId/books/:bookId
    
    Request URL: http://localhost:3000/users/34/books/8989
    
    req.params: { "userId": "34", "bookId": "8989" }


    Pour définir des itinéraires avec des paramètres d'itinéraire, spécifiez simplement les paramètres d'itinéraire dans le chemin de l'itinéraire comme indiqué ci-dessous.

        app.get('/users/:userId/books/:bookId', function (req, res) {
            
            res.send(req.params)
        
        })
    
    Pour avoir plus de contrôle sur la chaîne exacte qui peut être mise en correspondance par un paramètre d'itinéraire, vous pouvez ajouter une expression régulière entre parenthèses (()) :

        Route path: /user/:userId(\d+)
        
        Request URL: http://localhost:3000/user/42
        
        req.params: {"userId": "42"}

G-  Gestionnaires d'itinéraires

    Nous pouvons avoir plusieurs gestionnaires de requêtes, d'où le nom middleware. Ils acceptent le troisième paramètre/fonction next et l'appel de celui (next()) à exécuter fera passer le flux d'exécution d'un gestionnaire à l'autre :

        Par exemple :

            app.get('/api/v1/stories/:id', function(req,res, next) { 
                
                //do authorization
                //if not authorized or there is an error 
                //return next(error);
                //if authorized and no errors 

                return next(); 

            }, function(req,res, next) {
            
                //extract id and fetch the object from the database
                //assuming no errors, save story in the request object 
                
                req.story = story;

                return next(); 
            
            }, function(req,res) {

                //output the result of the database search 
                
                res.send(res.story);
            
            });

    Une autre technique utile consiste à passer des callbacks sous forme d'éléments d'un tableau, grâce au fonctionnement interne des arguments. Mécanisme JavaScript :

        const authAdmin = function (req, res, next) { 
            ... 
            return next(); 
        } 

        const getUsers = function (req, res, next) {
            ...
            return next();
        }

        const renderUsers = function (req, res) { 
            res.end(); 
        }

        const admin = [authAdmin, getUsers, renderUsers];
        app.get('/admin', admin);

H-  app.route()

    Vous pouvez créer des gestionnaires de route pouvant être chaînés pour un chemin de route à l'aide de app.route(). Étant donné que le chemin est spécifié à un seul emplacement, la création de routes modulaires est utile, tout en réduisant les redondances et les fautes de frappe.
    Voici un exemple de gestionnaires de route chaînés définis à l'aide de app.route().

        app.route('/book')
            .get(function (req, res) {

                res.send('Get a random book');

            })
            .post(function (req, res) {

                res.send('Add a book');

            })
            .put(function (req, res) {

                res.send('Update the book');

            })

    Le middleware express.Router nous permet de regrouper les gestionnaires de route pour une partie particulière d'un site et d'y accéder à l'aide d'un préfixe de route commun.

    Le code ci-dessous fournit un exemple concret de la manière dont nous pouvons créer un module de routage, puis l'utiliser dans une application Express .
    Tout d'abord, nous créons des routes pour un article dans un module nommé post.js. Ensuite, le code importe l'objet d'application Express, l'utilise pour obtenir un objet Router et lui ajoute quelques routes à l'aide de la méthode get(). Enfin, le module exporte l'objet Router.

    post.js - module de routage de publication .

        const express = require('express');

        const router = express.Router();

        // post page route.
        router.route('/post/:slug')
                .all(function(req, res, next) {

                    // runs each time
                    // we can fetch the post by id from the database

                }) 
                .get(function(req, res, next) {

                    //render post

                })
                .put(function(req, res, next) {
                    
                    //update post
                    
                })
                .post(function(req, res, next) {

                    //create new comment 

                }) 
                .del(function(req, res, next) { 

                    //remove post 

                });

        module.exports = router

    Pour utiliser le module routeur dans notre fichier d'application principal, nous utilisons d'abord require()le module route (post.js). Nous appelons ensuite use()l'application Express pour ajouter le routeur au chemin de gestion du middleware tout en spécifiant le chemin URL de '/blog'.

        const post = require(‘./post’);
        
        app.use(‘/blog, post);

    L'application sera désormais en mesure de gérer les requêtes vers /blog/post.