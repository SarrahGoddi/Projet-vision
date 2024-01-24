<div style="overflow-x: auto; max-width: 100%;">
<h1 align="center">🔭
Identification des relations spatiales dans les images en utilisant les réseaux de neurones convolutifs.
</h1>
<p align="center">  
<a href="https://arxiv.org/abs/1908.02660">Référence</a>
.
<a href="https://zenodo.org/records/8104370)">Dataset SpatialSense </a>

</p>

***
###   Introduction:
***

<p align="justify">
Les relations spatiales entre les objets fournissent des informations importantes pour les tâches de compréhension et d’interprétation de scènes. Ainsi, l'intégration de ces informations spatiales dans l'analyse d'images peut augmenter l'efficacité et la qualité de leur traitement.
La reconnaissance de relations spatiales est utilisée dans plusieurs domaines, tels que la détection d'objets, la génération de légendes, la création de descripteurs pour les images, etc.<br>
De nombreuses approches dans la littérature ont été présentées. On peut citer par exemple les descripteurs classiques relations spatiales, qui incluent les modèles topologiques (modèles basées sur le calcul des connexions des régions, RCC) et directionnels (histogramme des forces).
Ces modèles constituent des représentations condensées des données, axées sur des aspects spécifiques, tels que la topologie pour le modèle RCC8, la direction pour l’histogramme d’angle, ou des caractéristiques hybrides, comme le R-histogramme qui combine les descripteurs topologiques et directionnels.
Cependant, les récentes performances des modèles d'apprentissage profond dans diverses taches (reconnaissance d'objets ,classification de scènes, traitement automatique du langage) encouragent l’application de ces modèles dans l'apprentissage automatique des relations directement à partir des images. Ces approches concernent les modèles d’attention textuels (BERT) et visuels (CNN).<br>
D'autres approches combinant ces modèles ont été étudiés (DRNet, VTransE). L’objectif est d'exploité un ensemble d'informations liées à la position des objets dans l'espace (les boites englobantes), leur apparence ainsi que leurs classes sémantiques,  pour prédire des relations spatiales précises. <br>
L'objectif de notre travail est de classer les configurations spatiales. Pour ce faire, plusieurs modèles ont été implémentés pour modéliser les relations spatiales dans les images. Le premier modèle est basé sur le réseau  VggNet préentrainé pour extraire les caractéristiques des images, suivie d’un perceptron multicouche (MLP) pour la classification des relations spatiale. La deuxième approche est un MLP à trois couches cachées entrainé pour modéliser les configurations spatiales en fonction des coordonnées des boites englobantes des deux objets. La troisième approche est une combinaison des deux premières, et prend en compte pour la classification les informations visuelles (images) ainsi que la position des objets dans l’espace, définie par les coordonnées des boites englobantes. La dernière approche est un modèle qui combine les informations visuelles (images), textuelles (description de l’image) ainsi que les informations  relatives à la position des objets (boites englobantes). Ces modèles sont entraînés sur deux bases de données d’images: SpatialSense et SimpleShapes.<br>
L’article est organisé comme suit. Dans la section matérielle et méthode, nous décrirons les données utilisées ainsi que les modèles développés. Dans la section résultats et discutions, nousprésenterons les résultats des différents modèles utilisés.
</p>

***
###   1. Matériels et méthodes:
***
* ### Les données:
#### Dataset SimpleShapes:

<p align="justify">
Deux dataset sont utilisés pour entraîner les modèles. Le premier dataset SimpleShapes (figure 1) corresponds à un ensemble de 1508 images segmentés de dimension (224 X 224).  Chaque image montre deux formes segmentées, décris par leurs boites englobantes. Les formes peuvent être plus ou moins complexes. Ces données décrivent  4 classes (Above, Left, Right, Under). Cette base de données présente l’avantage d'être équilibré, c’est-à-dire que chaque classe est repartie de façon égale dans la base. De plus, pour chaque classe, les données sont représentatives de la diversité des objets de la base (chaque classe contient des images qui représentent tous les objets de la base). La description des relations spatiales dans cette base de données est décrite par un triplets (sujet, relation, objet).
   
</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/cf2022e5-1cb0-48ba-a246-51c76983aafa" alt="Figure 1: Images de la base de données SimpleShapes.">
</p>

#### Dataset SpatialSense:

