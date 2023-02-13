# Les océans et leurs habitants : entre intrus et êtres-vivants

Les océans..., rien qu'à lire ce mot, nous savons déjà que nous nous aventurons dans quelque chose de vaste, trop vaste et plein de mystères. Et bien que les données à ma disposition, m'ont permis d'en apprendre plus, il faut garder en tête que nos connaissances sur le sujet ne sont finalement qu'une goutte d'eau dans la mer, dans l'océan ...

<iframe style="width: 80vw; height: 50vh; border: none;" src="https://query.wikidata.org/embed.html#%23Vue%20a%C3%A9rienne%20de%20la%20Terre%20(surface%20terrestre%2C%20couleur%20de%20l'oc%C3%A9an%20et%20glaces%20maritimes)%0A%23defaultView%3AImageGrid%0ASELECT%20%3Focean%20%3FoceanLabel%20%3FvueAerienne%0AWHERE%20%7B%0A%20%20%3Focean%20wdt%3AP31%20wd%3AQ9430.%20%23oc%C3%A9ans%0A%20%20%3Focean%20wdt%3AP8592%20%3FvueAerienne.%0A%20%20SERVICE%20wikibase%3Alabel%20%7Bbd%3AserviceParam%20wikibase%3Alanguage%20%22fr%22%20%7D%0A%7D%0A%0A" referrerpolicy="origin" sandbox="allow-scripts allow-same-origin allow-popups"></iframe>
 > Image générée avec [Wikidata Query Service](https://query.wikidata.org)

Tout d'abord, nous devons faire connaisance avec l'océan, qu'est-ce que c'est, où c'est, combien y'en a-t'il ? ... et pour y répondre rien de mieux qu'une requête wikidata :

``` sparql
#
SELECT DISTINCT ?ocean ?oceanLabel
WHERE {
  ?ocean wdt:P31 wd:Q9430. #océans
  SERVICE wikibase:label {bd:serviceParam wikibase:language "fr" }
}
```

Cette première requête aussi simpliste soit-elle m'a permis de faire "un état des lieux", à partir des résultats, j'ai pu me rendre compte qu'en plus des entités océans connues de tous (les 5 océans), j'ai finalement 10 valeurs, et ceci est liés

## Sommaire
1. [1](#a)
2. [2](#b)
3. [3](#c)
4. [4](#d)
5. [5](#e)
6. [6](#f)

##

``` sparql
#Océans
SELECT DISTINCT ?ocean ?oceanLabel ?image
WHERE {
  ?ocean wdt:P31 wd:Q9430. # Un océan
  OPTIONAL { ?ocean wdt:P18 ?image.} #une image en lien avec l'océan
  SERVICE wikibase:label {bd:serviceParam wikibase:language "fr" }
}
```

<script type="text/javascript">!function(){"use strict";window.addEventListener("message",(function(e){if(void 0!==e.data["datawrapper-height"]){var t=document.querySelectorAll("iframe");for(var a in e.data["datawrapper-height"])for(var r=0;r<t.length;r++){if(t[r].contentWindow===e.source)t[r].style.height=e.data["datawrapper-height"][a]+"px"}}}))}(); 
</script>
 <iframe title="Température moyenne de l'air en surface des zones océaniques ou terrestres (°C)" aria-label="Interactive line chart" id="datawrapper-chart-ocDqe" src="https://datawrapper.dwcdn.net/ocDqe/1/" scrolling="no" frameborder="0" style="border: none;" width="1076" height="581" data-external="1"></iframe>
 
 > Tableau généré avec [Tables Generator](https://www.tablesgenerator.com)

## 1<a name="a"></a>
## 2<a name="b"></a>
## 3<a name="c"></a>
## 4<a name="d"></a>
## 5<a name="e"></a>
## 6<a name="f"></a>
