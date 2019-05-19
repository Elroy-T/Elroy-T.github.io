---
layout: default
title:  "Assignment 5 force-directed graph layout"
date:   2019-05-19 16:50:00
categories: main
---

# force-directed graph layout

Create by Elroy-Tian SDU CS(class 2)

## what is force-directed graph layout

Force-directed graph drawing algorithms are a class of algorithms for drawing graphs in an aesthetically-pleasing way. Their purpose is to position the nodes of a graph in two-dimensional or three-dimensional space so that all the edges are of more or less equal length and there are as few crossing edges as possible, by assigning forces among the set of edges and the set of nodes, based on their relative positions, and then using these forces either to simulate the motion of the edges and nodes or to minimize their energy.[2]

While graph drawing can be a difficult problem, force-directed algorithms, being physical simulations, usually require no special knowledge about graph theory such as planarity.

## Implementation method 1

So powerful D3.js is, that it provides a lot of methods which will be used when we make force-directed graph. So the easiest way to complete this assignment is to use D3's method.

---




<style>

	.link {
		fill: none;
		stroke: #666;
		stroke-width: 1.5px;
	}

	#licensing {
		fill: green;
	}

	.link.licensing {
		stroke: green;
	}

	.link.resolved {
		stroke-dasharray: 0,2,1;
	}

	circle {
		fill: #ccc;
		stroke: #333;
		stroke-width: 1.5px;
	}

	text {
		font: 12px Microsoft YaHei;
		pointer-events: none;
		text-shadow: 0 1px 0 #fff, 1px 0 0 #fff, 0 -1px 0 #fff, -1px 0 0 #fff;
	}

	.linetext {
		font-size: 12px;

	}

