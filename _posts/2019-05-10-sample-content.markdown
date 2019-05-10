---
layout: default
title:  "Assignment 4 Reingold-Tilford algorithm"
date:   2019-05-10 16:50:00
categories: main
---

# Reingold-Tilford algorithm（作业已提交，博客还未写完）

Create by Elroy-Tian SDU CS(class 2)

## Goal of algorithm

Make smarter use of space, maximize density and symmetry.Originally binary trees, extended by Walker to cover general case.Corrected by Buchheim et al. to achieve a linear time algorithm.

## Design Considerations

Clearly encode depth levelNo edge crossingsIsomorphic subtrees drawn identically Ordering and symmetry preserved Compact layout (don’t waste space)

## Algorithm principleInitial bottom-up (post-order) traversal of the treeY-coordinates based on tree depthX-coordinates set piecemeal via “shifts” at each depthAt each parent node: merge left and right subtreesShift right subtree as close as possible to the left Computed efficiently by maintaining subtree contours “Shifts” in position saved for each nodeParent nodes centered above childrenFinal top-down (pre-order) traversal to set X-coordinatesSum initial layout and aggregated shifts## Example: Radial Tree Layouts


## sHow to achieve it with d3?
D3’s tree layout implements the Reingold–Tilford “tidy” algorithm for constructing hierarchical node-link diagrams, improved to run in linear time by Buchheim et al. Tidy trees are typically more compact than cluster dendrograms, which place all leaves at the same level. See also the radial variant.## How to achieve radial?When setting the node's position, set the “transform” with "rotate( d.x - 90)translate( d.y )" 


----


<script src="https://d3js.org/d3.v3.min.js"></script>

<style type="text/css">

	.node {
		cursor: pointer;
	}

	.overlay{
		background-color:#EEE;
	}

	.node circle {
		fill: #fff;
		stroke: steelblue;
		stroke-width: 1.5px;
	}

	.node text {
		font-size:10px;
		font-family:sans-serif;
	}

	.link {
		fill: none;
		stroke: #ccc;
		stroke-width: 1.5px;
	}

	.templink {
		fill: none;
		stroke: red;
		stroke-width: 3px;
	}

	.ghostCircle.show{
		display:block;
	}

	.ghostCircle, .activeDrag .ghostCircle{
		display: none;
	}

</style>

<script type="text/javascript">
	d3.json("data/flare.json",function(error, root){
		var width = 1200;
		var height = 1200;
		var svg = d3.select("body")
						.append("svg")
						.attr("width", width)
						.attr("height", height);
		var	tree = d3.layout.tree()
					.size([360, width/2 -100])
					.separation(function(a,b){
						return (a.parent == b.parent ? 1 : 2) / a.depth;
					});
		var nodes = tree.nodes(root);
		var links = tree.links(nodes);
		console.log(nodes);
		var diagonal = d3.svg.diagonal.radial()
					.projection(function(d){
						var radius = d.y;
						var angle = d.x / 180 * Math.PI;
						return [radius , angle];
					});
		var g = svg.append("g")
				  	.attr("font-family", "sans-serif")
					.attr("font-size", 10)
					.attr("transform", 'translate('+width/2+','+height/2+')');


		var link = g.selectAll(".link")
				.data(links)
				.enter()
				.append("path")
				.attr("class" , "link")
				.attr("d" , diagonal);
		var node = g.selectAll(".node")
					.data(nodes)
					.enter()
					.append("g")
					.attr("class", "node")
					.attr("transform" , function(d){
						return "rotate(" + (d.x - 90) + ")translate(" + d.y + ")";
					});

		node.append("circle")
			.attr("r" , 4.5);
		node.append("text")
			.attr("transform" , function(d){
			return d.x<180 ? "translate(8)" : "rotate(180)translate(-8)";
		})
			.attr("dy" , ".3em")
			.style("text-anchor" , function(d){
			return d.x<180 ? "start" : "end";
		})
			.text(function(d){
			return d.name;
		})
		});
	
</script>
  
  
  
  
  
