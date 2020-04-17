# DC & Crossfilter


## Crossfilter

```js
var dim_class = crossfilter.dimension(d => d.class);

var count_by_class = dim_class.group();
var count_by_class = dim_class.group().reduceCount;
var sum_by_class = dim_class.group().reduceSum;
```

## DC
###Using with dc.dimension().group()
```js
dc.barChart("#chart_id")
.dimension(dim_class)
.group(count_by_class)
```

Usually you only need to specify the crossfilter group in .group(), as it will normally be a single numeric value for each dimension.
(dc/crossfilter is creating basic key:value pairs by looping through each row of data. The dimension value becomes the key, and the matching output from the .reduce() calculation becomes its value. ie. {key: ""C"", value: 8}.....)"

```js
dc.barChart("#chart_id")
.dimension(dim_class)
.group(reduce_by_class, "optional label string", function(d) { return d.value["Male"]; })
```

However, if you have used a custom .reduce(), then you also need to customise this function.
'd' is available to your custom function - this represents all the key:value pairs that dc has created, except the value is an object rather than just a number i.e. {key: ""C"", value: {Male: 5, Female: 3}}
Therefore your custom function needs to specify which number in the value you need.


The colors used in a dc chart can be set using the dc-chart.colors() function:
```js
chart1 = dc.barChart("#chart_id")
                    .dimension(dim)
                    .group(count_by_dim)
                    .colors(....)
```

If you dont call this function, dc will automatically color charts using one of these d3's color scale functions :
```js
.colors( d3.scale.category20c() )
.colors( d3.scale.category10() )
```

These d3 color functions are shorthand versions of a longer function eg.	
```js
colors( d3.scale.ordinal().range(d3_category20c) )
```
Where  d3_category20c  is a predefined array of colors (eg. ["red", "orange", "yellow",....])	

Using alternative color schemes (Ordinal)	
You can create your own ordinal color scheme by inserting your own color array into the scales range:	
```js
chart1 = dc.barChart("#chart_id")
                    .dimension(dim)
                    .group(count_by_dim)
                    .colors(d3.scale.ordinal().range(["red", "orange", "green"]))
```

or, you can use other predefined color scheme (eg. ColorBrewer*) by inserting the name of the array you need:	
```js
chart1 = dc.barChart("#chart_id")
                    .dimension(dim)
                    .group(count_by_dim)
                    .colors(d3.scale.ordinal().range(colorbrewer.Oranges[9]))
```
*You will need to add the "http://colorbrewer2.org/export/colorbrewer.js" script in the <head>	
	
	
Color Ordering	
By default colors are assigned in the the order that a charts categories appear:	
Pie Charts :  The categories are automatically ordered by value (from largest to smallest) therefore the largest category will get the first color in the color array.	
Row Charts :  The categories are automatically ordered by value (largest at the top) therefore the largest category will get the first color in the color array.	
Bar Charts :  The categories are automatically ordered alphabetically - however by default they all have the same color (first color in the color array).	
	
Color by category name	
If you want to assign colors to specific categories then you need to add a .domain() to the color scale:	
```js
chart1 = dc.barChart("#chart_id")
                    .dimension(dim)
                    .group(count_by_dim)
                    .colors(d3.scale.ordinal().domain(["Poor", "Neither", "Good"]).range(["red", "orange", "green"]))
```
The .domain() should contain an array with the category names/keys, and should be in the same order as the colors you want to give them.	

