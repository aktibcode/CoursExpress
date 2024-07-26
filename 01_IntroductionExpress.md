Chapitre : INTRODUCTION A EXPRESS


A-  Qu'est-ce que Express.js ?

    Après la grande montée en puissance de Node.js, de nombreux frameworks et bibliothèques ont été créés. Le plus utilisé pour le rendu backend est Express. Ce sera le sujet de cette Super Skill. Au cours de ce chapitre, nous allons donc explorer :

        *   Qu'est-ce qu'Express ?

        *   Comment configurer un environnement Express ?

        *   Qu'est-ce que le routage via Express ?

        *   Que sont les middlewares et à quoi servent-ils ?

        *   Comment travailler avec un moteur de template ?

    Le site Web (https://expressjs.com/) d'Express décrit Express comme un « framework d'application Web Node.js minimal et flexible qui fournit un ensemble robuste de fonctionnalités pour les applications Web et mobiles ». Qu'est-ce que cela signifie exactement ?
    Express fournit des outils de base qui facilitent la création d'applications Node.js sans occulter les fonctionnalités Node.js que vous connaissez et aimez.

    Il fournit un ensemble de méthodes pour traiter les requêtes HTTP et fournit un système middleware qui étend ses fonctionnalités.
    Il facilite également la gestion du chemin (URL) de votre application et sans oublier qu'il utilise des modèles.

    Si vous avez écrit des applications sérieuses en utilisant uniquement les modules de base de Node.js, vous avez probablement dû réinventer la roue en écrivant à plusieurs reprises le même code pour les mêmes tâches telles que :

        *   Analyse des corps de requêtes HTTP.

        *   Analyse des cookies.

        *   Gestion des sessions.

        *   Organisation d'itinéraires avec une chaîne de conditions if basées sur des chemins URL et des méthodes de requête HTTP.

        *   Déterminer les en-têtes de réponse appropriés en fonction des types de données.

B-  Comment fonctionne Express ?

    Express.js possède généralement un point d'entrée ou, en d'autres termes, un fichier principal. Dans ce fichier, nous pouvons effectuer les étapes suivantes :

        1-  Inclure les dépendances tierces.

        2-  Configurez les paramètres de l'application Express.js tels que le moteur de modèle et les extensions de ses fichiers.

        3-  Définir des intergiciels tels que des gestionnaires d'erreurs, des dossiers de fichiers statiques, des cookies et d'autres analyseurs.

        4-  Définir des itinéraires.

        5-  Connectez-vous à des bases de données telles que MongoDB, Redis ou MySQL.

        6-  Démarrez l'application.

    Lorsque l'application Express.js est en cours d'exécution, elle écoute les requêtes. Chaque requête entrante est traitée via une chaîne définie de middlewares et de routes, de haut en bas. Par exemple, nous pouvons avoir plusieurs fonctions gérant chaque requête et certaines de ces fonctions seront au milieu (d'où le nom middleware) :

        1   Analysez les informations des cookies et passez à l’étape suivante lorsque vous avez terminé.

        2   Analysez les paramètres de l’URL et passez à l’étape suivante une fois terminé.

        3   Récupérez les informations de la base de données en fonction de la valeur du paramètre, 
            si un utilisateur est autorisé (cookie/  session), et passez à l'étape suivante s'il y a une correspondance.
        
        4   Affichez les données et terminez la réponse.

