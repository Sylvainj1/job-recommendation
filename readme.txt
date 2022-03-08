Test technique pour BimBamJob par Sylvain JIANG

L'ensemble du projet à été réaliser sur jupyter.

L'objectif de l'exercice est de proposer un algorithmes de recommendation d'emplois. Pour résoudre ce problème on possède une liste de personne qui recherche de l'emploi avec son historique des postes qu'il a pu a occupés par le passé. On possède également la liste des emplois dans lesquels il postule actuellement. 

J'ai fais de la vieille technologique pour voir ce qu'il se fait actuellement. Je suis tomber sur deux méthodes utilisés pour des algorithmes de recommendations. 

- Filtrage collaboratif:
Le problème que nous avons s'apparente à ce que Netflix peut proposer sur son catalogue. Si je regarde un film "A", je peux être intéressé par ce que les précédents utilisateurs ont appréciés après avoir vu ce film "A". Imaginons que nous avons les mêmes goûts qu'une personne, il y a de forte chance que s'il nous conseille une série on risque de l'apprécier. On appelle cette méthode filtrage collaboratif. Selon l'historique d'un users, on va rechercher dans la base de données des users avec un passé proche et recommander selon leur parcours. On a besoin pour cela d'avoir un historique conséquent pour pouvoir apprendre. 

Dans notre cas, nous avons un users avec son historique des jobs qui a pu faire dans le passé et les offres dans lesquels il a postuler. Ce qu'il manque dans les données est un système de notation, par exemple pour un système de recommendation sur amazon, l'utilisateur note un produit, il donne un avis bien définit sur le produit s'il est apprécier ou pas. Dans notre cas, on a aucune certitude que le poste l'intéresse, on part du principe que si on postule à une offre c'est qu'on est intéresser. 
Par rapport au passé de l'utilisateur on a pas idée de s'il apprécit ou pas ce qu'il a pu faire dans le passé et recommander selon ses goûts. La forte présence d'un type de poste pourrait être un indicateur, par exemple si un users à occupé plusieurs postes en tant que serveur. 


- Content based filtering:
La seconde méthode est du content based filtering. Elle repose sur l'idée de proximité entre les postes, par exemple un data scientist est proche d'un poste de consultant, data engineering et est éloignés de poste tel que community manager. À partir de NLP, on peut utiliser de l'embedding sur la description et les requis d'un poste pour vectoriser les métiers. Plus les métiers vont être proche en distance, plus ils auront des requis similaires. On recommande ensuite selon le passé de l'utilisateur. Néanmoins, il y a pas d'évolution ou de changement d'environnement, on aura toujours des recommendations proche de ce que l'utilisateur fait ou à pu faire. 



Déroulement du projet:

1.EDA: 
La première étape que j'ai pu faire est d'explorer les données pour mieux les comprendre. 
Tout ce que j'ai pu faire dans cette partie est dans le fichier EDA.ipynb
Finalement en avançant sur le projet, j'ai utilisé peu de données qui sont fournit.

2.functions:
Les traitements des données sont tous sous formes de fonctions dans le répertoire function


3.Content based filtering:
Dans cette étape, je me suis basé uniquement sur les titres des postes pour créer un embedding avec BERT. J'ai vectoriser chaque titre de poste dans une dataframe. De la même manière avec la base de test, j'ai vectoriser l'historique de chaque poste que le users à pu occupé. Pour chaque poste que le test_users à pu occupé, je prédis une liste de 3 postes en utilisant du cosine similarity.  

Pour aller plus loin, on pourrait comparer selon la description, les compétences requises en faisant apprendre BERT sur ces informations. On aurait un contexte supplémentaire autre que le titre du poste.

jobs_title_similarity.ipynb contient ce que j'ai pu faire sur cette partie


4.filtrage collaboratif:
Dans cette partie là, j'ai considérer que si l'user postule à une offre c'est qu'il est intéresser. J'ai concaténer par la suite l'historique de l'user avec les jobs auxquels il a postuler pour augmenter la quantité de données. J'ai récupérer l'historique de chaque utilisateur avec la fréquence de chaque poste. Par exemple s'il a occupé 2x un poste de serveur, je vais indiquer une feature NB = 2. Les features d'entrée sont l'id de l'users, l'id du jobs, le nombre de fois où il a occupés ce jobs. 
J'utilise l'agorithme Alternating Least Squares avec implicit qui est très utilisé pour des recommendations. Pour chaque test_users, je retourne 3 recommendations.

implicit.ipynb contient ce que j'ai pu faire sur cette partie




