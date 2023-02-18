# Les océans et leurs habitants : entre intrus et êtres-vivants

Les océans..., rien qu'à lire ce mot, nous savons déjà que nous nous aventurons dans quelque chose de vaste, trop vaste et plein de mystères. Et bien que les données à ma disposition, m'ont permis d'en apprendre plus, il faut garder en tête que nos connaissances sur le sujet ne sont finalement qu'une goutte d'eau dans la mer, dans l'océan ...

<iframe style="width: 80vw; height: 50vh; border: none;" src="https://query.wikidata.org/embed.html#%23Vue%20a%C3%A9rienne%20du%20Monde%0A%23The%20Blue%20Marble%3A%20Land%20Surface%2C%20Ocean%20Color%20and%20Sea%20Ice%20%7C%20La%20bille%20bleue%20%3A%20surface%20terrestre%2C%20couleur%20de%20l'oc%C3%A9an%20et%20glaces%20maritimes%0A%23defaultView%3AImageGrid%0ASELECT%20%3Focean%20%3FoceanLabel%20%3FvueAerienne%0AWHERE%20%7B%0A%20%20%3Focean%20wdt%3AP31%20wd%3AQ9430.%20%23oc%C3%A9ans%0A%20%20%3Focean%20wdt%3AP8592%20%3FvueAerienne.%0A%20%20SERVICE%20wikibase%3Alabel%20%7Bbd%3AserviceParam%20wikibase%3Alanguage%20%22fr%22%20%7D%0A%7D%0A%0A" referrerpolicy="origin" sandbox="allow-scripts allow-same-origin allow-popups"></iframe>
 > Source : [The Blue Marble: Land Surface, Ocean Color and Sea Ice ](https://visibleearth.nasa.gov/images/57730/the-blue-marble-land-surface-ocean-color-and-sea-ice)

## Sommaire : 
1. [Collecte des données](#données)
2. [Représentation visuelle de la localisation des océans.](#localisationCarte)
3. [Évolution des températures moyennes de l'air en surface des zones océaniques et terrestres](#températures)
4. [Les intrus: microplastiques dans les océans](#intrus)
5. [À la découverte d’êtres-vivants majestueux : les baleines](#baleines)
6. [L'origine de ces taxonomies](#donnéesCroisées)
7. [Conclusion](#conclusion)

## Collecte des données <a name="données"></a>

Tout d'abord, nous devons faire connaissance avec l'océan, qu'est-ce que c'est, où c'est, y en a-t-il qu'un seul ou plusieurs ? ... et pour y répondre rien de mieux qu'une requête wikidata :

``` sparql
#Infos de base sur les océans
SELECT DISTINCT ?ocean ?oceanLabel
WHERE {
  ?ocean wdt:P31 wd:Q9430. #océans
  SERVICE wikibase:label {bd:serviceParam wikibase:language "fr" }
}
```

Cette première requête aussi simpliste soit-elle m'a permis de faire "un état des lieux", à partir des résultats, j'ai pu me rendre compte que me sont renvoyés 10 valeurs de l'entité océan, cependant en réalité je suis censée en avoir 5, et ceci est lié au fait que wikidata inclue toutes les instances "ocean", soit même les sous-parties des océans (ex: l'océan Paciﬁque nord fait partie de l'océan Pacifique).

Tandis que la requête ci-dessous donne un résultat plus cohérent : 

```sparql

#Les 5 océans et leur superficies respectives
SELECT DISTINCT  ?ocean ?oceanLabel ?area ?image
{
  ?ocean wdt:P31 wd:Q9430. # Un océan
  ?ocean wdt:P2046 ?area. #superficie de l'océan en kilomètre carré
  OPTIONAL { ?ocean wdt:P18 ?image.} #une image en lien avec l'océan
  SERVICE wikibase:label {bd:serviceParam wikibase:language "fr" }
}
```
On retrouve les 5 océans, et leurs superficies respectives :

|      Océans      | Surface (en km²) |
|:----------------:|:----------------:|
|  océan Arctique  |      14 056 000  |
|   océan Indien   |      76 174 000  |
|   océan Austral  |      20 327 000  |
| océan Atlantique |     106 460 000  |
|  océan Pacifique |     161 760 000  |
|   océan mondial  |     361 260 000  |

> *Il faut noter que l'océan mondial regroupe les mers et océans de la terre, ainsi les 5 océans font partie de l'océan mondial.*
>
>>Tableau généré avec [Tables Generator](https://www.tablesgenerator.com)

Souhaitant obtenir un jeu de données plus riche j'ai tenté de collecter des informations dans wikidata autours des océans. Le problème est que certaines lignes sont soit des doublons voir des triplées (exemple: il y a deux entités pour la propriété profondeur, donc un renvois d'une valeur moyenne et d'une valeur maximale ce qui fait que la ligne "océan pacifique" apparaît 2 fois). Soit vides, dû à l'absence de propriétés communes, d'où le choix d'utiliser la fonction "OPTIONNAL" afin de ne pas me retrouver avec 0 résultat, voire pire une ligne a été supprimée comme par exemple Océan Austral car il n'a pas la propriété "coordonnées géographiques" sur wikidata.

```sparql
#Informations sur les océans
SELECT DISTINCT  ?ocean ?oceanLabel ?area ?volume ?largeur ?profondeur ?carteLocalisationImage ?image ?relief ?coordonneesGeo
{
  ?ocean wdt:P31 wd:Q9430. #océan
  ?ocean wdt:P2046 ?area. #superficie des océans
  ?ocean wdt:P2234 ?volume.#volume des océans
  ?ocean wdt:P4511 ?profondeur.#profondeur des océans
  ?ocean wdt:P625 ?coordonneesGeo.#coordonnées géographiques des océans
  
  OPTIONAL { ?ocean wdt:P2049 ?largeur.} #éventuellement la largeur des océans
  OPTIONAL { ?ocean wdt:P1943 ?carteLocalisationImage.} #éventuellement une carte de la localisation des océans
  OPTIONAL { ?ocean wdt:P18 ?image.} #éventuellement une image en lien avec les océans
  OPTIONAL { ?ocean wdt:P1944 ?relief.} #éventuellement une image en relief des océans
  SERVICE wikibase:label {bd:serviceParam wikibase:language "fr,en" }
}
```

Pour le reste des données exploitées, elles sont majoritairement issues de ressources des articles scientifiques. Pour les utiliser, j’ai fait du data wrangling avec Open Refine.

## Représentation visuelle de la localisation des océans <a name="localisationCarte"></a>

Je souhaitais utiliser l'outil map de Wikidata mais la visualisation avec de tels résultats laisse à désirer, c'est pourquoi j'ai donc décidé de travailler mes données sur Datawrapper, un outil de datavisualisation permettant à la fois de modifier mon tableau de données et de créer une visualisation sous forme de carte. 

Pour créer cette carte j’ai importé les données à ma disposition, et je me suis rendu compte que les données de coordonnées géographique était inutile, car la latitude et la longitude sont calculées en fonction des données saisies dans la colonne “Adresse”.

<iframe title="Localisation des océans" aria-label="Carte" id="datawrapper-chart-dxl9c" src="https://datawrapper.dwcdn.net/dxl9c/9/" scrolling="no" frameborder="0" style="border: none;" width="774" height="624" data-external="1"></iframe>

Les océans ont pour localisation un point présenté sous forme de cercle variant en fonction du volume de celui-ci (attention :  il ne faut pas oublier qu’un océan est une étendue d’eau pas un point précis). Sur chaque cercle, on peut retrouver une bulle d’information contenant les caractéristiques des océans récupérées au préalable par la requête wikidata. On y retrouve des liens cliquables, accessibles grâce à la fonction figer les infobulles. D’ailleurs leur contenu est codé ainsi : 


À l’intérieur de chaque accolade on rappelle les données présentes dans les différentes colonnes de notre choix, et on peut au utiliser des balises html ou non.

## Évolution des températures moyennes de l'air en surface des zones océaniques et terrestres <a name="températures"></a>

Les données extraites sont issues du site de la NASA, au format CSV. Les données étant de qualitées, je n'ai pas eu de modifications majeures à faire en dehors de traductions de l'anglais au français des colonnes et des lignes. Sur la plateforme [Datawrapper](https://www.datawrapper.de), je me suis focalisée sur l'affinage et l'ajout d'annotations.

<script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(e){if(void 0!==e.data["datawrapper-height"]){var t=document.querySelectorAll("iframe");for(var a in e.data["datawrapper-height"])for(var r=0;r<t.length;r++){if(t[r].contentWindow===e.source)t[r].style.height=e.data["datawrapper-height"][a]+"px"}}}))}(); 
</script>
 <iframe title="Température moyenne de l'air en surface des zones océaniques ou terrestres (°C)" aria-label="Interactive line chart" id="datawrapper-chart-ocDqe" src="https://datawrapper.dwcdn.net/ocDqe/1/" scrolling="no" frameborder="0" style="border: none;" width="1076" height="581" data-external="1"></iframe>
 
Grâce à cette datavisualisation, il est aisé de se rendre compte que la température moyenne de l'air en surface des zones océaniques ou terrestres ne fait qu'augmenter depuis les années 1880, et c'est d'autant plus important au niveau de la surface terrestre, jusqu'à atteindre une augmentation de 1,57°C en 2020, à l'inverse des zones océaniques qui elles bien qu'elle subissent une augmentation des températures, celles-ci n'ont jamais dépassé les 0,7°C en terme d'augmentation. En effet, l'océan absorbe au moins 90 % de l'excès de chaleur dans l'atmosphère, donc les températures moyennes de l'air en surface  des zones océaniques augmentent moins rapidement que celles en surface des zones terrestres .

## Les intrus: des microplastiques dans les océans <a name="intrus"></a>


![Image de l’observation au microscope de microplastiques](Mikroplastasarp.jpg)

> Observation au microscope de microplastiques
>
>> Par Dantor (talk) 20:55, 18 November 2013 (UTC) — Travail personnel, CC BY-SA 3.0, [Source](https://commons.wikimedia.org/w/index.php?curid=29710934)


<div class="flourish-embed flourish-bar-chart-race" data-src="visualisation/12727263"><script src="https://public.flourish.studio/resources/embed.js"></script></div>


Les microplastiques sont les petites particules inférieures à 5mm, issus de l’usure de matière plastique ou à des microbilles comme celles que l'on trouve dans les masques exfoliants ou les dentifrices.

Ces microplastiques sont dispersés partout dans le monde, en plus des sols, des différents points d’eau, on en retrouve énormément dans les océans, en surface comme dans les fonds marins. Depuis, l'ensemble des espèces de l’environnement marins ont été contaminés, nous contaminant par la suite, nous humain par le biais de notre alimentation,  car nous consommons pour la plupart certaines de ces espèces marines.

Les préoccupations autour de ces émissions ne sont pas prêtes de cesser, car en dehors de ceux déjà dans la nature involontairement (dans les eaux de surface européennes il y a  à environ 176 000 tonnes de microplastiques par an), il faut savoir qu’ au sein de l’UE, environ  [145 000 tonnes](# https://echa.europa.eu/fr/hot-topics/microplastics) de microplastiques au total sont utilisés chaque année. 

-----------------------------------------------------------------------------
 
* Astuce : séléctionner un pays pour mieux visualiser les bar charts *
<div class="flourish-embed flourish-chart" data-src="visualisation/12728041"><script src="https://public.flourish.studio/resources/embed.js"></script></div>

Les données permettant de faire cette datavisualisation étaient en anglais. Pour traduire les intitulés des colonnes, c’est évident, par contre traduire le nom de chaque pays, ligne par ligne aurait été chronophage, l’astuce a été d'utiliser OpenRefine et de réconcilier la colonne Pays avec les entités pays de wikidata en français. 

## À la découverte d’êtres-vivants majestueux : les baleines<a name="baleines"></a>

<div class="flourish-embed flourish-cards" data-src="visualisation/12792740"><script src="https://public.flourish.studio/resources/embed.js"></script></div>


## L'origine de ces taxonomies <a name="donnéesCroisées"></a> : 
 
Nombre d’espèces de baleine à fanons trouvées en fonction des années et les noms des personnes qui les ont découvert :
 
| Année de découverte/description | Personnes ayant décrit ou découvert ces espèces | Nombre d’espèces découvertes : |
|---------------------------------|-------------------------------------------------|--------------------------------|
| 1758                            | Carl von Linné                                  | 4                              |
| 1776                            | Philipp Ludwig Statius Müller                   | 1                              |
| 1781                            | Georg Heinrich Borowski                         | 1                              |
| 1804                            | Bernard-Germain de Lacépède                     | 1                              |
| 1818                            | Bernard-Germain de Lacépède                     | 1                              |
| 1822                            | Charles Des Moulins                             | 1                              |
| 1846                            | John Edward Gray                                | 1                              |
| 1861                            | Vilhelm Lilljeborg                              | 1                              |
| 1867                            | Hermann Burmeister                              | 1                              |
| 1878                            | John Anderson                                   | 1                              |
| 2003                            | Shiro Wada                                      | 1                              |
| 2003                            | Masayuki Oishi                                  | 1                              |
| 2003                            | Tadasu K. Yamada                                | 1                              |

> Jeux de données extrait avant enrichissment par réconciliation sur Open Refine
>
>>Tableau généré avec [Tables Generator](https://www.tablesgenerator.com)
<div class="flourish-embed flourish-table" data-src="visualisation/12793882"><script src="https://public.flourish.studio/resources/embed.js"></script></div>

L’objectif de cette visualisation sous forme de tableau est de visualiser qui a nommé le plus de baleine à fanons, en quelle année et aussi en savoir plus sur la personne qui les a découverts et/ou décrits. Il faut noter que la découverte d'une nouvelle espèces en 2003 concerne la même espèce pour les 3 personnes sont à l'origine de cette découverte.

##  Conclusion : <a name="conclusion"></a>

- Les  +  :
  - Sujet passionnant, enrichissant et qui me motive à continuer ce projet de datavisualisation de mon côté.
  - Meilleure compréhension des outils de datavisualisation au fur et à mesure de leur utilisation.
  - Utilisation de Github pages, que je pourrais à l’occasion me réapproprier pour d'autres projets.
  - La possibilité de transformer facilement le contenu des colonnes en un clic sur OpenRefine est révolutionnaire et d’un gain de temps précieux.
 
- Les  -  :
  - J’ai eu beaucoup de difficultés à trouver les données que je voulais, si j'avais opté pour la facilité il aurait été plus judicieux de se baser et de partir sur des jeux de données existants plutôt que de partir sur une idée de sujet et recherchée les jeux de données en lien.
  - Extension de fichier à exporter proposée rarement adéquate (je suis tombée sur beaucoup de fichiers .gz ou des données à exploiter en langage R ou autre logiciels pas à ma disposition.)
  - Difficulté de croiser des données
  - Les données ne sont pas en Open Data, très souvent il y a des paramètres bloquants, comme par exemple, les données ne sont exportables qu’à partir de X année, pour en récupérer certaines il aurait fallu créer des comptes avec adresse mail et mot de passe (exemple sur la plateforme Global Biodiversity Information Facility (GBIF)).
  - La partie sur les données croisées n'en est pas réellement une.
  - Sujet trop ambitieux qui nécessite plus de temps.


