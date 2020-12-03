---
title: 'Toulouse Postcodes Map Application'
author: "Akim van Eersel"
date: "03/12/2020"
output: 
  ioslides_presentation: 
    keep_md: yes
editor_options: 
  chunk_output_type: console
---



## Intoduction

This README file is an R Markdown presentation introducing the application made with the Shiny framework and hosted on Shiny servers: [__Toulouse Map__](https://vanakim.shinyapps.io/ToulouseMap/).  
_To see the present file in presentation view use the following link hosting it on RPubs : [Shiny App presentation](http://rpubs.com/vanAkim/ToulouseMapApp)._

In France, there is some confusion about postal codes as there are different databases with certain exclusive postal codes. This can be problematic in the search for address or geographical location. 

The purpose of the application is to show the geographical areas linked to the postal codes of the city of Toulouse and its surroundings, in the south of France.


## Post code

The official post code system links each municipality to a postal code. These postal codes are decided by __INSEE__ *([Insee](https://en.wikipedia.org/wiki/Institut_national_de_la_statistique_et_des_%C3%A9tudes_%C3%A9conomiques) is a french official public statistical institute)* and this metric is often called the INSEE postal code.

However, __La Poste__, which is the French public and main postal services company uses a postal code database similar to the one deliver by INSEE, but with many tweaks. These changes in some postal codes compared to the official database are used to improve the delivery and layout of __La Poste__ offices.  

## Post code

The two databases are used just as much, and therefore can lead to some confusion, especially since there is other systems also used.

Since I live in Toulouse, I'm interested to get a map of the city and its surroundings with the geographical areas linked to both post code databases.

On [Data.toulouse-metropole](https://data.toulouse-metropole.fr/pages/accueilv3/) both datasets can be gather as geojson files :  
- [`codes-postaux-de-toulouse`](https://data.toulouse-metropole.fr/explore/dataset/codes-postaux-de-toulouse/information/?dataChart=eyJxdWVyaWVzIjpbeyJjb25maWciOnsiZGF0YXNldCI6ImNvZGVzLXBvc3RhdXgtZGUtdG91bG91c2UiLCJvcHRpb25zIjp7fX0sImNoYXJ0cyI6W3siYWxpZ25Nb250aCI6dHJ1ZSwidHlwZSI6ImNvbHVtbiIsImZ1bmMiOiJBVkciLCJ5QXhpcyI6ImlkX2NvZGVfcG9zdGFsIiwic2NpZW50aWZpY0Rpc3BsYXkiOnRydWUsImNvbG9yIjoiIzY2YzJhNSJ9XSwieEF4aXMiOiJpZF9jb2RlX3Bvc3RhbCIsIm1heHBvaW50cyI6NTAsInNvcnQiOiIifV0sInRpbWVzY2FsZSI6IiIsImRpc3BsYXlMZWdlbmQiOnRydWUsImFsaWduTW9udGgiOnRydWV9) dataset, from **La Poste**, with last data input on **2020-12-01**  
- [`communes`](https://data.toulouse-metropole.fr/explore/dataset/communes/information/?location=11,43.64177,1.41796&basemap=jawg.streets) dataset, from **Toulouse Metropole**, with last data input on **2020-12-01**  
both are made available under the [Open Database License](http://opendatacommons.org/licenses/odbl/1.0/) ([local license text](https://github.com/vanAkim/DataScienceSpecialization-JohnsHopkins/blob/master/Data%20Products/FinalProject/ODC%20Open%20Database%20License%20(ODbL).md)). Any rights in individual contents of the database are licensed under the [Database Contents License](http://opendatacommons.org/licenses/dbcl/1.0/). 

## Post code

Here's an overview of theses datasets


```r
library(geojsonio)
geojson_read("./ToulouseMapApp/data/codes-postaux-de-toulouse.geojson", what = "sp") %>% as.data.frame() %>% head(3)
```

```
##   code_postal id_code_postal        geo_point_2d
## 1       31130              3 43.604213, 1.528549
## 2       31770             17 43.611717, 1.326944
## 3       31270              9 43.534212, 1.334696
```


```r
geojson_read("./ToulouseMapApp/data/communes.geojson", what = "sp") %>% as.data.frame() %>% head(3)
```

```
##              libcom           libelle code_insee code_fantoir
## 1     TOURNEFEUILLE     Tournefeuille      31557       310557
## 2     DREMIL LAFAGE     Drémil-Lafage      31163       310163
## 3 QUINT FONSEGRIVES Quint-Fonsegrives      31445       310445
##          geo_point_2d
## 1 43.578320, 1.335061
## 2 43.591307, 1.602786
## 3 43.581292, 1.542891
```

## Base map

The map is set by Leaflet Framework and polygons are drawn with the geojson files objects. Here's the background map.


```r
library(leaflet)
leaflet() %>% setView(lat = 43.6016, lng = 1.4407, 11) %>% 
      addProviderTiles(providers$CartoDB.Positron)
```

<!--html_preserve--><div id="htmlwidget-6d83e1001788478bf90e" style="width:720px;height:432px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-6d83e1001788478bf90e">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"setView":[[43.6016,1.4407],11,[]],"calls":[{"method":"addProviderTiles","args":["CartoDB.Positron",null,null,{"errorTileUrl":"","noWrap":false,"detectRetina":false}]}]},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
