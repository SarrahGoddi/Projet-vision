
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
</p>
   
<p align="justify">
   
L'objectif de notre travail est de classer les configurations spatiales. Pour ce faire, plusieurs modèles ont été implémentés pour modéliser les relations spatiales dans les images. Le premier modèle est basé sur le réseau  VggNet préentrainé pour extraire les caractéristiques des images, suivie d’un perceptron multicouche (MLP) pour la classification des relations spatiale. La deuxième approche est un MLP à trois couches cachées entrainé pour modéliser les configurations spatiales en fonction des coordonnées des boites englobantes des deux objets. La troisième approche est une combinaison des deux premières, et prend en compte pour la classification les informations visuelles (images) ainsi que la position des objets dans l’espace, définie par les coordonnées des boites englobantes. La dernière approche est un modèle qui combine les informations visuelles (images), textuelles (description de l’image) ainsi que les informations  relatives à la position des objets (boites englobantes). Ces modèles sont entraînés sur deux bases de données d’images: SpatialSense et SimpleShapes.<br>
L’article est organisé comme suit. Dans la section matérielle et méthode, nous décrirons les données utilisées ainsi que les modèles développés. Dans la section résultats et discutions, nousprésenterons les résultats des différents modèles utilisés.

  
</p>

***
###   1. Matériels et méthodes:
***



