# Covid-ViT_State_of_the_Art

## Présentation

L’algorithme du transformeur que nous allons voire s’appelle ViT et a été développé et publié par des
chercheurs de l’Université de Cornell. L’article est disponible sous le lien suivant :
https://arxiv.org/abs/2010.11929

En Deep Learning, les transformeurs utilisent les mécanismes de l’attention en attribuant un poids de
pertinence sur chaque partie des données d’entrée. Ils possèdent plusieurs couches auto-attentives.
La différence entre les réseaux CNN et ViT

Le modèle ViT se représente une image d’entrée comme une série de parcelles d’image et prédit
directement la classe de l’image entière. Les réseaux CNN utilisent des matrices de pixels alors que
les ViT découpent l’image en pièces visuelles. Le transformeur visuel divise l’image en parcelles de
tailles fixes, les vectorise et inclut un vecteur de positionnement en tant que donnée d’entrée pour
l’encodeur.

La couche auto-attentive permet au modèle de vectoriser l’information globale à travers toute
l’image. Il apprend ainsi à encoder la position relative d’une parcelle d’image pour reconstruire toute
la structure de l’image.
Le transformeur inclut aussi :

- Une couche auto-attentive à têtes multiples : elle concatène toutes les données de sortie des
couches auto-attentives aux bonnes dimensions. Les nombreuses têtes permettent au
modèle de s’entraîner en prenant en compte des relations locales et globales entre les
différentes structures de l’image.
- Une couche de perceptrons multicouches : elle contient deux couches avec une fonction
d’activation Gaussienne GELU
- Une couche de normalisation : elle est ajoutée avant chaque block pour améliorer le temps
d’entraînement et la performance global.

### Que sont les zones d’attention du ViT ?

L’attention, et plus particulièrement l’auto-attention, est l’un des schémas essentiels des
transformeurs. Elle quantifie les interactions par pairs d’entités qui aident un réseau à apprendre les
hiérarchies et les alignements présents dans les données d’entrée. Ce schéma d’auto-attention s’est
révélé être un élément clé pour augmenter la robustesse des réseaux visuels.

### Architecture des ViT

L’architecture des modèles de transformeurs visuels se compose des éléments ci-dessous :
- Découper une image en parcelles (de taille fixe)
- Aplatir les parcelles d’images
- Créer des vecteurs de dimension inférieur à partir de ces parcelles
- Inclure des vecteurs positionnels
- Alimenter la séquence vectorielle en tant que donnée à un transformeur encodeur
- Pré-entraîner le modèle ViT avec des images étiquetées
- Ajuster les dernières couches en les entraînant sur les images que l’on souhaite classer.

## Jeu de données

Notre sujet d’étude sera la détection du virus COVID-19 à partir d’images de tomodensitométrie de
poumons de patients. Nos deux modèles s’entraîneront sur une base de données d’entraînement
puis tenterons de détecter, sur un jeu de données de validation, quels patients ont contracté le virus.

Les données sont disponibles via le lien ci-dessous :
https://drive.google.com/drive/folders/1nBI02F-8Y0hFeN10CMu9Svj4xRViN5xt

Références bibliographiques
Notre preuve de concept s’appuiera sur l’article de recherche ci-dessous :
https://arxiv.org/ftp/arxiv/papers/2107/2107.01682.pdf
Cet article est le résultat de la participation du département informatique de l’université de
l’Université du Middlesex de Londres à la compétition MIA-COV19 organisée par l’Université de
Lincoln. Ci-dessous le lien de la compétition :
https://mlearn.lincoln.ac.uk/mia-cov19d/

### Objectifs

Nous vérifierons si notre modèle Vision Transformers est effectivement plus efficace pour la
problématique choisie. Nous tenterons ensuite, dans le cadre de notre preuve de concept, de dresser
l’état de l’art des transformeurs dans le domaine du traitement des images, à savoir notamment si,
oui ou non, ils sont amenés à remplacer les réseaux de neurones convolutifs.

## Modélisation

### Baseline : VGG-16 et Lenet

Nous allons entraîner deux modèles sur notre jeu de données d’entraînement composé de 130,739
images. Ces modèles seront testés sur un jeu de validation de 29,669 images. Etant donné la taille du
jeu de données, nous effectuerons l’entraînement par batch directement à partir des dossiers.
Nous effectuerons les prédictions des patients du jeu de données de validation par vote, comme
effectué dans l’article de recherche. Si plus de 25% des scans d’un patients sont étiquetées COVID
par un modèle, alors le patient est considéré comme ayant contracté le virus. Nous utiliserons les
métriques de précision, sensibilité et F1. Ces métriques sont des références pour l’évaluation des
algorithmes de classification.

Voici le rapport de résultats obtenus pour le modèle VGG-16. Le modèle étant sensible, nous avons
finalement décidé de rehausser le seuil à 37% pour obtenu de meilleurs résultats.

![image](https://user-images.githubusercontent.com/76253068/191058231-5f158bc7-3e66-4892-a311-e733e9175e02.png)

Les résultats du modèle Lenet :

![image](https://user-images.githubusercontent.com/76253068/191058343-7f71778b-13d1-401f-9033-4f85a380f969.png)

L’article sur lequel nous nous appuyons a obtenu les résultats ci-dessous :

![image](https://user-images.githubusercontent.com/76253068/191058483-155d13eb-c8fc-4b17-8c4a-b0dfb64bc412.png)

## Conclusion

Les résultats obtenus avec l’algorithme ViT sont nettement supérieurs aux résultats obtenus avec nos
deux modèles de référence : VGG-16 et Lenet-5.

En effet, les scores moyens F1 sont de 0.57 pour Lenet, 0.61 pour VGG-16 et enfin 0.76 pour le
transformeur ViT. Les transformeurs se révèlent être, dans notre cas précis et plus efficaces que les
réseaux de neurones convolutifs.

Ils sont plus robustes, obtiennent de meilleurs résultats et nécessitent un temps de traitement plus
court.

Ceci dit, pour obtenir de tels résultats, nous avons préchargé les poids d’un modèle entraîné au
préalable sur un très grand nombre d’images. De plus, les réseaux CNN ne sont pas à l’arrêt : des
innovations fréquentes améliorent leurs performances aujourd’hui encore, comme par exemple
l’EfficientNet qui a gagné le concours ImageNet en 2021.

Un autre modèle propose par Google, CoCa, est un système qui combine encodeur, doubleencodeur, encodeur-décodeur et obtient d’excellents résultats dans le traitement d’images.

Pour conclure, nous dirons que les transformeurs visuels pourraient effectivement remplacer les
réseaux CNN. Cependant, la concurrence entre les modèles est telle qu’il est impossible de répondre
à cette question de manière certaine.
