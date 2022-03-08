Test technique pour BimBamJob par Sylvain JIANG

L'ensemble du projet a été réalisé sur jupyter.

L'objectif de l'exercice est de proposer un algorithmes de recommandation d'emplois. Pour résoudre ce problème on possède une liste de personnes qui recherche de l'emploi avec son historique des postes qu'il a pu a occupé par le passé. On possède également la liste des emplois dans lesquels il postule actuellement. 

J'ai faits de la vieille technologique sur ce qui se fait actuellement. Je suis tombé sur deux méthodes utilisées pour des algorithmes de recommandations. 

- Filtrage collaboratif:
Le problème que nous avons s'apparente à ce que Netflix peut proposer sur son catalogue. Si je regarde un film "A", je peux être intéressé par ce que les précédents utilisateurs ont apprécié après avoir vu ce film "A". Imaginons que nous avons les mêmes goûts qu'une personne, il y a de forte chance que s'il nous conseille une série on risque de l'apprécier. On appelle cette méthode filtrage collaboratif. Selon l'historique d'un users, on va rechercher dans la base de données des users avec un passé proche et recommander selon leur parcours. On a besoin pour cela d'avoir un historique conséquent pour pouvoir apprendre. 

Dans notre cas, nous avons un users avec son historique des jobs qui a pu faire dans le passé et les offres dans lesquels il a postulé. Ce qu'il manque dans les données est un système de notation, par exemple pour un système de recommandation sur amazon, l'utilisateur note un produit, il donne un avis bien défini sur le produit s'il est apprécié ou pas. Dans notre cas, on n'a aucune certitude que le poste l'intéresse, on part du principe que si on postule à une offre c'est qu'on est intéresser. 
Par rapport au passé de l'utilisateur on n'a pas idée de s'il apprécie ou pas ce qu'il a pu faire dans le passé et recommander selon ses goûts. La forte présence d'un type de poste pourrait être un indicateur, par exemple si un users à occuper plusieurs postes en tant que serveur. 


- Content based filtering:
La seconde méthode est du content based filtering. Elle repose sur l'idée de proximité entre les postes, par exemple un data scientist est proche d'un poste de consultant, data engineering et est éloigné de poste tel que community manager. À partir de NLP, on peut utiliser de l'embedding sur la description et les requis d'un poste pour vectoriser les métiers. Plus les métiers vont être proche en distance, plus ils auront des requis similaires. On recommande ensuite selon le passé de l'utilisateur. Néanmoins, il n'y a pas d'évolution ou de changement d'environnement, on aura toujours des recommandations proche de ce que l'utilisateur fait ou à pu faire. 



Déroulement du projet:

1.EDA: 
La première étape que j'ai pu faire est d'explorer les données pour mieux les comprendre. 
Tout ce que j'ai pu faire dans cette partie est dans le fichier EDA.ipynb
Finalement en avançant sur le projet, j'ai utilisé peu de données qui sont fournies.

2.functions:
Les traitements des données sont tous sous forme de fonctions dans le répertoire function


3.Content based filtering:
Dans cette étape, je me suis basé uniquement sur les titres des postes pour créer un embedding avec BERT. J'ai vectorisé chaque titre de poste dans une dataframe. De la même manière avec la base de test, j'ai vectorisé l'historique de chaque poste que les users ont pu occuper. Pour chaque poste que le test_users a pu occuper, je prédis une liste de 3 postes en utilisant du cosine similarity.  

Pour aller plus loin, on pourrait comparer selon la description, les compétences requises en faisant apprendre BERT sur ces informations. On aurait un contexte supplémentaire autre que le titre du poste.

jobs_title_similarity.ipynb contient ce que j'ai pu faire sur cette partie


4.filtrage collaboratif:
Dans cette partie-là, j'ai considéré que si l'user postule à une offre c'est qu'il est intéressé. J'ai concaténé par la suite l'historique de l'user avec les jobs auxquels il a postulé pour augmenter la quantité de données. J'ai récupéré l'historique de chaque utilisateur avec la fréquence de chaque poste. Par exemple s'il a occupé 2x un poste de serveur, je vais indiquer une feature NB = 2. Les features d'entrée sont l'id de l'users, l'id du jobs, le nombre de fois où il a occupé ce jobs. 
J'utilise l'algorithme Alternating Least Squares avec implicit qui est très utilisé pour des recommandations. Pour chaque test_users, je retourne 3 recommandations. J'aurais pu également utiliser du KNN mais ALS est clairement plus performant pour éviter le biais de popularité, c'est-à-dire plus le poste à d'interaction, plus il va être recommandé. 

implicit.ipynb contient ce que j'ai pu faire sur cette partie




