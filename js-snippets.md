# Javascript Snippets

## Inputs
A CSV can be uploaded and converted to JSON: 
```html
<input type="file" id="inputFileId" name="inputFileName" onchange="importcsv()">
```
```js
function importcsv() {      
  var file = document.querySelector('#inputFileId').files[0]; 
  var reader = new FileReader();
	
  // Filereader is asynchronous so must wait for file to load until result can be used
  reader.onload = function fileReadCompleted(){
    // Convert csv to json using D3 (>= v5.9)
    dataset = d3.csvParse(reader.result, d3.autoType); 
    // ......
  };
  reader.readAsText(file);
}
```    

Alternatively, a textarea can added for users to paste a table from Excel. The cells are automatically tab seperated.
```html
<textarea id="textareaId" width="500px" height="250px"></textarea>
<input type="button" onclick="readText()" value="Submit"/>
```
```js
function readText() {
  var data = $('#textareaId').val();
  
  // Convert tsv to json using D3 (>= v5.9)
  dataset = d3.tsvParse(data, d3.autoType);  
}
```
