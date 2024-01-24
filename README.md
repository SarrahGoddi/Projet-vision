
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
Le modèle utilisé est développé pour classifier les images en fonction de la position relative des objets d’intérêt. Ce modèle prend en entrée des images RGB ou des images segmentées (les éléments segmentés correspondent au sujet et  l’objet).
L'architecture du modèle présenté dans la figure suivante (figure 4) se compose de deux étapes principales : le prétraitement des entrées et la génération du modèle. Dans la phase de prétraitement, les caractéristiques des images sont extraites en utilisant le modèle VGGNet pré-entrainé, modifié pour être entièrement convolutif. Ensuite,  un perceptron multicouche (MLP) à deux couches cachés est entraîné pour détecter les relations spatiales à partir des vecteurs de caractéristiques des images . 
La première couche du MLP (FC-0) réduit la dimensionalité à 512, et la seconde couches  (FC-1) la réduit davantage à 256. La dernière couche du modèle correspondant a une couche de sortie avec 4 neurones (correspondant au nombre de classes), caractérisée par la fonction d’activation softmax.
</p>
<p align="center">
  <img src="https://github.com/SarrahGoddi/Projet-vision/assets/157230807/4d3e322e-3633-4d83-92e3-26a47868d131" alt="Figure 4: Modèle d’apprentissage des relations spatiales entre les objets dans une image (modèle 1: CNN pré-entraîné + MLP)">
</p>




