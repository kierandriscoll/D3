# Javascript
Variables can be defined and can contain numbers or strings.
  var = 123;

An **array** can be created [ ]. Values can be referenced using the position in the array eg. cars[0] = Saab
```js
var cars = ["Saab", "Volvo", "BMW"];
```

An **object** can created using { }, and are made of key:value pairs. Values can be numbers, strings, arrays, objects or functions. A value can be referenced using the object and key name separated by a period eg. people.lastName = Doe
```js
var people = {firstName:"John", lastName:"Doe"};     
```

# User Defined Functions
Functions are defined as shown below:
```js
function myFunction() { alert( cars[1] ); }
```
Alternatively you can use arrow functions:
```js
myFunction = () => { alert( cars[1] ); }
```

If you want to link an action (eg clicking a button) to your function:
```html
<button onclick="javascript: myfunction()"></button>
```
```js
myFunction = () => { alert( "The button has been clicked." ); }
```

# Conditions / IF-ELSE / IF-THEN-DO
if() conditions can be written as:
```js
var conditional = if(year == 2015) {
  "Equal";
} else {
  "Not Equal";
}
```
Alternatively there is a shorthand way of writing this:   
**(condition) ? value if TRUE : value if FALSE**
```js
var conditional = (year == 2015) ? "Equal" : "Not Equal";
```

## Javascript Libraries
Libraries are available that will add/extend javascript functionality. They are usually loaded in the HTML head:
```html
<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js" type="text/javascript"></script>
</head>	
```
However it is possible to add them using javascript (eg. if you are developing in the console): 
```js
function addLibrary(path) { 
  var script = document.createElement('script');
  script.type = 'text/javascript';
  script.src = path;
  document.head.appendChild(script);
}
```

## Getting data in 
The easiest way to imports a delimited file (eg. CSV) is to load the d3.js library and use **d3.csv()**.  
This function reads and converts CSV into **JSON**
```js
d3.csv("dataset.csv", d3.autoType)
  .then(function(data){
  // other code to apply to data
  });
```

If you need to import an Excel file then the best option is to to load the xlsx.js library   


# JSON Arrays
A JSON array is an **array of objects** and is used by various visualisation libraries. If you import tabular data into javascript it will normally need to be in this format:  
```js
var json = [{firstName:"John", lastName:"Doe"}, {firstName:"Susan", lastName:"Smith"}, {firstName:"David", lastName:"Jones"}];     
```

To filter a JSON array (and return a new one):
```js
var filtered = json.filter(function(json){return json.lastName == "Smith"});
// or using arrow function
var filtered = json.filter(json => {json.lastName == "Smith"});
```

To loop through a JSON array and create/edit values:
```js
json.forEach(function(i) {return i.fullname = i.firstName + " " + i.lastName});
// or using arrow function
json.forEach(i => {i.fullname = i.firstName + " " + i.lastName});
```


# JQuery
JQuery is a javascript library that makes it easier to change a HTML document. The main way of selecting a tag/id/class.. with JQuery is to use the **$** function with the relevant CSS selector - for example the code below would select all HTML tags whose id="mapid" :
```html
<div id="mapid"></div>
```
```js
$("#mapid");
```

Insert/append content using JQuery. 
```js
$("#mapid").append("Add text");  
$("#mapid").append("<div><a href="google.com">Add link</a><div>");
```
Insert class or style using JQuery. 
```js
$("#mapid").addClass("ssss");
$("#mapid").css("color", "red")
```

Nb. JQuery is simpler way of selecting elements compared to using raw javascript such as *document.getElementById(id)* 

