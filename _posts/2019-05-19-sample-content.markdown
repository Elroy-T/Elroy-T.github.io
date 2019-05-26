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

<div>
    <svg width="1560" height="1200" ></svg>
</div>
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

	var svg = d3.select("svg").append("svg")
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




## Implementation method 2


### Barnes-Hut algorithm (use quad-tree)

The Barnes–Hut simulation (named after Josh Barnes and Piet Hut) is an approximation algorithm for performing an n-body simulation. It is notable for having order O(n log n) compared to a direct-sum algorithm which would be O(n2).[1]

The simulation volume is usually divided up into cubic cells via an octree (in a three-dimensional space), so that only particles from nearby cells need to be treated individually, and particles in distant cells can be treated as a single large particle centered at the cell's center of mass (or as a low-order multipole expansion). This can dramatically reduce the number of particle pair interactions that must be computed.




## Forces

Force-directed graph drawing algorithms assign forces among the set of edges and the set of nodes of a graph drawing. Typically, spring-like attractive forces based on Hooke's law are used to attract pairs of endpoints of the graph's edges towards each other, while simultaneously repulsive forces like those of electrically charged particles based on Coulomb's law are used to separate all pairs of nodes. In equilibrium states for this system of forces, the edges tend to have uniform length (because of the spring forces), and nodes that are not connected by an edge tend to be drawn further apart (because of the electrical repulsion). Edge attraction and vertex repulsion forces may be defined using functions that are not based on the physical behavior of springs and particles; for instance, some force-directed systems use springs whose attractive force is logarithmic rather than linear.

### Repulsive Force

Considering each node as a electron, the repulsion force between electron and electron is Coulomb force. According to Coulomb's law, the repulsion force between electrons can be calculated as follows:$\frac{d}{dx}e^{ax}=ae^{ax}\quad$

$$
	F_1=k_1\frac{(q_1)(q_2)}{r^2}
$$

```
$$\frac{d}{dx}e^{ax}=ae^{ax}\quad$$
```

### Traction Force

The particles connected by the edges are involved by the edges, which generate spring-like Hooke gravity:

$$F_2=k_2*(L-\Delta x)$$

$$ x^{y^z}=(1+{\rm e}^x)^{-2xy^w} $$

## Quad-Tree


### Constructing a Quad-Tree

To build a Barnes-Hut tree, we insert nodes one by one into the tree. Specifically, when inserting an object B into a tree represented by (root) node X, we recursively perform the following steps (note that here we add a bracket to the word "root" to mean that even if a node in the whole tree is not a root node, it may be the root node of one of the subtrees):

If the (root) node X does not contain objects, the new object B is put into it.

If the (root) node X is an internal node (i.e. a non-leaf node), the total mass and centroid of X are updated. The object B is inserted recursively into one of the four quadrants (or bifurcations).

If (root) node X is an external node (leaf node), it contains an object C, that is to say, there are two objects B and C in the same region. Then the region is further divided into four sub-regions. Then the objects B and C are inserted into the corresponding bifurcation recursively. Because B and C may still be in the same subregion, multiple subregion partitions may be involved in a single insertion operation. Finally, the centroid and total mass of node X are updated.









## Visualization 
<script src="/js/jquery-3.3.1.min.js"></script>
<script src="/js/quadtree.js"></script>
<script src="/js/test.js"></script>
<div style="text-align: center;">
    <div>
        <h1>Force-Directed layout graph</h1>
    </div>
<div><p>Points number：
<input type="text" name="p_num" value="" id="p_num" onblur="pnum(this)" />
<input type="button" value="Show" onclick="fun()"/></p>
</div>
    <div>
        <canvas id="canvas" width="1600" height="1600" style="border: 3px solid rgb(190, 120, 120);width:800px;height:800px"></canvas>
    </div>
    <div>
        <label id="cord">0,0</label>
    </div>
</div>
<script>
    function fun(){
        let point_num = p_num.value;
        console.log(point_num);
        inti();
    }
</script>



## main part of the Code