</style>
<body>
<script src="https://d3js.org/d3.v3.min.js"></script>
<script>

	// http://blog.thomsonreuters.com/index.php/mobile-patent-suits-graphic-of-the-day/
	var links = [
		{source: "APPLE", target: "MAC", type: "resolved", rela:"主营产品"},
		{source: "APPLE", target: "iPad", type: "resolved", rela:"主营产品"},
		{source: "APPLE", target: "Apple Watch", type: "resolved", rela:"主营产品"},
		{source: "APPLE", target: "AirPods", type: "resolved", rela:"主营产品"},
		{source: "APPLE", target: "HomePod", type: "resolved", rela:"主营产品"},
		{source: "APPLE", target: "Apple Pencil", type: "resolved",rela:"主营产品"},
		{source: "APPLE", target: "iPhone", type: "resolved",rela:"主营产品"},
		{source: "MAC", target: "iMAC", type: "resolved", rela:"支线产品"},
		{source: "MAC", target: "MacBook", type: "resolved", rela:"支线产品"},
		{source: "MAC", target: "MacBook Air", type: "resolved", rela:"支线产品"},
		{source: "MAC", target: "MacBook Pro", type: "resolved", rela:"支线产品"},
		{source: "iPhone", target: "iPhone XS", type: "resolved", rela:"支线产品"},
		{source: "iPhone", target: "iPhone XS MAX", type: "resolved", rela:"支线产品"},
		{source: "iPhone", target: "iPhone XR", type: "resolved", rela:"支线产品"},
		{source: "iPad", target: "iPad Air", type: "resolved", rela:"支线产品"},
		{source: "iPad", target: "iPad Pro", type: "resolved", rela:"支线产品"},
		{source: "AirPods", target: "AirPods 1", type: "resolved", rela:"支线产品"},
		{source: "AirPods", target: "AirPods 2", type: "resolved", rela:"支线产品"},
		{source: "HUAWEI", target: "手机", type: "resolved", rela:"主营产品"},
		{source: "HUAWEI", target: "平板", type: "resolved", rela:"主营产品"},
		{source: "HUAWEI", target: "路由器", type: "resolved", rela:"主营产品"},
		{source: "HUAWEI", target: "手表", type: "resolved", rela:"主营产品"},
		{source: "HUAWEI", target: "耳机", type: "resolved", rela:"主营产品"},
		{source: "HUAWEI", target: "电脑", type: "resolved", rela:"主营产品"},
		{source: "手机", target: "P30", type: "resolved", rela:"支线产品"},
		{source: "手机", target: "P30 Pro", type: "resolved", rela:"支线产品"},
		{source: "手机", target: "Mate 20", type: "resolved", rela:"支线产品"},
		{source: "手机", target: "Mate 20 Pro", type: "resolved", rela:"支线产品"},
		{source: "手机", target: "iPhone", type: "resolved", rela:"竞争产品"},
		{source: "平板", target: "iPad", type: "resolved", rela:"竞争产品"},
		{source: "平板", target: "M3", type: "resolved", rela:"支线产品"},
		{source: "平板", target: "M5", type: "resolved", rela:"支线产品"},
		{source: "电脑", target: "MAC", type: "resolved", rela:"竞争产品"},
		{source: "电脑", target: "MateBook 14", type: "resolved", rela:"支线产品"},
		{source: "电脑", target: "MateBook X", type: "resolved", rela:"支线产品"},
	

	];

	var nodes = {};

	links.forEach(function(link) {
		link.source = nodes[link.source] || (nodes[link.source] = {name: link.source});
		link.target = nodes[link.target] || (nodes[link.target] = {name: link.target});
	});

	var width = 1560,
			height = 1200;

	var force = d3.layout.force()//layout将json格式转化为力学图可用的格式
			.nodes(d3.values(nodes))//设定节点数组
			.links(links)//设定连线数组
			.size([width, height])//作用域的大小
			.linkDistance(180)//连接线长度
			.charge(-1500)//顶点的电荷数。该参数决定是排斥还是吸引，数值越小越互相排斥
			.on("tick", tick)//指时间间隔，隔一段时间刷新一次画面
			.start();//开始转换

	var svg = d3.select("body").append("svg")
			.attr("width", width)
			.attr("height", height);

	//箭头
	var marker=
			svg.append("marker")
			//.attr("id", function(d) { return d; })
					.attr("id", "resolved")
					//.attr("markerUnits","strokeWidth")//设置为strokeWidth箭头会随着线的粗细发生变化
					.attr("markerUnits","userSpaceOnUse")
					.attr("viewBox", "0 -5 10 10")//坐标系的区域
					.attr("refX",32)//箭头坐标
					.attr("refY", -1)
					.attr("markerWidth", 12)//标识的大小
					.attr("markerHeight", 12)
					.attr("orient", "auto")//绘制方向，可设定为：auto（自动确认方向）和 角度值
					.attr("stroke-width",2)//箭头宽度
					.append("path")
					.attr("d", "M0,-5L10,0L0,5")//箭头的路径
					.attr('fill','#000000');//箭头颜色

	/* 将连接线设置为曲线
    var path = svg.append("g").selectAll("path")
        .data(force.links())
        .enter().append("path")
        .attr("class", function(d) { return "link " + d.type; })
        .style("stroke",function(d){
            //console.log(d);
           return "#A254A2";//连接线的颜色
        })
        .attr("marker-end", function(d) { return "url(#" + d.type + ")"; });
    */

	//设置连接线
	var edges_line = svg.selectAll(".edgepath")
			.data(force.links())
			.enter()
			.append("path")
			.attr({
				'd': function(d) {return 'M '+d.source.x+' '+d.source.y+' L '+ d.target.x +' '+d.target.y},
				'class':'edgepath',
				//'fill-opacity':0,
				//'stroke-opacity':0,
				//'fill':'blue',
				//'stroke':'red',
				'id':function(d,i) {return 'edgepath'+i;}})
			.style("stroke",function(d){
				var lineColor;
				//根据关系的不同设置线条颜色
				if(d.rela=="上位产品" || d.rela=="上游" || d.rela=="下位产品"||d.rela=="支线产品"||d.rela=="竞争产品"){
					lineColor="#A254A2";
				}else if(d.rela=="主营产品"){
					lineColor="#B43232";
				}
				return lineColor;
			})
			.style("pointer-events", "none")
			.style("stroke-width",0.5)//线条粗细
			.attr("marker-end", "url(#resolved)" );//根据箭头标记的id号标记箭头

	var edges_text = svg.append("g").selectAll(".edgelabel")
			.data(force.links())
			.enter()
			.append("text")
			.style("pointer-events", "none")
			//.attr("class","linetext")
			.attr({  'class':'edgelabel',
				'id':function(d,i){return 'edgepath'+i;},
				'dx':80,
				'dy':0
				//'font-size':10,
				//'fill':'#aaa'
			});

	//设置线条上的文字
	edges_text.append('textPath')
			.attr('xlink:href',function(d,i) {return '#edgepath'+i})
			.style("pointer-events", "none")
			.text(function(d){return d.rela;});

	//圆圈
	var circle = svg.append("g").selectAll("circle")
			.data(force.nodes())//表示使用force.nodes数据
			.enter().append("circle")
			.style("fill",function(node){
				var color;//圆圈背景色
				var link=links[node.index];
				if(node.name==link.source.name && link.rela=="主营产品"){
					color="#F6E8E9";
				}else{
					color="#69ABff";
				}
				return color;
			})
			.style('stroke',function(node){
				var color;//圆圈线条的颜色
				var link=links[node.index];
				if(node.name==link.source.name && link.rela=="主营产品"){
					color="#B43232";
				}else{
					color="#5254A2";
				}
				return color;
			})
			.attr("r", 28)//设置圆圈半径
			.on("click",function(node){
				//单击时让连接线加粗
				edges_line.style("stroke-width",function(line){
					console.log(line);
					if(line.source.name==node.name || line.target.name==node.name){
						return 4;
					}else{
						return 0.5;
					}
				});
				//d3.select(this).style('stroke-width',2);
			})
			.call(force.drag);//将当前选中的元素传到drag函数中，使顶点可以被拖动

	//圆圈的提示文字
	circle.append("svg:title")
			.text(function(node) {
				var link=links[node.index];
				if(node.name==link.source.name && link.rela=="主营产品"){
					return "双击可查看详情"
				}
			});

	var text = svg.append("g").selectAll("text")
			.data(force.nodes())
			//返回缺失元素的占位对象（placeholder），指向绑定的数据中比选定元素集多出的一部分元素。
			.enter()
			.append("text")
			.attr("dy", ".35em")
			.attr("text-anchor", "middle")//在圆圈中加上数据
			.style('fill',function(node){
				var color;//文字颜色
				var link=links[node.index];
				if(node.name==link.source.name && link.rela=="主营产品"){
					color="#B43232";
				}else{
					color="#A254A2";
				}
				return color;
			}).attr('x',function(d){
				// console.log(d.name+"---"+ d.name.length);
				var re_en = /[a-zA-Z]+/g;
				//如果是全英文，不换行
				if(d.name.match(re_en)){
					d3.select(this).append('tspan')
							.attr('x',0)
							.attr('y',2)
							.text(function(){return d.name;});
				}
				//如果小于四个字符，不换行
				else if(d.name.length<=4){
					d3.select(this).append('tspan')
							.attr('x',0)
							.attr('y',2)
							.text(function(){return d.name;});
				}else{
					var top=d.name.substring(0,4);
					var bot=d.name.substring(4,d.name.length);

					d3.select(this).text(function(){return '';});

					d3.select(this).append('tspan')
							.attr('x',0)
							.attr('y',-7)
							.text(function(){return top;});

					d3.select(this).append('tspan')
							.attr('x',0)
							.attr('y',10)
							.text(function(){return bot;});
				}
				//直接显示文字
				/*.text(function(d) {
                return d.name; */
			});


	function tick() {
		//path.attr("d", linkArc);//连接线
		circle.attr("transform", transform1);//圆圈
		text.attr("transform", transform2);//顶点文字

		edges_line.attr('d', function(d) {
			var path='M '+d.source.x+' '+d.source.y+' L '+ d.target.x +' '+d.target.y;
			return path;
		});
		//旋转边上的文字
		edges_text.attr('transform',function(d,i){
			if (d.target.x<d.source.x){
				bbox = this.getBBox();
				rx = bbox.x+bbox.width/2;
				ry = bbox.y+bbox.height/2;
				return 'rotate(180 '+rx+' '+ry+')';
			}
			else {
				return 'rotate(0)';
			}
		});
	}

	//设置连接线的坐标,使用椭圆弧路径段双向编码
	function linkArc(d) {

		return 'M '+d.source.x+' '+d.source.y+' L '+ d.target.x +' '+d.target.y
	}
	//设置圆圈和文字的坐标
	function transform1(d) {
		return "translate(" + d.x + "," + d.y + ")";
	}
	function transform2(d) {
		return "translate(" + (d.x) + "," + d.y + ")";
	}