# Dates
Javascript can understand dates if they are in the format 'YYYY-MM-DD'.  
If they are in a different format (eg. imported from a CSV as DD/MM/YYYY) then you may need to convert them to do calculations or sorting.  
The easiest way to do this is to use moment.js to convert the string and .toDate() to store it as a standard javascript date
```html
<script src="lib/moment.min.2.18.1.js" type="text/javascript"></script> 
```
```js
moment('18/04/2020', 'DD/MM/YYYY').toDate();
```
To do this on an array of objects:  
```js
array.forEach(function(d) {
    d.properDate = moment(d.stringDate, 'DD/MM/YYYY').toDate();
 });   
```

# Map & Reduce & Sort
.map() .reduce() .sort() and .filter() are functions that can be used on **arrays**. You need to insert a function into them to specify how to process any output. 

## Map
**.map()** automatically loops through each element of an array and returns a value. 
```js
array = [1,1,2,3,5,8]
array.map(i => i)
// array.map(function (i) { return i });
```

You can add other calculations during this process: 
```js
array.map(i => i + "A")
// array.map(function (i) { return i + "A" });
```
returns the array: ["1A", "1A", "2A", "3A", "5A", "8A"]

You can also apply other functions during this process: 
```js
array = ["hoRriBle "," tEXt"," sTRinGs "]
array.map(i => i.trim().toLowerCase())
```
returns the array: ["horrible","text","strings"]

If you have an **array of objects** then you can use the objects keys to extract their values **as an array**:
```js
var json = [{firstName:"John", lastName:"Doe"}, {firstName:"Susan", lastName:"Smith"}, {firstName:"David", lastName:"Jones"}];  

json.map(i => i.firstName)
```
returns the array: ["John", "Susan", "David"]

## Reduce
**.reduce()** automatically loops through each element of an array, and carries out a **cumulative** calculation which you define.  
The following code uses reduce to create a simple **sum** of the values in the array (ie. 20): 
```js
array = [1,1,2,3,5,8]
array.reduce((acc, i) => acc + i, 0);
// array.reduce(function (acc, i) { return acc + i}, 0);
```
*i* represents the current element.  
*acc* represents an accumulator. Its keeps a running total, and its final value will be your result.  
The *0* argument is the initial value we want the accumulator to start with.

The calculation you specify can include other values or conditions, to allow you to return things like the min/max/average value:
```js
array.reduce((acc, i) => (acc + i) / array.length, 0);  // The average value
array.reduce((acc, i) => i > acc ? i : acc, array[0]);  // The maximum value (the initial value is the first element in the array)
array.reduce((acc, i) => i < acc ? i : acc, array[0]);  // The minimum value
```

## Sort
**.sort()** sorts an array.
If you are sorting **numeric values** you need to specify a function for how they will be ordered.
Nb. sort() works by iteratively comparing pairs of values (a,b) and moving them depending on the whether the differenc is +/-:
Nb. Dates will need to be converted in date format or 'YYYY-MM-DD' to sort properly.
```js
array = [10,1,25,3,95,80]
array.sort((a, b) => a - b)  // ascending order - for descending revesre the calculation: (a, b) => b - a  
// array.sort(function(a, b) {return a-b});  
```

If you are only sorting by one **character value**, then a function is not needed. It will be alphabetical by default (you can then use .reverse() for descending order). 
```js
array = ["bike","dog","brick","cat","ant","zzz"]
array.sort();  
```

If you have an **array of objects** then add the key that you want to compare:
```js
var json_gp = [{id: 1, amount:123, colour:"red"}, {id: 1, amount:250, colour:"blue"}, {id: 2, amount:30, colour:"red"}, {id: 3, amount:188, colour:"yellow"}, {id: 3, amount:99, colour:"green"}];     

array.sort((a, b) => a.amount - b.amount);  
```

To sort by more than one value (eg. id & amount) combine comparisons with the OR operator:
```js
json_gp.sort((a, b) => a.id - b.id || a.amount - b.amount);  
```
If one of these is a **character value**  you need to use the character comparison: **a > b ? 1 : -1**  
```js
json_gp.sort((a, b) => a.id - b.id || a.name > b.name ? 1 : -1);  
```

