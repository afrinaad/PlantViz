# PlantViz – Plant data visualization system in University of Malaya

## What is PlantViz?
PlantViz is a graphical viewer platform to display the retrieved results based on the query from user in the form of network graph. The main goal is to present information of plant in a visual form, to allow user to see how information of plant is related to each other. It is also to highlight important data to the user instead of showing all information at one time. PlantViz also enables user to interact with the data directly. 

## The PlantViz Flowchart
![PlantViz flowchart][flowchart]

## The PlantViz implementation

#### The Plant Ontology UM (POUM): ‘poum.owl’ file
This file consists of annotated plant data on tree and shrub samples collected in University of Malaya. In the original ontology file we have approximately 129 samples and 43 species of 42 genus for trees and 93 samples and 31 species of 28 genus for shrubs. However, for the reviewing purposes, only 3 tree samples and 3 shrub samples are provided in this ontology. The images of 6 samples of trees and shrubs are provided in folder `images`.

#### The images: ‘images’ folder
 There are 18 images in this folder.
- Tree images
  1)	Delonix regia (DELREGO-B001.jpg, DELREGO-R001.jpg, DELREGO-T001.jpg)
  2)	Hura crepitans (HURCREO-B001.jpg, HURCREO-R001.jpg, HURCREO-T001.jpg)
  3)	Tabebuia rosea (TABROSO-B001.jpg, TABROSO-R001.jpg, TABROSO-W001.jpg)


- Shrub images
  1)	Ixora javanica (IXOJAVO-B001.jpg, IXOJAVO-R001.jpg, IXOJAVO-T001.jpg)
  2)	Dillenia suffruticosa (DILSUFO-B001.jpg, DILSUFO-R001.jpg, DILSUFO-L001.jpg)
  3)	Allamanda cathartica (ALLCATO-B001.jpg, ALLCATO-R001.jpg, ALLCATO-T001.jpg)

---

#### The PlantViz query page: `query.jsp`
There are three parameters which are ‘Scientific Name’, ‘Family Name’, and ‘Location’ provided in the PlantViz GUI. User has to select one of these parameters to send as a query.

<span style="font-size:4em">*Snippet code from `query.jsp`*</span>
```
<form action="dataQuery" method="post">
<div>Search for &nbsp;:&nbsp; 
<input class="enjoy-css" style="width:120px" placeholder="Search" type="text" name="dataquery1">
&nbsp;&nbsp;&nbsp;
<select class="enjoy-css" style="width:170px" name="optdata">		        
<option value="scname">Scientific Name</option>
<option value="fname">Family Name</option>
<option value="location">Location</option>
</select>
&nbsp;&nbsp;&nbsp;
<input class="enjoy-css" type="submit" name="Search" value="Search">
</div>
</form>	
```
	
The PlantViz search engine: `dataQuery` servlet
SPARQL query is almost similar to each other.The only difference is only on `FILTER` command as shown below.

<span style="font-size:4em">*Snippet code from `dataQuery` servlet*</span>

If variable `optdata` equals to `Scientific Name` : `FILTER (?scname = \"" + dataquery1 + "\")`
  
If variable `optdata` equals to `Family Name` : `FILTER (?fname = \"" + dataquery1 + "\")`
  
If variable `optdata` equals to `Location` : `FILTER (?loc = \"" + dataquery1 + "\")`

The result is then stored as an array data type named `resultlist`.
```
//get result into array resultlist
if (plantName != null) {
	resultlist.add(scientificName.toString());
  resultlist.add(plantName.toString());
  resultlist.add(familyName.toString());
  resultlist.add(sample);
	resultlist.add(plantLoc.toString());
	resultlist.add(classDesc.toString());
  
	resultlist.add(pdTree);
  resultlist.add(dtreeHeight.toString());
  
	resultlist.add(pdFlo);
  resultlist.add(dflowerType.toString());					
  resultlist.add(dflowerColor.toString());
```