</script>









---



## Design Considerations

Clearly encode depth levelNo edge crossingsIsomorphic subtrees drawn identically Ordering and symmetry preserved Compact layout (don’t waste space)

## Algorithm principleInitial bottom-up (post-order) traversal of the treeY-coordinates based on tree depthX-coordinates set piecemeal via “shifts” at each depthAt each parent node: merge left and right subtreesShift right subtree as close as possible to the left Computed efficiently by maintaining subtree contours “Shifts” in position saved for each nodeParent nodes centered above childrenFinal top-down (pre-order) traversal to set X-coordinatesSum initial layout and aggregated shifts## Example: Radial Tree Layouts

### example 1 (data: flare-one.json):

![tree1](/picture/tree1.png) 

### example 2 (data: flare.json):

![tree1](/picture/tree2.png)

## How to achieve it with d3?
D3’s tree layout implements the Reingold–Tilford “tidy” algorithm for constructing hierarchical node-link diagrams, improved to run in linear time by Buchheim et al. Tidy trees are typically more compact than cluster dendrograms, which place all leaves at the same level. See also the radial variant.

```
var	tree = d3.layout.tree()
					.size([360, width/2 -100])
					.separation(function(a,b){
						return (a.parent == b.parent ? 1 : 2) / a.depth;
					});
```## How to achieve radial?When setting the node's position, set the “transform” with "rotate( d.x - 90)translate( d.y )" ```
var node = g.selectAll(".node")
					.data(nodes)
					.enter()
					.append("g")
					.attr("class", "node")
					.attr("transform" , function(d){
						return "rotate(" + (d.x - 90) + ")translate(" + d.y + ")";
					});
```


----

## Visualization 


  ---
## main part of the Code

```

```  
  
  
  
