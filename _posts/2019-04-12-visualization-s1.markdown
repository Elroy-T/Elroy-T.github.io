---
title:  "Assignment 1 US_population"
date:   2019-03-12 17:50:00
categories:
  - Visualization
elements:
  - visualization
  - d3
---

The Population Pyramid is a graphic depicting the age and gender distribution of the population in an image similar to the ancient Egyptian pyramids, showing the status of the population and its type of development. 

# The Population Pyramid of USA

Create by Elroy-Tian SDU CS(class 2)

The Population Pyramid is a graphic depicting the age and gender distribution of the population in an image similar to the ancient Egyptian pyramids, showing the status of the population and its type of development. The population pyramid can be summarized into three types: (a) Expansion type. The proportion of the young population group is relatively large, gradually decreasing from the lowest age group to the highest age group, and the tower shape is wide and sharp. (b) Stable type. Except for the oldest age group, the other age groups are roughly the same, and the enlargement or reduction is not obvious, and the tower shape is relatively straight. (c) Shrinkage type. The young population group has gradually narrowed down. The proportion of each group in the middle-aged group is larger, and the tower shape is narrower and wider.

After a century of change, the population of the United States has increased significantly and the population structure has changed. However，it is difficult to reflect the changes in demographic structure by observing the data in the CSV file. Therefore, I want to reflect the changes in the population structure of the United States by drawing a pyramid of American population at these two time points.

Firstly, I decide to use a bar chart as the main body, because the bar chart can better reflect the difference in the number of different groups. Secondly, in the use of color, I decide to use blue for men and pink for women, where light blue and light pink represent 2000 year’s data, dark blue and dark represent 1900 year’s data. Especially，I struggled with choosing dark or light color for 2000. by comparing the visualization results of these two color schemes, I found that because the number of people of all ages in 2000 was much more than 1900, the color scheme of using light colors to indicate 2000 year’s data can simultaneously make the shape of the two pyramids more visible.


----


  
<script src="https://d3js.org/d3.v3.min.js" charset="utf-8"></script>
<script src="https://d3js.org/d3-color.v1.min.js"></script>
<style>

 #content {
			text-align:center;

    }
    .axis path,
    .axis line{
        fill: none;
        stroke: black;
        shape-rendering: crispEdges;
    }
    .axis text{
        font-family: sans-serif;
        font-size: 11px;
    }
    svg {
        margin-left: auto;
        margin-right: auto;
        display: block;
        text-align:center;
    }

    .bar.right {
        fill: steelblue;
    }

    .bar.left {
        fill: brown;
    }

    .axis text {
        font: 10px sans-serif;
    }

    .axis path,
    .axis line {
        fill: none;
        stroke: #000;
        shape-rendering: crispEdges;
    }

    .grid .tick {
        stroke: #FFF;
        opacity: 0.7;
        stroke-width: 0.5;
    }

    .grid path {
        stroke-width: 0;
    }
</style>


<div id="content1">
    <svg width="1500" height="800" ></svg>
</div>

