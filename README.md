# 2023-m2cns-rd-EnvEducatifs

## Contexte

L'objectif de ce projet est la réalisation d'une solution à base d'objets connectés pour améliorer l'expérience éducative (à l'école, à l'université ou même à la maison). Les objets connectés sont une formidable opportunité pour construire un écosystème éducatif plus efficace et plus juste. En effet, de tels écosystèmes peuvent faire en sorte de fournir un parcours éducatif approprié à chacun et de réduire les inégalités pour ceux qui nécessitent une attention particulière. Par exemple, un facteur important dans la réussite du processus d’apprentissage est la concentration. 

## Conception

Notre solution se concentrera sur cet aspect, et aura pour but de mesurer la concentration d'une classe d'élève en temps réel, en connectant pour cela une caméra à un ordinateur sur lequel les données seront centralisées, analysées et affichées. 
Nous avons utlisé l'outil Node-RED qui permet de développer des solutions d'objets connectés sur navigateur, au moyen d'un flux constitué de noeuds ayant chacun une fonction et connectés entre eux. Le serveur Node-RED est démarré localement sur l'ordinateur, et dans le cas de notre prototype, interagit avec le téléphone pour récupérer les données au moyen d'une application nommée "IP Webcam". Celle-ci enverra le flux vidéo capturé vers un lien HTTP que nous pourrons récupérer à intervalles régulièrs sous forme d'image.
Ensuite, l'image est analysée pour récupérer le nombre de visage visibles, en se basant sur le modèle de machine learning YOLO qui permet la détection d'objets en temps réel. L'enseignant renseigne au préalable le nombre d'élèves présents, et un score basé sur le nombre de visages détectés est affiché sur l'interface suivante, et mis à jour à intervalles réguliers : 

<img width="233" alt="image" src="https://github.com/evry-paris-saclay/2023-m2cns-rd-EnvEducatifs/assets/47394498/d7d2b863-06ad-443b-9d12-2b704f4ac957">

## Installation

Pour utiliser cette solution, il est nécessaire d'installer les deux noeuds suivants dans Node-RED :
- `@good-i-deer/node-red-contrib-face-detection` : il s'agit du  noeud qui contient le modèle de machine learning utilisé pour analyser les images ;
- `node-red-dashboard` : il s'agit du noeud permettant de mettre en place le dashboard.

Il est également possible d'installer le noeud `node-red-contrib-image-output`, qui permet de visualiser les images capturées.

Le flux est le suivant : 
<img width="935" alt="image" src="[https://github.com/evry-paris-saclay/2023-m2cns-rd-EnvEducatifs/assets/47394498/bbe38523-0100-4576-9dfb-298b432bf016](https://github.com/evry-paris-saclay/2023-m2cns-rd-EnvEducatifs/blob/main/docs/flux.png)">

Voici une explication des différents noeuds du flux :
- Le noeud **Lancement** sert à lancer le flux manuellement. Aussi, les noeuds **Reset** et **Réinitialisation** ont été ajoutés pour réinitialiser les valeurs du compteur et du graphique visibles sur le dashboard ;
- **Requête HTTP** sert à récupérer une image du flux envoyé par la caméra au lancement du noeud ;
- **Écriture image** va enregistrer l'image précédemment capturée dans un dossier de l'ordinateur ;
- **Lecture image** va chercher l'image à analyser afin de pouvoir l'utiliser dans le flux ;
- **Good Face Detection** va faire fontionner le modèle, en utilisant l'image chargée par le noeud précédent ;
- **Traîtement données** est une fonction qui calculera le score de concentration en fonction du nombre d'élèves reconnus par le modèle ;
- Les noeuds Dashboard **Évolution de la concentration** et  **Concentration en temps réel** servent à afficher les données réunies sur une interface. C'est de cette manière que l'utilisateur aura accès aux données qui lui sont pertinentes ;
- Les noeuds liés à la **Prévisualisation** et au **Debugging** servent à assurer le bon fonctionnement de certains noeuds individuellement ;
- Les noeuds "Change" (**Récupération** et **Stockage scoreGlo**) nous permettent de manipuler certaines données ;

  
Node-RED permet d'appliquer la solution à de plus grandes échelles, et il serait envisageable de la déployer dans un établissement scolaire. Concernant la prise en main, notre solution propose un dashboard facilement lisible par tous. Cependant, l'outil employé nécessite des connaissances pour pouvoir l'utiliser.
