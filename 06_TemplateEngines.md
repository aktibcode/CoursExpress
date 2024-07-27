Chapitre : MOTEURS DE TEMPLATES

A-  Moteurs de modèles Introduction

    Un moteur de templates facilite l'utilisation de fichiers de templates statiques dans vos applications. Lors de l'exécution, il remplace les variables d'un fichier de templates par des valeurs réelles et transforme le template en un fichier HTML envoyé au client. Cette approche est donc privilégiée car elle permet de concevoir facilement des pages HTML.

    Certains moteurs de modèles populaires qui fonctionnent avec Express sont Pug , Mustache et EJS . Le générateur d'applications Express utilise Pug (anciennement connu sous le nom de jade) comme moteur par défaut, mais il prend également en charge plusieurs autres.
    Voir Moteurs de modèles (wiki Express : https://github.com/strongloop/express/wiki#template-engines) pour obtenir une liste des moteurs de modèles que vous pouvez utiliser avec Express. Voir également Comparaison des moteurs de création de modèles JavaScript : Pug, Mustache, Dust et plus encore (https://strongloop.com/strongblog/compare-javascript-templates-jade-mustache-dust/).

B-  Utilisation des moteurs de modèles avec Express
    
    Le moteur de modèles vous permet d'utiliser des fichiers de modèles statiques dans votre application. Pour restituer les fichiers de modèles, vous devez définir les propriétés de paramètres d'application suivantes :

        *   Vues : spécifie un répertoire dans lequel se trouvent les fichiers de modèle. Par exemple : app.set('views', './views').

        *   moteur de vue : il spécifie le moteur de modèle que vous utilisez. Par exemple, pour utiliser le moteur de modèle Pug : app.set('view engine', 'pug').
        
    Prenons le moteur de modèle Pug.

    -   Moteur de modèle Pug

    Apprenons à utiliser le moteur de modèles Pug dans les applications Node.js à l'aide d'Express.js. Pug est un moteur de modèles pour Node.js. Pug utilise des espaces et des indentations dans sa syntaxe. Sa syntaxe est facile à apprendre.

    -   Installer Pug

    Exécutez la commande suivante pour installer le moteur de modèle Pug :

        npm install pug --save

C-  Moteur de modèle Pug 1/2

    -   Installer pug

    Exécutez la commande suivante pour installer le moteur de modèle Pug :

        npm install pug --save

        https://i.imgur.com/BtUUi2N.png

D-  Moteur de modèle Pug 2/2

    Maintenant que Pug est installé, définissez-le comme moteur de modèle par défaut pour votre application. Vous n'avez pas besoin de le « requérir ». Ajoutez le code suivant à votre fichier index.js .

        app.set('view engine', 'pug');

        app.set('views','./views');

    Créez maintenant un nouveau répertoire appelé « vues ». À l'intérieur, créez un fichier appelé first_view.pug et saisissez les données suivantes.

        doctype html
        
        html
            head
                title = "Hello Pug"
            body
                p.greetings#people Hello World!

    Pour exécuter cette page, ajoutez l’itinéraire suivant à votre application.

        app.get('/first_template', function(req, res){

            res.render('first_view');

        });

    Vous obtiendrez le résultat suivant : Hello World !
    Pug convertit ce balisage très simple en HTML. Nous n'avons pas besoin de fermer nos balises ni d'utiliser les mots-clés class et ID. À la place, nous utilisons '.' et '#' pour les définir. Le code précédent est d'abord converti en :

        <!DOCTYPE html>
        <html>
            <head>
                <title>Hello Pug</title>
            </head>
            
            <body>
                <p class = "greetings" id = "people">Hello World!</p>
            </body>
        </html>

    Pug est capable de faire bien plus que simplifier le balisage HTML.

E-  Caractéristiques importantes du carlin (1/5)

    -   Caractéristiques importantes du pug

    Explorons maintenant quelques fonctionnalités importantes de Pug.

    -   Balises simples

    Les balises sont imbriquées en fonction de leur indentation. Comme dans l'exemple précédent, <title>était indenté à l'intérieur de la <head>balise, donc il était à l'intérieur de celle-ci. Mais, la <body>balise était sur la même indentation et cela signifie qu'elle est une sœur de la <head>balise.

    Nous n'avons pas besoin de fermer les balises. Dès que Pug rencontre la balise suivante sur le même niveau d'indentation ou sur un niveau d'indentation extérieur, il ferme automatiquement la balise.
    Nous avons 3 méthodes pour mettre du texte à l'intérieur d'une balise :

    -   Espace séparé

        h1 Welcome to Pug

    -   Texte canalisé

            div
                | To insert multiple lines of text, 
                | You can use the pipe operator

    -   Bloc de texte
            
            div.
                But that gets tedious if you have a lot of text.
                You can use "." at the end of a tag to denote a block of text.
                To put tags inside this block, simply enter tag in a new line and 
                indent it accordingly.

    -   commentaires

    Pug utilise la même syntaxe que JavaScript (//) pour créer des commentaires. Ces commentaires sont convertis en commentaires HTML (<!--comment-->). Par exemple,

        //This is a Pug comment

        <!--This is a Pug comment-->

    -   Les attributs
    
    Pour définir des attributs, nous utilisons une liste d'attributs séparés par des virgules et placés entre parenthèses. Les attributs de classe et d'ID ont des représentations spéciales. La ligne de code suivante couvre la définition des attributs, des classes et de l'ID de balise HTML.

        div.container.column.main#division(width = "100", height = "100")

    Cette ligne de code est convertie en ce qui suit :

        <div class = "container column main" id = "division" width = "100" height = "100"></div>


F-  Caractéristiques importantes du carlin (2/5)

    -   Passer des valeurs aux modèles

    Lorsque nous rendons un modèle Pug, nous pouvons lui transmettre une valeur de notre gestionnaire de route, que nous pouvons ensuite utiliser dans notre modèle. Créez un nouveau gestionnaire de route avec ce qui suit.

        const express = require('express');
        
        const app = express();

        app.get('/dynamic_view', function(req, res){

            res.render('dynamic', {

                name: “Gomycode", 

                url:"http://www.tutorialspoint.com"

            });

        });

        app.listen(3000);

    Et créez un nouveau fichier de vue dans le répertoire des vues, appelé dynamic.pug, avec le code suivant

        html
           head
              title=name
           body
              h1=name
              a(href = url) URL

    Ouvrez localhost:3000/dynamic_view dans votre navigateur ; vous devriez obtenir le résultat suivant

        https://i.imgur.com/TWQv3Cx.png


G-  Caractéristiques importantes du carlin (3/5)

    Nous pouvons également utiliser ces variables passées dans le texte. Pour insérer des variables passées entre le texte d'une balise, nous utilisons la syntaxe #{variableName}. Par exemple, dans l'exemple ci-dessus, si nous voulions mettre Greetings from TutorialsPoint, nous aurions pu procéder comme suit.

        html
           head
              title = name
           body
              h1 Greetings from #{name}
              a(href = url) URL

    Cette méthode d'utilisation des valeurs est appelée interpolation. Le code ci-dessus affichera le résultat suivant.

        https://i.imgur.com/i1TZJuR.png

H-  Caractéristiques importantes du carlin. (4/5)

    -   Les conditionnels

    Nous pouvons également utiliser des instructions conditionnelles et des constructions en boucle.
    Considérez ce qui suit :
    Si un utilisateur est connecté, la page doit afficher « Bonjour, utilisateur » et si ce n'est pas le cas, le lien « Connexion/Inscription » doit s'afficher. Pour y parvenir, nous pouvons définir un modèle simple comme

         html
            head
                title Simple template
            body
                if(user)
                    h1 Hi, #{user.name}
                else
                    a(href = "/sign_up") Sign Up

    Lorsque nous rendons ceci en utilisant nos routes, nous pouvons passer un objet comme dans le programme suivant

        res.render('/dynamic',{
        user: {name: "Ayush", age: "20"}
        });
        
    Vous recevrez un message − Bonjour, Ayush. Mais si nous ne transmettons aucun objet ou si nous en transmettons un sans clé utilisateur, nous recevrons un lien d'inscription.

I-  Caractéristiques importantes du carlin (5/5)

    -   Inclure et composants

    Pug fournit une manière très intuitive de créer des composants pour une page Web. Par exemple, si vous voyez un site Web d'actualités, l'en-tête avec le logo et les catégories est toujours fixe. Au lieu de copier cela dans chaque vue que nous créons, nous pouvons utiliser la fonction d'inclusion. L'exemple suivant montre comment nous pouvons utiliser cette fonctionnalité.
    Créez 3 vues avec le code suivant

    *   HEADER.PUG

        div.header.
            I'm the header for this website.

    *   CONTENT.PUG

        html
          head
              title Simple template
          body
              include ./header.pug
              h3 I'm the main content
              include ./footer.pug

    *   FOOTER.PUG

        div.footer.
            I'm the footer for this website.

    Créez un itinéraire pour cela comme suit

        var express = require('express');
        var app = express();

        app.get('/components', function(req, res){

            res.render('content');

        });

        app.listen(3000);
    
    Accédez à localhost:3000/components, vous recevrez le résultat suivant

        https://i.imgur.com/FUzMPLw.png

    include peut également être utilisé pour inclure du texte brut, du CSS et du JavaScript.
    Pug propose de nombreuses autres fonctionnalités. Mais celles-ci sont hors de portée de ce tutoriel. Vous pouvez explorer davantage Pug sur Pug