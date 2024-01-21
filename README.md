
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
D'autres approches combinant ces modèles ont été étudiés (DRNet, VTransE). L’objectif est d'exploité un ensemble d'informations liées à la position des objets dans l'espace (les boites englobantes), leur apparence ainsi que leurs classes sémantiques,  pour prédire des relations spatiales précises
  
</p>



