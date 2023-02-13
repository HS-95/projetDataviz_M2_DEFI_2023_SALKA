# Les océans et leurs habitants : entre intrus et êtres-vivants

Les océans, rien qu'à lire ce mot, nous savons déjà que nous nous aventurons dans quelque chose de vaste, trop vaste et plein de mystères. Et bien que les données à ma disposition, m'ont permis d'en apprendre plus, il faut garder en tête que nos connaissances sur le sujet ne sont finalement qu'une goutte d'eau dans la mer, dans l'océan ...


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