<script >
    // 添加SVG画布
    //var dataset =[885,325,285,706 ];//['crew',885], ['first',325], ['second',285], ['third',706]  [885],[325],[285],[706]  885,325,285,706
    var categories = ['0-4', '5-9', '10-14', '15-19',
        '20-24', '25-29', '30-34', '35-39', '40-44',
        '45-49', '50-54', '55-59', '60-64', '65-69',
        '70-74', '75-79', '80-84', '85-89', '90+'
        ];

    var dataset=[];
    var sex=[];
    var year=[];
    var age=[];
    var people=[];


    //var key =['crew','first','second','third'];
    var width = 1500;    // SVG的宽度
    var height = 800;   // SVG的长度
    var svg = d3.select("svg")
        //.append('svg')  // body中添加SVG
        .attr('width', width)
        .attr('height', height);
    var padding = {top: 20, right: 50, bottom: 70, left: 50};

    // 定义数据与比例尺
    var xAxisWidth = 1000;   //x轴宽度
    var yAxisWidth = 600;   //y轴宽度
    var xrangeWidth = 900;   //x轴宽度
    var yrangeWidth = 550;   //y轴宽度

    var xScale = d3.scale.linear()  //x轴比例尺（线性比例尺）
       // .domain([0,d3.max(people, function(d){ return d;})])
        //.range([0+padding.left,xAxisWidth-padding.right]);
        .range([0,xAxisWidth]);
    var yScale = d3.scale.ordinal() //y轴比例尺（序数比例尺）
    //.domain(d3.range(dataset.length))//d3.range(dataset.length)
        //.domain(categories)
        .rangeRoundBands([yAxisWidth,0],0.2);
        //yScale.range([yAxisWidth,0]);


    // 定义坐标轴
    var xAxis = d3.svg.axis()
        .scale(xScale)
        //.tickFormat(function(d) { return (d<0) ? -1*d+"%" : d+"%"; })
        .orient("bottom")
        //.ticks(12)
        .tickSize(-yAxisWidth)
        .tickFormat(function(d) { return (d<0.5) ? ((0.5-d)*24).toFixed(1)+"M" : ((d-0.5)*24).toFixed(1)+"M"; });
       // .tickPadding(height);
        //.tickFormat(function(d) { return (d<0.5) ? Math.round((0.5-d)*24)+"M" : Math.round((d-0.5)*24)+"M"; });


    var yAxis = d3.svg.axis()
        .scale(yScale)
        .orient("left");
    var yAxis2 = d3.svg.axis()
        .scale(yScale)
        .orient("right");


    // 添加坐标轴
    svg.append("g")
        .attr("class","axis")
        .attr("transform","translate(" + padding.left + "," + (height - padding.bottom) + ")")
        .call(xAxis);

    //alert("out");
    var steelblue = d3.rgb("steelblue");

        headerColumn = "age";
        leftColumn = "people";
        rightColumn = "people2";
        sextype = "sex";
        yeartype = "year";
    d3.csv("/data/census2000.csv", function(error, data) {
        if (error){
            console.log(error);
            alert("eee");

        }
        console.log(data);
        data.forEach(function(d) {
        //for(var i=0; i<data.length; i++) {
            if(d[yeartype]==="1990"){
                d[leftColumn] = d[leftColumn];
                d[rightColumn] = d[rightColumn];
               // alert("aaa");
            }

            //alert("aaa");
       // }
        });
        //alert("Are you ready?");
        xScale.domain(
            // Include other columnß
            [-d3.max(data, function(d) { return d[leftColumn]; })*1.2,
                d3.max(data, function(d) { return d[leftColumn]; })*1.2]
        ).nice();
        yScale.domain(data.map(function(d) { return d[headerColumn]; }));

        var bar = svg.selectAll(".bar").data(data).enter();



        var m = d3.rgb(0,150,240);	//浅蓝色
        var w = d3.rgb(240,120,120);	//浅粉色
        var om = d3.rgb(0,90,190);	//蓝色
        var ow = d3.rgb(160,70,70);	//粉色
        var b = d3.rgb(200,200,200);

        // Right bar
        bar.append("rect")
            .attr({
                "class": "bar right",
                "x": padding.left+xScale(0),
                "y": function(d,i) { if((i%2)===0){
                    //alert(i);
                    return height-yAxisWidth-padding.bottom +yScale(d[headerColumn]);
                }else if((i%2)===1){
                    //alert(i);
                    return height-yAxisWidth-padding.bottom +yScale(d[headerColumn])+15;
                }},
                "width": function(d) {
                    return Math.abs(xScale(d[rightColumn]) - xScale(0)); },
                "height": yScale.rangeBand()*0.6
            })
            .style("fill",function(d,i){
                if((i%2)===0){
                    return ow;
                }else if((i%2)===1){
                    return w;
                }
            });
        // Left bar
        bar.append("rect")
            .attr({
                "class": "bar left",
                "x": function(d) { return padding.left+xScale(-d[leftColumn]); },
                "y": function(d,i) { if((i%2)===0){
                    //alert(i);
                    return height-yAxisWidth-padding.bottom +yScale(d[headerColumn]);
                }else if((i%2)===1){
                    //alert(i);
                    return height-yAxisWidth-padding.bottom +yScale(d[headerColumn])+15;
                }},
                "width": function(d) {
                    return Math.abs(xScale(d[leftColumn]) - xScale(0)); },
                "height": yScale.rangeBand()*0.6
            })
            .style("fill",function(d,i){
                if((i%2)===0){
                    return om;
                }else if((i%2)===1){
                    return m;
                }
            });


        svg.append("g")
            .attr("class","axis")
            .attr("transform","translate(" + padding.left + "," + (height - padding.bottom - yAxisWidth) + ")")
            .call(yAxis);

        svg.append("g")
            .attr("class","axis")
            .attr("transform","translate(" + (xAxisWidth+padding.left) + "," + (height - padding.bottom - yAxisWidth) + ")")
            .call(yAxis2);


        bar.append("rect")
            .attr("x", padding.left+ 100)
            .attr("y", height-40)
            .attr("width", 20)
            .attr("height", 20)
            .style("fill", m);

        bar.append("text")
        //.attr("x", xAxisWidth)
        //.attr("y", height-yAxisWidth-padding.bottom-50)
        // .classed("legendtext", true)
            .attr("transform","translate(" + (padding.left + 130) + "," + (height -25) + ")")
            .attr("font-size","14px")
            .text("Man(2000)");

        bar.append("rect")
            .attr("x", padding.left+600)
            .attr("y", height-40)
            .attr("width", 20)
            .attr("height", 20)
            .style("fill", w);

        bar.append("text")
            .attr("transform","translate(" + (padding.left+630) + "," + (height - 25) + ")")
            .attr("font-size","14px")
            .text("Woman(2000)");

        bar.append("rect")
            .attr("x", padding.left+ 300)
            .attr("y", height-40)
            .attr("width", 20)
            .attr("height", 20)
            .style("fill", om);

        bar.append("text")
        //.attr("x", xAxisWidth)
        //.attr("y", height-yAxisWidth-padding.bottom-50)
        // .classed("legendtext", true)
            .attr("transform","translate(" + (padding.left+330) + "," + (height - 25) + ")")
            .attr("font-size","14px")
            .text("Man(1900)");

        bar.append("rect")
            .attr("x", padding.left+800)
            .attr("y", height-40)
            .attr("width", 20)
            .attr("height", 20)
            .style("fill", ow);

        bar.append("text")
            .attr("transform","translate(" + (padding.left+830) + "," + (height - 25) + ")")
            .attr("font-size","14px")
            .text("Woman(1900)");


        bar.append("text")
            .attr("transform","translate(" + (xAxisWidth+70) + "," + (height- padding.bottom+10) + ")")
            .attr("font-size","12px")
            .text("Population");
        bar.append("text")//age left
            .attr("transform","translate(" + (padding.left-20) + "," + (height - padding.bottom - yAxisWidth-10) + ")")
            .attr("font-size","12px")
            .text("Age");
        bar.append("text")//age right
            .attr("transform","translate(" + (xAxisWidth+50) + "," + (height - padding.bottom - yAxisWidth-10) + ")")
            .attr("font-size","12px")
            .text("Age");

        bar.append("text")//title
            .attr("transform","translate(" + (padding.left+280) + "," + (height - padding.bottom - yAxisWidth-40) + ")")
            .attr("font-size","20px")
            .text("The Change of U.S. Population Pyramid (1900-2000)");

    });

</script>











