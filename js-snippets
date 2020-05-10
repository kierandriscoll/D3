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
	  // Convert csv to json using D3
    dataset = d3.csvParse(reader.result); 
    // ......
  };
      
	reader.readAsText(file);
}
```    

Alternatively, a textbox can added for users to paste a table from Excel. The cell are automatically tab seperated.
```html
<textarea id="textareaId" width="500px" height="250px"></textarea>
<input type="button" onclick="readText()" value="Submit"/>
```
```js
function readText() {
  var data = $('#textareaId').val();
  
  // Convert tsv to json using D3
  dataset = d3.tsvParse(data);  
}
```
