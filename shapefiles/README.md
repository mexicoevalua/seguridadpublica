This is a rewrite of the mapping code used in [this post](http://bl.ocks.org/diegovalle/8967565) by [Diego Valle](@https://twitter.com/diegovalle). It basically replaces all the boilerplate code with the `ichoropleth` function in `rMaps`, which wraps it, allowing easier reuse. Please follow the code from the original post to the get the shapefiles, topojson and data. My note will start with the dataset `hom`.

```S
d1 <- ichoropleth(rate ~ name, data = hom, ncuts = 9, pal = 'YlOrRd', 
  animate = 'year',  map = 'states'
)
d1$set(
  geographyConfig = list(
   dataUrl = "mx_states.json"
  ),
 scope = 'states',
 setProjection = '#! function( element, options ) {
   var projection, path;
   projection = d3.geo.mercator()
    .center([-89, 21]).scale(element.offsetWidth)
    .translate([element.offsetWidth / 2, element.offsetHeight / 2]);
  
   path = d3.geo.path().projection( projection );
   return {path: path, projection: projection};
  } !#'
)
d1$save('index.html', cdn = T)
```

You can publish this chart without leaving the comfort of R using the following lines of code (assuming you have a github account)

```S
# NOTE: DO NOT load rCharts before, since it interferes with rMaps currently.
library(rCharts)
post_gist(
  create_gist(c('index.html', 'mx_states.json')), 
  viewer = 'http://bl.ocks.org/'
)
```

Not bad for a few lines of R code!


