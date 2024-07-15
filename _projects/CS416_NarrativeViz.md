---
name: CS 416 Narrative Visualization by Marcos Stocco
tools: [JavaScript, HTML, D3]
image: assets/pngs/cars.png
description: For CS 416
---

# test

<html>
    <select id="selectNames" size="5" multiple></select>
    <script>
    var select = document.getElementById("selectNames");
    var options = ["1", "2", "3", "4", "5","6","7","8","9"];
    for(var i = 0; i < options.length; i++) {
        var opt = options[i];
        var el = document.createElement("option");
        console.log(el);
        el.textContent = opt;
        el.value = opt;
        select.appendChild(el);
    }
    </script>
</html>

<html>
   <script src='https://d3js.org/d3.v5.min.js'></script>
   <body onload='init()'>
   <svg width="100%" height="100%"></svg>
   <script>
   const includesAny = (arr, values) => values.some(v => arr.includes(v));
    async function init() {
        var data = await d3.csv("https://raw.githubusercontent.com/Socram-Occots/Socram-Occots.github.io/main/python_notebooks/CS416/worlddata_massaged.csv");
    }
    function selectField() {
        var specific_data = data.filter(function(d) {if ((includesAny(d["Country Name"],["Africa Eastern and Southern","Caribbean small states"])) && (d["Indicator Name"] == "Agricultural land (sq. km)")) {
            return d;
        }});
        console.log(specific_data);
        var years_yvalue = specific_data.map(function(d){
            return {
                name: d["Country Name"],
                year: parseInt(d.variable, 10),
                value: d.value*1.0
            };
        });
        years = years_yvalue.map(function(d){
            return {
                year: d.year
            };
        });
        yvaluesarray = years_yvalue.map(function(d){
            return d.value;
        });
        var grouped = d3.nest()
            .key(function(d) { return d.name; })
            .entries(years_yvalue);
        console.log(years_yvalue);
        console.log(grouped);
        // console.log(years);
        var xmax = d3.max(years);
        // console.log(yvaluesarray);
        var ymax = d3.max(yvaluesarray);
        // console.log(ymax);
        var margin = 75,
            width = 800,
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
        // var line = d3.line()
        //     .x(function(d) { return x(d.year); })
        //     .y(function(d) { return y(d.value); })
        //     .defined(function(d) { return d.value != 0; });
        // svg.append("path")
        //     .datum(years_yvalue)
        //     .attr("fill", "none")
        //     .attr("stroke", "steelblue")
        //     .attr("stroke-width", 1.5)
        //     .attr("d", line);
        var res = grouped.map(function(d){ return d.key });
        var color = d3.scaleOrdinal()
            .domain(res)
            .range(['blue','green']);
        svg.selectAll(".line")
            .data(grouped)
            .enter()
            .append("path")
                .attr("fill", "none")
                .attr("stroke", function(d){ return color(d.key) })
                .attr("stroke-width", 1.5)
                .attr("d", function(d){
                return d3.line()
                    .x(function(d) { return x(d.year); })
                    .y(function(d) { return y(d.value); })
                    .defined(function(d) { return d.value != 0; })
                    (d.values)     
                })
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
            .attr("transform", "rotate(-45)");
        svg.append("g")
            .call(yAxis);
        svg.append("text")
            .attr("class", "x label")
            .attr("text-anchor", "end")
            .attr("x", width/2)
            .attr("y", height + 50)
            .text("years (1960 - 2023)")
            .style('fill', 'white');
        svg.append("text")
            .attr("class", "y label")
            .attr("text-anchor", "end")
            .attr("x",-height/2)
            .attr("y", -55)
            .attr("dy", ".75em")
            .attr("transform", "rotate(-90)")
            .text("test")
            .style('fill', 'white');
    }
   </script>
   </body>
</html>