Array `resultlist` is then stored into 2 lists data type named `nodes` and `links` for JSON file. 
```
List<PlantArray> nodes = new ArrayList<>();

for (int d = 0; d < resultlist.size(); d = d + 49) {			
//general info
for (int a = d; a < d+5; a++) {
nodes.add(new PlantArray(resultlist.get(a),1));
}
...
List<PlantLink> links = new ArrayList<>();

for (int d = 0; d < resultlist.size(); d = d + 49) {
//general	
for (int a = 0; a < d+6; a++) {
	links.add(new PlantLink(a,d,10));
	}
```

JSON file is then created and stored in folder `WebContent`

```
PlantList answer = new PlantList(nodes, links);
Gson gson = new Gson();
	String plant = gson.toJson(answer);
	System.out.println("plantJSON = " + plant);
		
	try (FileWriter file = new FileWriter("c:/users/user/workspace/plantont/WebContent/plant.json")) {
			file.write(plant);
}
```

#### The PlantViz result page: ‘viewData.jsp’
The network graph is developed using `d3.js` library. D3 is a JavaScript library for visualizing data that involves the combination of SVG, HTML and CSS.

JSON file named `plant.json` is read by d3.json and each nodes and links from JSON are assigned into variables `node` and `link` respectively. (For reference Figure 9)

<span style="font-size:4em">*Snippet code from `dataQuery` servlet*</span>

```
//read JSON file created
d3.json("plant.json", function(error, json) {
  if (error) throw error;
				
    force
      .nodes(json.nodes)
      .links(json.links)
      .start();

    var link = svg.selectAll(".link")
               .data(json.links)
               .enter().append("line")
               .attr("class", "link");

    node = svg.selectAll(".node")
          .data(json.nodes)
          .enter().append("g")
          .attr("class", "node")
          .call(force.drag)
 ```

Interactivity is also added to encourage the communication between users and data. Images of plant are shown whenever the user hovers the cursor on the node containing plant scientific name. 
```
.on("mouseover", function(d) {

  //display images at specific node source == 0
  if(d.index === 0) {
    div.transition()
        .duration(200)
        .style("opacity", .9);

  var string = 
      "<img src=\"images/psimg/DELREGO-B001.jpg\"/>&nbsp;&nbsp;"+
      "<img src=\"images/psimg/DELREGO-R001.jpg\"/>&nbsp;&nbsp;" +
      "<img src=\"images/psimg/DELREGO-T001.jpg\"/>";	
  
  div.html(string)
    .style("left", (d3.event.pageX + 10) + "px")
    .style("top", (d3.event.pageY + 50) + "px")
    .style("font-color", "white");
  }
})
```

By double clicking on node, it will show neighbouring nodes that are connected to the clicked node.

```
function connectedNodes() {

  if (toggle == 0) {

    //Reduce the opacity of all but the neighbouring nodes
    d = d3.select(this).node().__data__;

    node.style("opacity", function (o) {
      return neighboring(d, o) | neighboring(o, d) ? 1 : 0.1;
	  });

    link.style("opacity", function (o) {
    return d.index==o.source.index | d.index==o.target.index ? 1 : 0.1;
		});

    //Reduce the op
    toggle = 1;
  }

  else {
    //Put them back to opacity=1
    node.style("opacity", 1);
    link.style("opacity", 1);
    toggle = 0;
  }
}
```

## References

More information on tools used in this project.

* Eclipse IDE - https://eclipse.org/
* SPARQL Query Language - https://www.w3.org/TR/rdf-sparql-query/
* Jackson - https://github.com/FasterXML/jackson
* D3.js - https://d3js.org/

[flowchart]:https://github.com/afrinaad/PlantViz/blob/master/img/github%20-%20flowchart.png "PlantViz flowchart"
<span style="font-size:8px">**LAST UPDATED 25 Jan 2018**</span>


