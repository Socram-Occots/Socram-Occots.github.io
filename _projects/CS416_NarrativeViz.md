---
name: CS 416 Narrative Visualization by Marcos Stocco
tools: [JavaScript, HTML, D3]
image: assets/pngs/cars.png
description: For CS 416
---

<h1 id="title">test</h1>

<script>
    var places = ["Africa Eastern and Southern", "Africa Western and Central", "Middle East & North Africa", "South Africa", "Arab World", "Caribbean small states", "Central Europe and the Baltics", "East Asia & Pacific", "Europe & Central Asia", "Latin America & Caribbean", "North America", "South Asia"]
    var incomes = ["High income", "Upper middle income", "Middle income", "Lower middle income", "Low income"];
    var final_scene = ["World"];
    var input_array = [];
    const input_dictionary={ 
        0:places, 
        1:places, 
        2:places,
        3:places,
        4:incomes,
        5:final_scene
    };
    const field_dictionary={ 
     0:"Urban population (% of total population)", 
     1:"Land area (sq. km)",
     2:"Population, total",
     3:"Population in the largest city (% of urban population)",
     4:"Urban population (% of total population)",
     5:"Urban population (% of total population)"
    };
    const filter_options = ["option 1", "option 2", "option 3", ];
    const stage_options = ["stage 1", "stage 2", "stage 3"];
    var current_stage = 0;
    var drawing = false
    function stageNext() {
        if (drawing) return;
        current_stage = (current_stage + 1)%6
        console.log(current_stage);
        promtDrawCycle()
    };
    function stageBack() {
        if (drawing) return;
        if (current_stage - 1 < 0) { current_stage = 0 }
        else { current_stage = (current_stage - 1)%6 };
        console.log(current_stage);
        promtDrawCycle()
    };
    function promtDrawCycle() {
        dePopulateOptions();
        populateOptions();
        parseData();
    }
</script>

<body onload='init()'>
    <script src='https://d3js.org/d3.v5.min.js'> </script>
    <script>
        async function init() {
            drawing = true
            d3_csv_data = await d3.csv("https://raw.githubusercontent.com/Socram-Occots/Socram-Occots.github.io/main/python_notebooks/CS416/worlddata_massaged.csv");
            promtDrawCycle();
        };
    </script>
</body>

<body>
    <style>
        .container {
            max-width: fit-content;
            margin-left: auto;
            margin-right: auto;
        }
    </style>
    <div class="container">
        <button onclick='stageBack()'> <- Back </button> 
        <button onclick='stageNext()'> Next -> </button> 
        <select id="selectNames" size="5" multiple>
        <option disabled selected value> -- select an option(s) -- </option>
        </select>
    </div>
    <div class="container">
        <svg id="dataviz"></svg>
        <svg id="legend"></svg>
    </div>
    <script>
    function populateOptions() {
        var selectPOP = document.getElementById("selectNames");
        for(var i = 0; i < input_dictionary[current_stage].length; i++) {
            var opt = input_dictionary[current_stage][i];
            var el = document.createElement("option");
            el.textContent = opt;
            el.value = opt;
            selectPOP.appendChild(el);
        }
    }
    function dePopulateOptions() {
        var select = document.getElementById("selectNames");
        for (var i = select.length - 1; i >= 1; i--) {
            select.removeChild(select.children[i]);
        }
    }
    </script>
</body>

<body>
    <script src='https://d3js.org/d3.v5.min.js'> </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/d3-annotation/2.5.1/d3-annotation.min.js" integrity="sha512-iBAeBWWWFb8HqSBcrqcz98iIpuVH1la39dEYHtyQ/pGpeCQTQVvLJOWAuhv2Q7JSHp9k7hWA7sGxU3hHJe+tFg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <script>
    function parseData() {
        var specific_data = d3_csv_data.filter(function(d) {if ((input_dictionary[current_stage].includes(d["Country Name"])) && (d["Indicator Name"] == field_dictionary[current_stage])) {
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
        d3.select("#dataviz").selectAll("*").remove();
        d3.select("#legend").selectAll("*").remove();
        drawGraph(xmax, ymax, grouped);
        };
        function drawGraph(xmax, ymax, grouped) {
            var margin = 100,
                width = 1000,
                height = 500;
            var x = d3.scaleLinear()
                .domain([1960,2023])
                .range([0, width]);
            var y = d3.scaleLinear();
            if ([1,2].includes(current_stage)) {y.domain([0, ymax]).range([height, 0]);}
            else {y.domain([0, 100]).range([height, 0]);}
            var svg = d3.select("#dataviz")
                .attr("width", width + 2 * margin)
                .attr("height", height + 2 * margin)
                .append("g")
                .attr("transform", "translate(" + margin + "," + margin + ")");
            var res = grouped.reduce((acc, currElement, index) => {
                acc[currElement.key] = index;
                return acc;
            }, {});
            console.log(res);
            var object_keys = Object.keys(res);
            console.log(object_keys.length);
            var color = d3.scaleSequential(d3.interpolateSpectral)
                .domain([0, object_keys.length - 1]);
            svg.selectAll("g")
                .data(grouped)
                .enter()
                .append("path")
                    .attr("fill", "none")
                    .attr("stroke", function(d){ return color(res[d.key]); })
                    .attr("stroke-width", 1.5)
                    .attr("d", function(d){
                    return d3.line()
                        .x(function(d) { return x(d.year); })
                        .y(function(d) { return y(d.value); })
                        .defined(function(d) { return d.value != 0; })
                        (d.values)     
                    });
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
                .attr("transform", "rotate(-35)");
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
                .attr("x",-height/4)
                .attr("y", -55)
                .attr("dy", ".75em")
                .attr("transform", "rotate(-90)")
                .text(field_dictionary[current_stage])
                .style('fill', 'white');
            // plotting legend
            const legend_start_pos_y = 0;
            const legend_start_pos_x = 0;
            svg_legend = d3.select("#legend")
                .attr("width", 300 + 2 * 300)
                .attr("height", 1000 + 2 * 1000)
                .append("g")
                .attr("transform", "translate(" + 20 + "," + 20 + ")");
            svg_legend.selectAll("mydots")
                .data(object_keys)
                .enter()
                .append("circle")
                    .attr("cx", legend_start_pos_x)
                    .attr("cy", function(d,i){ return legend_start_pos_y + i*25})
                    .attr("r", 7)
                    .style("fill", function(d){ return color(res[d])});
            svg_legend.selectAll("mylabels")
                .data(object_keys)
                .enter()
                .append("text")
                .attr("x", legend_start_pos_x + 20)
                .attr("y", function(d,i){ return legend_start_pos_y + i*25})
                .style("fill", function(d){ return color( res[d]) })
                .text(function(d){ return d })
                .attr("text-anchor", "left")
                .style("alignment-baseline", "middle");
            // Add annotation to the chart
            var annotations = [{note: {
                    label: "Here is the annotation label",
                    title: "Annotation title"},
                    x: 100,y: 100,dy: 100,dx: 100}];
            var makeAnnotations = d3.annotation()
                .editMode(true)
                .annotations(annotations);
            d3.select("svg")
                .append("g")
                .call(makeAnnotations)
                    drawing = false;
                };
    </script>
</body>