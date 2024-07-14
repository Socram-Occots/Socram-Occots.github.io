---
name: CS 416 Narrative Visualization by Marcos Stocco
tools: [JavaScript, HTML, D3]
image: assets/pngs/cars.png
description: For CS 416
---

# test

<html>
   <script src='https://d3js.org/d3.v5.min.js'></script>
   <body onload='init()'>
   <svg width="20000" height="10000"></svg>
   <script>
    async function init() {
        var data = await d3.csv("https://raw.githubusercontent.com/Socram-Occots/Socram-Occots.github.io/main/python_notebooks/CS416/worlddata_massaged.csv");
        var specific_data = data.filter(function(d) {if ((d["Country Name"] == "Africa Eastern and Southern") && (d["Indicator Name"] == "Agricultural land (sq. km)")) {
            return d;
        }});
        var years_yvalue = specific_data.map(function(d){
            return {
                year: parseInt(d.variable, 10),
                value: parseFloat(d.value)
            };
        });
        years = years_yvalue.map(function(d){
            return {
                year: d.year
            };
        });
        yvaluesjson = years_yvalue.map(function(d){
            return {
                value: d.value
            };
        });
        console.log(years_yvalue);
        console.log(years);
        var xmax = d3.max(years);
        var yvaluesarray = yvaluesjson.map(function(d) {
            return d.value !== null ? d.value : 0;
        });
        console.log(yvaluesarray);
        var ymax = d3.max(yvaluesarray);
        console.log(ymax);
        var margin = 50,
            width = 500,
            height = 500;
        var x = d3.scaleLinear()
            .domain([1960,2023])
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
            .datum(years_yvalue)
            .attr("fill", "none")
            .attr("stroke", "steelblue")
            .attr("stroke-width", 1.5)
            .attr("d", d3.line()
                .x(function(d) {
                    return x(d.year)
                })
                .y(function(d) {
                    return y(d.value)
                })
                .defined(function(d) { return d.value; })
            )
        var xAxis = d3.axisBottom(x)
            .tickFormat(d3.format('d'))
            .ticks(63);
        var yAxis = d3.axisLeft(y)
            .tickFormat(d3.format("~s"))
            .ticks(20);
        svg.append("g")
            .attr("transform", "translate(0," + height + ")")
            .call(xAxis)
            .selectAll("text")  
            .style("text-anchor", "end")
            .attr("dx", "-.8em")
            .attr("dy", ".15em")
            .attr("transform", "rotate(-25)");
        svg.append("g")
            .call(yAxis);
    }
   </script>
   </body>
</html>