<p align="justify">
SpatialSense est une base de données conçue pour évaluer la compréhension de la configuration spatiale. Ce dataset est  construit grâce à un crowdsourcing contradictoire, dans lequel des annotateurs humains sont chargés de trouver des relations spatiales difficiles à prédire. Chaque image est décrite par une phrase (vraie ou fausse) ainsi que les coordonnées des boites englobantes des objets d’interêts. De la même façon que la base SimpleShapes, la description des relations spatiales dans ce dataset est décrite aussi par un triplet (sujet, relation, objet).
Cette base de données présente de nombreux avantages, notamment en termes de variété de scène (environnements urbains, paysages naturels, scènes intérieures, etc.) et de diversité des éléments qui décrivent les sujets et les objets (figure 2). Cependant, cette base présente une limite majeur liées à la complexité des relations spatiales. En effet, les relations spatiales représentées dans SpatialSense ne se limitent pas aux interactions simples (comme c’est le cas pour la base SimpleShapes) . Elles incluent des configurations complexes et ambiguës (figure 3), à l’origine de défis supplémentaires en termes de prédiction et d’interprétation des relations spatiales. Ces ambiguïtés sont dues à la subjectivité de la perception spatiale (les relations spatiales telles que "gauche" et  " droite " peuvent être interprété en fonction de l’observateur), à l’ambiguïté des instructions de crowdsourcing (interprétation spécifique), à la variabilité des points de vue (la perception des relations spatiales depend de l’emplacement de l’observateur, de la prise en compte des perspectives dans les images, etc.), et à l’erreur humaine. Nous verrons  par la suite que toutes ces ambiguïtés dans les données de SpatialSense ont une influence sur la performance des modèles.

</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/9e212699-2fd3-4113-ba8e-a5708c5f11b7" alt="Figure 2: Images de la base de données SpatialSense qui ne présentent pas d’ ambiguïtés.">
</p>

* ### Les modèles:
<p align="justify">
4 modèles ont été développés pour intégrer la perception spatiale dans les images. Ces modèles peuvent prendre en compte uniquement l’information extraite des images (image) ou les informations  relatives à la position des objets (boites englobantes) ,ou la combinaison de ces informations (images + boites englobantes). Le dernier modèle qui sera présenté inclura en plus de ces éléments l'information textuelle.
</p>

#### Modèles de classification d’images (modèle 1): 
<p align="justify">
Le modèle utilisé est développé pour classifier les images en fonction de la position relative des objets d’intérêt. Ce modèle prend en entrée des images RGB ou des images segmentées (les éléments segmentés correspondent au sujet et  l’objet). <br>
L'architecture du modèle présenté dans la figure suivante (figure 4) se compose de deux étapes principales : le prétraitement des entrées et la génération du modèle. Dans la phase de prétraitement, les caractéristiques des images sont extraites en utilisant le modèle VGGNet pré-entrainé, modifié pour être entièrement convolutif. Ensuite,  un perceptron multicouche (MLP) à deux couches cachés est entraîné pour détecter les relations spatiales à partir des vecteurs de caractéristiques des images . 
La première couche du MLP (FC-0) réduit la dimensionalité à 512, et la seconde couches  (FC-1) la réduit davantage à 256. La dernière couche du modèle correspondant a une couche de sortie avec 4 neurones (correspondant au nombre de classes), caractérisée par la fonction d’activation softmax.
</p>
<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/4d3e322e-3633-4d83-92e3-26a47868d131" alt="Figure 4: Modèle d’apprentissage des relations spatiales entre les objets dans une image (modèle 1: CNN pré-entraîné + MLP)">
</p>

#### Modèles de classification des boites englobantes (modèle 2):
<p align="justify">
   
   L’architecture du modèle correspond à un MLP à 3 couches cachées (figure 5). La couche d’entrée prend une liste de 16 éléments, qui correspondent aux coordonnées des boites englobantes respectivement pour le sujet et l’objet. Les 3 couches cachées sont composé respectivement de 64, 32 et 16 neurones et utilisent la fonction d'activation ReLU. La dernière couche du réseau est une couche dense avec un nombre de neurones équivalent au nombre de classes (4) que le modèle doit prédire. Elle utilise la fonction d'activation softmax.
</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/89130972-98d9-48c2-b8bc-430b1ff45065" alt="Figure 5: Modèle de classification des coordonnées des boîtes englobantes (modèle 2, MLP)">
</p>

