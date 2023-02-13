# Les océans et leurs habitants : entre intrus et êtres-vivants

Les océans..., rien qu'à lire ce mot, nous savons déjà que nous nous aventurons dans quelque chose de vaste, trop vaste et plein de mystères. Et bien que les données à ma disposition, m'ont permis d'en apprendre plus, il faut garder en tête que nos connaissances sur le sujet ne sont finalement qu'une goutte d'eau dans la mer, dans l'océan ...

<iframe style="width: 80vw; height: 50vh; border: none;" src="https://query.wikidata.org/embed.html#%23Vue%20a%C3%A9rienne%20du%20Monde%0A%23The%20Blue%20Marble%3A%20Land%20Surface%2C%20Ocean%20Color%20and%20Sea%20Ice%20%7C%20La%20bille%20bleue%20%3A%20surface%20terrestre%2C%20couleur%20de%20l'oc%C3%A9an%20et%20glaces%20maritimes%0A%23defaultView%3AImageGrid%0ASELECT%20%3Focean%20%3FoceanLabel%20%3FvueAerienne%0AWHERE%20%7B%0A%20%20%3Focean%20wdt%3AP31%20wd%3AQ9430.%20%23oc%C3%A9ans%0A%20%20%3Focean%20wdt%3AP8592%20%3FvueAerienne.%0A%20%20SERVICE%20wikibase%3Alabel%20%7Bbd%3AserviceParam%20wikibase%3Alanguage%20%22fr%22%20%7D%0A%7D%0A%0A" referrerpolicy="origin" sandbox="allow-scripts allow-same-origin allow-popups"></iframe>
 > Source : [The Blue Marble: Land Surface, Ocean Color and Sea Ice ](https://visibleearth.nasa.gov/images/57730/the-blue-marble-land-surface-ocean-color-and-sea-ice)

## Sommaire
1. [Collecte des données](#données)
2. [Évolution des températures moyenne de l'air en surface des zones océaniques et terrestres](#températures)
3. [3](#c)
4. [4](#d)
5. [5](#e)
6. [6](#f)

## Collecte des données <a name="données"></a>

Tout d'abord, nous devons faire connaisance avec l'océan, qu'est-ce que c'est, où c'est, y en a-t'il qu'un ou plusieurs ? ... et pour y répondre rien de mieux qu'une requête wikidata :

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

Souhaitant obtenir un jeu de données plus riches j'ai tenté de collecter des informations dans wikidata autours des océans. Le problème est que certaines lignes sont soit des doublons (exemple: il y a deux entités pour la propriété profondeur, donc un renvois d'une valeur moyenne et d'une valeur maximale ce qui fait que la ligne "océan pacifique" apparaît 2 fois), soit vides dû à l'absence de propriétés communes, d'où le choix d'utiliser la fonction "OPTIONNAL" afin de ne pas me retrouver avec 0 résultat.

```sparql
#Informations sur les océans
SELECT DISTINCT  ?ocean ?oceanLabel ?area ?volume ?largeur ?profondeur ?carteLocalisationImage ?image ?relief ?coordonnéesGeo
{
  ?ocean wdt:P31 wd:Q9430. #océan
  ?ocean wdt:P2046 ?area. #superficie des océans
  ?ocean wdt:P2234 ?volume.#volume des océans
  ?ocean wdt:P4511 ?profondeur.#profondeur des océans
  ?ocean wdt:P625 ?coordonnéesGeo.#coordonnées géographiques des océans
  
  OPTIONAL { ?ocean wdt:P2049 ?largeur.} #éventuellement la largeur des océans
  OPTIONAL { ?ocean wdt:P1943 ?carteLocalisationImage.} #éventuellement une carte de la localisation des océans
  OPTIONAL { ?ocean wdt:P18 ?image.} #éventuellement une image en lien avec les océans
  OPTIONAL { ?ocean wdt:P1944 ?relief.} #éventuellement une image en relief des océans
  SERVICE wikibase:label {bd:serviceParam wikibase:language "fr" }
}
```

Pour arranger mon tableau de données j'ai donc décider d'utiliser OpenRefine.


##Évolution des températures moyenne de l'air en surface des zones océaniques et terrestres <a name="températures"></a>

Les données extraites sont issues du site de la NASA, au format CSV. Les données étant de qualitées, je n'ai pas eu de modification majeures à faire en dehors de traductions de l'anglais au français des colonnes et des lignes. Sur la plateforme [Datawrapper](https://www.datawrapper.de), je me suis focalisée sur l'affinage et l'ajout d'annotations.

<script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(e){if(void 0!==e.data["datawrapper-height"]){var t=document.querySelectorAll("iframe");for(var a in e.data["datawrapper-height"])for(var r=0;r<t.length;r++){if(t[r].contentWindow===e.source)t[r].style.height=e.data["datawrapper-height"][a]+"px"}}}))}(); 
</script>
 <iframe title="Température moyenne de l'air en surface des zones océaniques ou terrestres (°C)" aria-label="Interactive line chart" id="datawrapper-chart-ocDqe" src="https://datawrapper.dwcdn.net/ocDqe/1/" scrolling="no" frameborder="0" style="border: none;" width="1076" height="581" data-external="1"></iframe>
 
Grâce à cette datavisualisation, il est aisé de se rendre compte que la température moyenne de l'air en surface des zones océaniques ou terrestres ne fait qu'augmenté depuis les années 1880, et c'est d'autant plus important au niveau de la surface terrestre, jusqu'à atteindre une augmentation de 1,57°C en 2020, à l'inverse des zones océaniques qui elles bien qu'elle subissent une augmentation des températures, celles-ci n'ont jamais dépassé les 0,7°C en terme d'augmentation. En effet, l'océan absorbe au moins 90 % de l'excès de chaleur dans l'atmosphère, donc les températures moyennes de l'air en surface  des zones océaniques augmentent moins rapidement que celles en surface des zones terrestres .

## 3<a name="c"></a>
## 4<a name="d"></a>
## 5<a name="e"></a>
## 6<a name="f"></a>