```


function ForceDirected(){

    //let num = 15;
    let num = point_num;
    console.log("inti "+point_num);
    // 随机生成一颗树
    //生成点
    for (let i = 0; i < num; i++) {
        NodeList.push(new Node(i));
        f_x[i] = 0;
        f_y[i] = 0;
    }
    //生成边
    for (let i = 0; i < num; i++) {
        let edgeCount = Math.random() * 3 + 1;
        for (let j = 0; j < edgeCount; j++) {
            let targetId = Math.floor(Math.random() * num);
            let edge = new Edge(i, targetId);
            EdgeList.push(edge);
        }
    }

    //生成四叉树
    quad_tree = new QuadTree({
        x: 0,
        y: 0,
        width: 1600,
        height: 1600
    },1);
    //随机生成坐标.
    let initialX, initialY, initialSize = CANVAS_WIDTH * .8;
    for (let i in NodeList) {
        initialX = CANVAS_WIDTH * .5;
        initialY = CANVAS_HEIGHT * .5;
        NodeList[i].x = initialX + initialSize * (Math.random() - .5);
        NodeList[i].y = initialY + initialSize * (Math.random() - .5);

        quad_tree.insert( {
            id: NodeList[i].id,
            x: NodeList[i].x,
            y: NodeList[i].y,
            width: 10,
            height: 10
        });
    }

    self.setInterval("do_time()",16);

    // cal_spring();

    // Draw();
    // cal_repulsive();
    // console.log(quad_tree);
}
function do_time()
{
    cal_repulsive();
    cal_spring();

    apply_force();
    refresh_cord(quad_tree);
    refresh_all_mid(quad_tree);
    Draw();
}
function cal_repulsive()
{
    let c_node;

    for(let i in NodeList)
    {
        c_node = NodeList[i];
        let r_force =  cal_single_force(quad_tree, c_node);
        f_x[i] += r_force[0];
        f_y[i] += r_force[1];
    }

}
//计算任意两点之间的斥力
function cal_single_force(node, c_node)
{
    let force;
    let force_x=0;
    let force_y=0;
    let dis;
    let x;
    let y;

    x = node.mid_x - c_node.x;
    y = node.mid_y - c_node.y;
    dis = Math.sqrt(Math.pow(x,2)+Math.pow(y,2));
    if(dis/CANVAS_WIDTH < theta)
    {
        force = kr/Math.pow(dis,2);
        force_x = force * ((c_node.x - node.mid_x) / dis);
        force_y = force * ((c_node.y - node.mid_y) / dis);

        return[force_x, force_y];
    }else
    {
        for(let i=0; i<node.nodes.length; i++)
        {
            let r_force =  cal_single_force(node.nodes[i], c_node);
            force_x += r_force[0];
            force_y += r_force[1];
        }
        return[force_x, force_y];
    }
}
//计算边上的引力
function cal_spring()
{
    let f;
    let t;
    let dis;
    let force;
    let x;
    let y;
    let force_x;
    let force_y;
    for(let i in EdgeList)
    {
        f = NodeList[EdgeList[i].source];
        t = NodeList[EdgeList[i].target];
        x = Math.abs(f.x - t.x);
        y = Math.abs(f.y - t.y);
        dis = Math.sqrt(Math.pow(x, 2) + Math.pow(y, 2));

        force = (L - dis) * ks;//force>0 斥力;force<0 引力
        //force = (dis - L) * ks;

        force_x = x !==0 && dis !==0 ? force * (x / dis) : 0;
        force_y = y !==0 && dis !==0 ? force * (y / dis) : 0;

        if(f.x > t.x)
        {
            f_x[f.id] += force_x;
            f_x[t.id] -= force_x;
            // console.log(f_x[f.id]);
        }else
        {
            f_x[f.id] -= force_x;
            f_x[t.id] += force_x;
        }

        if(f.y > t.y)
        {
            f_y[f.id] += force_y;
            f_y[t.id] -= force_y;
        }else
        {
            f_y[f.id] -= force_y;
            f_y[t.id] += force_y;
        }
    }
}

```  
  
  
  
### Inspired by:

  https://www.jianshu.com/p/d3c64a39535a
  
  https://blog.csdn.net/baimafujinji/article/details/53036473?locationNum=5&fps=1

  
  