#### Modèle de combinaison (modèle 3):
<p align="justify">
Ce modèle prend en entrée le modèle de classification d’images ainsi que le modèle de classification des boites englobantes vues précédemment (figure 6) . Les vecteurs de caractéristiques extraites des images et des boites englobantes sont ensuite concaténés. Ainsi, les informations issues des deux modèles sont réunies pour obtenir un modèle de classification plus puissant. La prise de décision se fait par la dernière couche dense qui est ajouté pour permettre la classification.
</p>
<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/b088b47f-b5b5-41da-8af1-a871274b88c2" alt="Figure 6: Modèle de classification combinant  le modèle de classification des images et des boîtes englobantes (modèle3).">
</p>

#### Modèle multimodal (modèle 4):
<p align="justify">
Ce modèle prend en entrée un ensemble d’images, des coordonnées des boites englobantes ainsi qu’une description textuelle des relations entres les objets d'intérêt dans l’image (figure 7).
Dans ce modèle, les représentations des mots-clés des phases qui décrivent les images sont utilisées pour réaliser la classification des représentations spatiales. Pour extraire les représentations,  le modèle de langage XLM-Roberta est utilisé pour  obtenir les représentations vectorielles des mots (embeddings) des phrases.
</p>
<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/4a8870f7-b374-4a6a-8a77-b13274701119" alt="Figure 7: Modèle sémantique de classification (modèle 4).">
</p>

* ### Les hyperparamètres:

<p align="justify">
Pour les 3 premiers modèles présentés , la recherche des hyperparamètres a été réaliser en utilisant la méthode  Grid Search couplée a la k-fold cross validation. Cette méthode est définie pour rechercher la valeur du taux d’apprentissage qui maximise la vitesse de convergence du modèle.
Elle évalue ensuite chaque combinaison par validation croisée pour déterminer celle qui offre les meilleures performances en termes d’accuracy.
Le graphique suivant (figure 8) représente les résultats d'une recherche d'hyperparamètres utilisant une méthode de type Grid Search. Chaque point correspond à  un taux d'apprentissage différent.
Tout d’abord, on constate que les valeurs de  learning rate associées à une convergence rapide du modèle sont les valeurs comprises entre 10^-1 et 10^-4. Parmi ces valeurs, la valeur de learning rate qui maximise l’accuracy est égale a 10^-3 (accuracy : 0.75). <br>

Voici un tableau récapitulatif des paramètres des différents modèles (figure 9).

</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/664968c5-d61e-4e36-a258-4aa42730f522" alt="Figure 8: Résultat de la recherche de l’hyperparamètre maximisant la vitesse de convergence du modèle de classifications d’images (modele 1).">
</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/cdd201ca-61f7-44f3-bacf-23dc4717163f" alt="Figure 9: Hyperparamètres des modèles.">
</p>

***
###   2. Résultats et discutions:
***

* ### Résultats des performances sur le dataset SimpleShapes:
<p align="justify">
Les résultats des performances des modèles sur la base SimpleShapes sont présentés ci dessous (figure 10). Les performances des différents modèles sont évalués par l’accuracy. <br>
Le modèle 1, qui traite les images, montre une convergence rapide (au bout de 6 époques) et une précision élevée ( 0.95). Ce modèle ne présente pas de trace de surapprentissage, comme en témoignent les courbes de précision d'entraînement et de validation proches de l'une de l’autre.
 Le modèle 2, qui utilise les boîtes englobantes, prend plus de temps pour converger et a une précision moins élevée. En effet, le modèle converge au bout de 45 époques pour une accuracy de 0.9. Cette difference dans le temps de convergence peut être du à  la profondeur du modèle.
Le modèle 3, combinant images et Bbox, semble offrir des performances similaires au modèle de classification des boites englobantes. Ces résultats indiquent que le modèle qui prend en entrée uniquement les images est le plus performant.
   
</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/98147e39-8fbe-45ae-99ba-1de72cd19c4c" alt="Figure 10: Performances des modèles.">
</p>

* ### Résultats des performances sur le dataset SpatialSense:
<p align="justify">
Nous avons évalué les performances du modèle de classification des images (modèles 1) sur les données de SpatialSense. 
D’après le graphique qui présente la précision d'entraînement et de validation en fonction des époques, on constate que la précision d'entraînement augmente de manière constante, atteignant presque 1, tandis que la précision de validation semble atteindre 0,5. Cette différence entre la courbe des données d'entraînement et de validation suggère un surajustement du modèle (figure 11). <br>
Les performances du modèle sur un ensemble de données test sont présentées par la matrice de confusion (figure 11). D’après les résultats de la matrice de confusion, on constate que les classes ‘gauche’ et  ‘en dessous de’ ont les taux de classification les plus élevés, tandis que la classe ‘droite’ a le taux le plus bas, et présente une confusion avec la classe ‘gauche’. <br> 
Les faibles performances du modèle ainsi que la confusion entre la classe   ‘droite’ et ‘gauche’ (données test) sont du aux ambiguïtés de la base SpatialSense. En effet, les performances du modèle de classification d’images (modèle 1) entraîné par les sur les données SimpleShapes montrent de très bons résultat.
  </p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/cb2ea653-d65f-4401-8660-572343db130a" alt="Figure 11: Performances du  modèle de classification d’images (modèle 1) sur les données SpatialSense.">