## Grouping 
If you have an **array of objects** and you want something like counts/max/min but **grouped by a category**:
```js
var json_input = [
{id: 1, amount:123, date:"01/04/2020"},
{id: 1, amount:250, date:"15/04/2020"},
{id: 2, amount:30, date:"08/04/2020"},
{id: 3, amount:188, date:"02/04/2020"},
{id: 3, amount:99, date:"12/04/2020"}];

var json_output = [
{id: 1, max_amount:250},
{id: 2, max_amount:30"},
{id: 3, max_amount:188}];
```
There are no pre-built functions for this, but you can combine sort/filter/map/reduce() to create your own.  
Select the rows with the highest/lowest values:
```js
var slice_by = function(array, key) {
  by_groups = [...new Set(array.map(e => e[key]))];  // Creates an array of distinct key values
  output = [];  // Initialse empty output 
  
  by_groups.forEach((group) => {
    first_row = array.filter(e => e[key] == group).slice(0,1);  // Filter by key value and take first row
    output.push(first_row[0]);  // Add to the output array
  })
  return(output);
}
```
Usage - The function above only returns the top row for each group, so the data needs to be sorted first so that the highest/lowest value is at the top: 
```js
json_input.sort((a, b) => a.id - b.id || b.amount - a.amount);  // amount sorted in descending order 
json_output = slice_by(json_input, "id")
```


# String methods and Regex
String objects have various built-in **methods** that can be used to manipulate text.
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String
```js
textstring = "Humpty Dumpty sat on the wall."

textstring.trim()
textstring.substr(0,6)
textstring.replace('wall', 'fence')
textstring.toLowerCase()
textstring.split(" ")
textstring.trim()
```


# A Datatable in JS
The **DataTables** library depends on JQuery so that it can be interactive (sortable/searchable/pagination..). 

```html
<div id="dt_container">
<table id="table_id" class="display"></table>
</div>
```
```js
<script>
var jsondata = `r jsondata`;

    $('#table_id').DataTable({
        data: jsondata,
        columns: [ { title: "Category" },
                   { title: "Comments" },
                   { title: "%" },
                   { title: "Net Easy" },
                   { title: "Able To Do" }
        ],
        columnDefs: [ { targets: [0], width: "40%"},
                      { targets: [1,2,3,4], width: "15%" },
		      { targets: [2,4], render: function ( data, type, row ) { return data*100 + "%"; } },
		      { targets: [3], createdCell: function(td, cellData, rowData, row, col) {
		      					var color = (cellData > 0) ? 'green' : 'red';
                                                        $(td).css('color', color);
							$(td).css('font-weight', 'bold');	} }
        ],
	dom: 't',
	lengthMenu: [5, 10, 15]
    });
</script>
```
DataTable relies on JQuery to work, and has some other methods that can update the table when an event occurs. These methods can be chained: 
```
$('#table_id').DataTable().clear();  // Removes all data from the table
$(#table_id).DataTable().rows.add(...);  // Adds multiple rows of data (must be supplied)
$(#table_id).DataTable().draw();  // Instruction to draw the table
$(#table_id).DataTable().clear().rows.add(...).draw();  // All methods chained together
```

# Plotly Bar in JS
```html
<div id="plotly_bar_id" style="width:627px;></div>
```
```js
<script>
var setup = [{ x: `r pct`,
               y: `r category`,
               type: 'bar',
               orientation: 'h'
            }];
            
var layout = {title: 'Category Distribution',
              xaxis: { title: "% in each category",
                       showgrid: false,
                       showline: true,
                       zeroline: false,
                       tickformat: "%"  },
              yaxis: { title: "",
                       showgrid: false,
                       showline: false },
              margin: {t: 25, l: 100, r: 25, b: 60}
              };

Plotly.newPlot('plotly_bar_id', setup, layout, {displayModeBar: false});
</script>
```

