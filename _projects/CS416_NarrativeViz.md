---
name: CS 416 Narrative Visualization by Marcos Stocco
tools: [JavaScript, HTML, D3]
image: assets/pngs/cars.png
description: For CS 416
---

# test

<html>
   <script src='https://d3js.org/d3.v5.min.js'></script>
   <style> circle { fill: lightblue; stroke: black; } </style>
   <body onload='init()'>
   
   <svg width="1000" height="1000"></svg>
   <script>
    async function init() {
        var data = await d3.csv("https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/python_notebooks/CS416/WDICSV.csv");
        var specific_data = data.filter(function(d) {if ((d["Country"] == "Africa Eastern and Southern") && (d["Indicator Name"] == "Agricultural land (sq. km)")) {
            return d;
        }})
        var xmax = d3.max(specific_data["variable"])
        var ymax = d3.max(specific_data["value"])
        var margin = 50,
            width = 500,
            height = 500;
        var x = d3.scaleLinear()
            .domain([1950, 2022])
            .range([0, width]);
        var y = d3.scaleLinear()
            .domain([0, ymax])
            .range([height, 0]);
        var svg = d3.select("svg")
            .attr("width", width + 2 * margin)
            .attr("height", height + 2 * margin)
            .append("g")
            .attr("transform", "translate(" + margin + "," + margin + ")");
        svg.append("g")
            .datum(specific_data)
            .attr("fill", "none")
            .attr("stroke", "steelblue")
            .attr("stroke-width", 1.5)
            .attr("d", d3.line()
                .x(function(d) {
                    return x(d.variable)
                })
                .y(function(d) {
                    return y(d.value)
                })
            )
        var xAxis = d3.axisBottom(x)
            .tickFormat(d3.format("~s"));
        var yAxis = d3.axisLeft(y)
            .tickFormat(d3.format("~s"));
        svg.append("g")
            .attr("transform", "translate(0," + height + ")")
            .call(xAxis);
        svg.append("g")
            .call(yAxis);
    }
   </script>
   </body>
</html>