</p>

<p align="justify">
Nous souhaitons savoir si un modèle qui prends en compte plusieurs types  d’informations (modèle 3) peut capturer les relations complexes de la base de données  SpatialSense. Nous testons donc les performances du modèle 3 qui prends en compte les caractéristiques des images et des boîtes englobantes en entrainant d’abord ce modèle sur les données  SpatialSense. Ces performances sont comparées au modèle pré-entraîné avec les données SimpleShapes et affiné avec les données SpatialSense (figure 12). <br>
Les résultats présentés montrent la comparaison de l’accuracy de deux modèles de classification d’images  (le modèle entraîné de zéro, et le  modèle pré-entraîné sur SimpleShapes, affiné ensuite sur SpatialSense). Le modèle pré-entraîné et affiné surpasse le modèle entraîné de zéro, comme on le voit par des courbes d'exactitude plus élevées pour les ensembles d'entraînement et de validation.  Les matrices de confusion normalisées montrent que le modèle affiné a des performances > 50 % pour l’ensemble des 4 classes. De plus, on constate que l’ambiguïté entre les classe ‘droite’ et ‘gauches’ semble moins visibles pour le modèle pré-entraîné sur SimpleShapes  et affiné SpatialSense. En effet, le modèle affiné prédit correctement la classe ’left’ dans 52% des cas et prédit correctement la classe ‘right’ dans 60% des cas.  À l’inverse, le modèle entraîné de zéro confond pour tous les image test la classe ‘left’ et ‘right’. Ainsi, l’affinement du modèle permet de mieux distinguer les classes ambiguës ‘left’ et ‘right’.
</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/b4f5881c-6a77-490e-aea0-506351e2f75d" alt="Figure 12">
</p>

<p align="justify">
Nous avons aussi évalué le modèle multimodal  qui intègre les caractéristiques des images, des boites englobantes ainsi que les caractéristiques des mots des phrases associées aux images (figure 13). <br>
Les performances du modèle au cours de l’entraînement augmentent pour les données d’entrainement (jusqu’a 0.7 ) et de validation (jusqu’a 0.4). C’est observation indique que le modèle apprend, mais semble souffrir de surajustement, puisque l'accuracy sur les données validation est significativement inférieure à l'accuracy sur les données d’entraînement. <br>
La matrice de confusion, qui témoigne des performances du modèle sur un ensemble de test montre que le modèle prédit uniquement  la classe  ‘left’, avec toujours une ambiguïté entre les classes ‘left’ et ‘right’. Ces observations suggèrent que la prise en compte des représentations des mots n’apporte pas d’amélioration par rapport au modèle plus simple (modèle image + Bbox).
</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/78f3357e-1fd9-46fc-97d3-7b4566576831" alt="Figure 13: Performances du  modèle qui intègre en plus la représentation des mots (modèle 4)">
</p>

***
###  Conclusion:
***
<p align="justify">
L’objectif était de réaliser la classification des configurations spatiales. Pour cela, les performances de  4 modèles ont été testées sur deux bases de données, SimpleShapes qui ne présente pas d'ambiguïté et SpatialSense qui présente des erreurs de labélisation. <br> 
Pour le dataset SimpleShapes, tous les modèles montrent des performances élevées, indiquant un bon ajustement sans surapprentissage (figure 14). Cependant, pour la base de données SpatialSense, le modèle le plus performant reste le modèle qui prend en compte uniquement les caractéristiques des images pour la classification des relations spatiales. Cependant, l'ambiguïté entre les classe ‘gauche’ et ‘droite’ reste fortement apparente (figure 11). L’utilisation du modèle pré-entraîné sur les données SimpleShapes et affiné sur les données SpatialSense permet de réduire cette ambiguïté (figure 12).
</p>

<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/6798e693-387b-41b9-9a33-4a285d247eb3" alt="Figure 14: Performances des modèles sur les données test">
</p>


</div>









