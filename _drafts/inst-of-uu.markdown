---
layout: post
title:  "Institutes of Uppsala university"
categories: data-viz
---
A node tree of the institutes of Uppsala university.


<style>
	.teo:before {	width: 10px;
				height: 10px;
				-moz-border-radius:50%;
				-webkit-border-radius: 50%;
				border-radius: 50%;
				float:left;
	}

	.node circle {
		fill: #fff;
		stroke: steelblue;
		stroke-width: 1.5px;
	}
	
	.node {
		font: 10px sans-serif;
	}
	
	#uppsala {stroke: #000; fill: #fff;}
	#humsam {stroke: olivedrab;}
		#teo{stroke:#799938; border:3px solid #799938;}
		#jur{stroke:#88a44e;}
		#his{stroke:#97af65;}
		#spr{stroke:#a6bb7b;}
		#sam{stroke:#b5c691;}
		#utb{stroke:#c3d1a7;}
	#medfar {stroke: #386890;}
		#med{stroke:#4682b4;}
		#far{stroke:#6a9bc3;}
	#teknat {stroke: firebrick;}
		#mat{stroke:#b93838;}
		#fys{stroke:#c14e4e;}
		#tek{stroke:#c96464;}
		#kem{stroke:#d07a7a;}
		#bio{stroke:#d89090;}
		#geo{stroke:#e0a6a6;}

	.link {
		fill: none;
		stroke: #ccc;
		stroke-width: 1.5px;
	}
</style>

<script src="http://d3js.org/d3.v3.min.js"></script>
<script>

var diameter = 720;

var colors = [ ["Local", "#377EB8"],
					["Global", "4dad4a"]];

var tree = d3.layout.tree()
	.size([360, diameter/2 - 120])
	.separation(function(a, b) {return (a.parent == b.parent ? 1 : 2) / a.depth;});

var diagonal = d3.svg.diagonal.radial()
	.projection(function(d) { return [d.y, d.x / 180*Math.PI]; });

var svg = d3.select("article").append("svg")
	.attr("width", diameter + 100)
	.attr("height", diameter + 100)
	.append("g")
	.attr("transform", "translate("+diameter/2+","+diameter/2+")");

d3.json("/assets/data.json", function(error, root) {
	var nodes = tree.nodes(root),
		links = tree.links(nodes);

	var link = svg.selectAll(".link")
		.data(links)
		.enter().append("path")
		.attr("class", "link")
		.attr("d", diagonal);

	var node = svg.selectAll(".node")
		.data(nodes)
		.enter().append("g")
		.attr("class", "node")
		.attr("transform", function(d) { return "rotate("+(d.x-90)+")translate("+d.y+")"; });

	node.append("circle")
		.attr("r", 4.5)
		.attr("id", function(d) {
				if (typeof d.id != "undefined") return d.id;
			}
		);

	node.append("text")
		.attr("dy", ".31em")
		.attr("text-anchor", function(d) {return d.x < 180 ? "start" : "end" ; })
		.attr("transform", function(d) { return d.x < 180 ? "translate(8)" : "rotate(180)translate(-8)"; })
		.text(function(d) {
			if(d.name == "1" || d.name == "2" || d.name == "3") return "";
			var patt = /(fakulteten$|sektionen$|^Fakulteten|^Uppsala$)/;
			if(patt.test(d.name)) return "";
			return d.name; 
		});

/*	var legend = svg.selectAll(".legend")
		.data(color.domain())
		.enter().append("g")
		.attr("class", "legend")
		.attr("transform", function(d, i) {return "translate(0," + i * 20 + ")";});

	legend.append("rect")
		.attr("x", diameter -18)
		.attr("width", 18)
		.attr("height", 18)
		.style("fill", color);

	legend.append("text")
		.attr("x", diameter-24)
		.attr("y", 9)
		.attr("dy", ".31em")
		.style("text-anchor", "end")
		.text(function(d) {return d; });*/

	var legend = svg.append("g")
		.attr("class", "legend")
		.attr("height", 100)
		.attr("width", 100);

	var legendRect = legend.selectAll("rect").data(colors);

	legendRect.enter()
		.append("rect")
		//.attr("x", diameter/2)
		//.attr("y", diameter/2)
		.attr("width", 10)
		.attr("height", 10)
		.attr("transform", "translate("+ (diameter/2 - 100) +","+diameter/2+")");

	legendRect
		.attr("y", function(d,i) { return i*20+9;})
		.style("fill", function(d) { return d[1];});

	var legendText = legend.selectAll("text").data(colors);

	legendText.enter()
		.append("text")
		.attr("x", diameter/2);

	legendText
		.attr("y", function(d,i) { return i*20+9;})
		.text(function(d) { return d[0];});

});

d3.select(self.frameElement).style("height", diameter - 150 + "px");
</script>


