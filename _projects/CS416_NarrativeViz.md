---
name: CS 416 Narrative Visualization by Marcos Stocco
tools: [JavaScript, HTML, D3]
image: assets/pngs/cars.png
description: For CS 416
---

<script>
    const annotation_details = {
        1:["2007: The % of the urban and rural world population equalized (1)","Urban population (% of total population)",2007],
        2:["1961: The Green Revolution was extended to Asia and Africa. (2)", "Urban population (% of total population)",1961],
        3:["1974: The first World Population Conference was held. (3)","Urban population (% of total population)",1974],
        4:["1995: The commercial Internet Service Provider market took off. (4)","Urban population (% of total population)",1995],
        5:["1975: Decollectivized agriculture and privatized farms. (5)","China",1975],
        6:["1978: The Open Door Policy permitted foreign businesses to set. (6)","China",1978],
        7:["1984: Shifted many of their economic systems which led to decentralizing state control. (7)","China",1984],
        8:["2001: Joined the World Trade Organization. (7)","China",2001],
        9:["1991: Was economically liberated due to massive policy changes. (8)","India","1991"],
        10:["1990s: Experienced a massive tech industry boom. (9)","India",1990],
        11:["1980s: Made numerous infrastructure campaigns and policies. (10)","India",1980],
        12:["1960: Gained independence. (11)","Nigeria",1960],
        13:["1970s: Had expansion in banking, construction, and tourism sectors. (11)","Nigeria",1970],
        14:["1976: Created new states. (11)","Nigeria",1976],
        15:["1950-1960s: Major cities had extensive urbanization projects. (12)","Brazil",1960],
        16:["1994: Pushed to upgrade the infrastructure and living conditions. (13)","Brazil",1994],
        17:["1964: Passed the Civil Rights Act. (14)", "United States",1964],
        18:["1986: The Immigration Reform and Control Act legalizes illegals living illegally since 1982. (14)","United States",1986],
        19:["1968: Passed the Fair Housing Act (15)","United States",1968],
        20:["2008: Heavily invests in recovering from the Housing Crisis. (15)","United States",2008],
        21:["1956: The Interstate Highway System began construction. (16)","United States",1960],
        22:["1960s: Mainframe computers became widespread. (17)","United States",1960],
        23:["The wealthy live in urban environments much more than the rest.", "High income", 1975],
        24:["Now, the rest of world is quickly catching up.", "Low income", 2008],
        25:["Use the multi-value menu to explore the dataset!", "World",1991]
    };
    var countries1 = ["China","India","Brazil"];
    var countries2 = ["Nigeria", "United States"];
    var incomes = ["High income", "Upper middle income", "Middle income", "Lower middle income", "Low income"];
    var urban_vs_rural = ["World"];
    var final_scene = ["World"];
    var urbanvsrural_field = ["Urban population (% of total population)", "Rural population (% of total population)"];
    var standard_field = ["Urban population (% of total population)"];
    const input_dictionary={ 
        0:urban_vs_rural, 
        1:countries1, 
        2:countries2,
        3:incomes,
        4:final_scene,
        5:null
    };
    const field_dictionary={ 
        0:urbanvsrural_field,
        1:standard_field,
        2:standard_field,
        3:standard_field,
        4:standard_field,
        5:null
     };
    var current_stage = 0;
    var memory_stage = 0;
    var drawing = false
    function stageNext() {
        if (drawing) return;
        drawing = true;
        if (current_stage == 5) current_stage = 0;
        else current_stage = (current_stage + 1)%5;
        // console.log(current_stage);
        if (current_stage == 4) 
        {   document.getElementById("selectNames").style.display = "inline";
            document.getElementById("submit").style.display = "inline";}
        else 
        {   document.getElementById("selectNames").style.display = "none";
            document.getElementById("submit").style.display = "none";}
        promtDrawCycle()
    };
    function stageBack() {
        if (drawing) return;
        drawing = true;
        if (current_stage - 1 < 0) current_stage = 0;
        else current_stage = (current_stage - 1)%5 ;
        // console.log(current_stage);
        if (current_stage == 4) 
        {   document.getElementById("selectNames").style.display = "inline";
            document.getElementById("submit").style.display = "inline";}
        else 
        {   document.getElementById("selectNames").style.display = "none";
            document.getElementById("submit").style.display = "none";}
        promtDrawCycle()
    };
    function explore() {
        if (drawing) return;
        var multi_selecton = document.getElementById("selectNames");
        var user_selection = Array.from(multi_selecton.querySelectorAll("option:checked"),e=>e.value);
        if (user_selection.length == 0 || user_selection[0] == "") return;
        drawing = true;
        input_dictionary[5] = user_selection;
        if (current_stage != 5) {
            memory_stage = current_stage;
            field_dictionary[5] = field_dictionary[current_stage];
            current_stage = 5;
        }
        promtDrawCycle();
    };
    function promtDrawCycle() {
        dePopulateOptions();
        populateOptions();
        parseData();
    }
    const margin = 200, width = 1500, height = 1000, margin_tweak = 0.3;
    var unique_countries_array = [];
</script>

<body>
<script src='https://d3js.org/d3.v5.min.js'> </script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3-annotation/2.5.1/d3-annotation.min.js" integrity="sha512-iBAeBWWWFb8HqSBcrqcz98iIpuVH1la39dEYHtyQ/pGpeCQTQVvLJOWAuhv2Q7JSHp9k7hWA7sGxU3hHJe+tFg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>

<body onload='init()'>
    <script>
        async function init() {
            drawing = true
            d3_csv_data = await d3.csv("https://raw.githubusercontent.com/Socram-Occots/Socram-Occots.github.io/main/python_notebooks/CS416/worlddata_massaged.csv");
            for (var i in d3_csv_data) { 
                unique_countries_array.push(d3_csv_data[i]["Country Name"]); 
            };
            unique_countries_array = [... new Set(unique_countries_array)];
            promtDrawCycle();
        };
    </script>
</body>

<script>
    function makingAnnotations(anniearray, xaxis, yaxis, grouped) {
        var annotation_result = [];
        // disgusting code that I would make a function for but NO I want to finish this project
        // I shouldn't hurt performance in this senario anyways
        var count = 0;
        for (var i = 0; i < anniearray.length; i++) {
            var annie_i = anniearray[i];
            var annie_info = annotation_details[annie_i];
            var xpos = xaxis(annie_info[2]);
            var value_for_ypos = null;
            for (var a = 0; a < grouped.length; a++) {
                var inner_key = grouped[a];
                if (inner_key["key"] == annie_info[1]) {
                    var inner_values = inner_key["values"];
                    for (var b = 0; b < inner_values.length; b++) {
                        var inner_inner_values = inner_values[b];
                        if (inner_inner_values["year"] == annie_info[2]) {
                            value_for_ypos = inner_inner_values["value"];
                            break;
                        } 
                    } break;
                }
            }
            var ypos = yaxis(value_for_ypos);
            var dy_variation = -100;
            var dx_variation = -100;
            if (count%2 != 0) {dy_variation *= -1;}
            count++;
            // custon annotation edits... PLZ end my suffering :,(
            if (annie_info[2] < 1965) { dx_variation = 100 }
            if (annie_info[1] == "Brazil" && annie_info[2] == 1960) { dy_variation = -300;  dx_variation *= 3 }
            if (annie_info[1] == "India" && annie_info[2] == 1991) { dy_variation = -250;  dx_variation *= 0 }
            if (annie_info[1] == "China" && annie_info[2] == 1984) { dy_variation = -200;  dx_variation *= 0.7 }
            if (annie_info[1] == "China" && annie_info[2] == 2001) { dy_variation = 125; dx_variation *= -1 } 
            if (annie_info[1] == "United States" && annie_info[2] == 1986) { dy_variation = 100; dx_variation *= -1 }
            if (annie_info[1] == "United States" && annie_info[2] == 1960 && annie_i == 21) { dy_variation = -100; dx_variation *= 1 }
            if (annie_info[1] == "United States" && annie_info[2] == 1960 && annie_i == 22) { dy_variation = 35; dx_variation *= 1 }
            if (annie_info[1] == "United States" && annie_info[2] == 1968) { dy_variation = -125; dx_variation *= -2 }           
            annotation_result.push(
                {note: {
                    label: annie_info[0], title: annie_info[1], align: "middle", wrap: 550},
                    x: xpos + margin, y: ypos + margin*margin_tweak, dy: dy_variation, dx: dx_variation
                    }
            );
        }
        return annotation_result;
        };
    function deployAnnotations(x, y, grouped) {
        var tests_array = [0,1,2,3,4];
        var results_array = [[1,2,3,4],[5,6,7,8,9,10,11,15,16],[12,13,14,17,18,19,20,21,22],[23,24],[25]];
        var anniearray = null;
        for (var i = 0; i < tests_array.length; i++) {
            if (tests_array[i] == current_stage) {
                anniearray = results_array[i];
                break;}}
        return makingAnnotations(anniearray, x, y, grouped)
        };
</script>


<body>
    <style>
        .container {
            max-width: fit-content;
            margin-left: auto;
            margin-right: auto;
        }
        #title{
            max-width: fit-content;
            margin-left: auto;
            margin-right: auto;
        }
        #intro{
            max-width: fit-content;
            margin-left: auto;
            margin-right: auto;
        }
        #help {
            max-width: fit-content;
            margin-left: auto;
            margin-right: auto;
            }
        #selectNames {display: none;}
        #submit {display: none;}
    </style>
        <h1 id="title">The Development of Urbanization in the World</h1>
        <h2 id="intro">A Narrative Visualization</h2>
        <p id="help"> Progress through with the back and next buttons. The annotations can be moved out of the way if wanted.<p>
    <div class="container">
        <button onclick='stageBack()'> <- Back </button> 
        <button onclick='stageNext()'> Next -> </button> 
        <select id="selectNames" size="5" multiple> <option disabled selected value> -- select an option(s) (select multiple: ctr/shift) -- </option> 
        </select>
        <button id="submit" onclick='explore()'> Submit </button>
    </div>
    <div class="container">
        <svg id="dataviz"></svg>
        <svg id="legend"></svg>
    </div>
    <script>
    function populateOptions() {
        var selectPOP = document.getElementById("selectNames");
        var option_array = null;
        var temp_index = current_stage
        if (current_stage == 5) { 
            option_array = unique_countries_array;
            temp_index = memory_stage;
        }
        else if (current_stage == 4) { option_array = unique_countries_array; } 
        else { option_array = input_dictionary[temp_index]; }
        for(var i = 0; i < option_array.length; i++) {
            var opt = option_array[i];
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
    <script>
    function field_based_on_stage() {if ([0].includes(current_stage)) {return input_dictionary[current_stage]
        } else {return field_dictionary[current_stage];}};
    function which_input(d) {if ([0].includes(current_stage)) {return d["Indicator Name"];} else {return d["Country Name"]}};
    function parseData() {
        var specific_data = d3_csv_data.filter(function(d) {if ((input_dictionary[current_stage].includes(d["Country Name"])) && field_dictionary[current_stage].includes(d["Indicator Name"])) {
            return d;
        }});
        // console.log(specific_data);
        var years_yvalue = specific_data.map(function(d){
            return {
                name: which_input(d),
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
        // console.log(years_yvalue);
        // console.log(grouped);
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
            var x = d3.scaleLinear()
                .domain([1960,2023])
                .range([0, width]);
            var y = d3.scaleLinear();
            y.domain([0, 100]).range([height, 0]);
            // if ([1,2].includes(current_stage)) {y.domain([0, ymax]).range([height, 0]);}
            // else {y.domain([0, 100]).range([height, 0]);}
            var svg = d3.select("#dataviz")
                .attr("width", width + 2 * margin)
                .attr("height", height + 0.8 * margin)
                .append("g")
                .attr("transform", "translate(" + margin + "," + margin*margin_tweak + ")");
            var res = grouped.reduce((acc, currElement, index) => {
                acc[currElement.key] = index;
                return acc;
            }, {});
            // console.log(res);
            var object_keys = Object.keys(res);
            var object_keys_length = Object.keys(res).length;
            var color = d3.scaleSequential(d3.interpolateSpectral)
                .domain([0, object_keys_length - 1]);
            svg.selectAll("g")
                .data(grouped)
                .enter()
                .append("path")
                    .attr("fill", "none")
                    .attr("stroke", function(d){ return color(res[d.key]); })
                    .attr("stroke-width", 2)
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
                .attr("x",-height/3)
                .attr("y", -55)
                .attr("dy", ".75em")
                .attr("transform", "rotate(-90)")
                .text(field_based_on_stage())
                .style('fill', 'white');
            // plotting legend
            const legend_start_pos_y = 0;
            const legend_start_pos_x = 0;
            svg_legend = d3.select("#legend")
                .attr("width", 600)
                .attr("height", object_keys_length*25 + 20)
                .append("g")
                .attr("transform", "translate(" + 200 + "," + 20 + ")");
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
            if (current_stage != 5) {
                var annotations = deployAnnotations(x, y, grouped);
                var makeAnnotations = d3.annotation()
                    .editMode(true)
                    .annotations(annotations);
                d3.select("svg")
                    .append("g")
                    .call(makeAnnotations)
            }
            drawing = false;
            };
    </script>
</body>

</body>
<body>
    <h3 id="citations">Works Cited</h3>
    <p>
    (1) Hannah Ritchie, Veronika Samborska and Max Roser https://ourworldindata.org/urbanization
    <br>
    (2) Philanthropy Roundtable https://www.philanthropyroundtable.org/almanac/averting-millions-of-deaths-by-starvation/
    <br>
    (3) United Nations https://www.un.org/en/conferences/population/bucharest1974
    <br>
    (4) Greenstein, Shane (January 2000). "Commercialization of the Internet: The Interaction of Public Policy and Private Choices or Why Introducing the Market Worked so Well". Innovation Policy and the Economy. 1: 151–186. doi:10.1086/ipe.1.25056144. hdl:10419/38643. ISSN 1531-3468
    <br>
    (5) Hunt, Michael H. (2004). The World Transformed: 1945 to the Present. Boston: Bedford/St. Martin's. p. 355 https://archive.org/details/worldtransformed0000hunt/page/n5/mode/1up
    <br>
    (6) "Open Door Policy". BBC. Archived from the original on 2017-07-25 http://news.bbc.co.uk/2/shared/spl/hi/asia_pac/02/china_party_congress/china_ruling_party/key_people_events/html/open_door_policy.stm
    <br>
    (7) Brandt, Loren; et al. (2008), Brandt, Loren; Rawski, G. Thomas (eds.), China's Great Economic Transformation, Cambridge: Cambridge university press
    <br>
    (8) Verma, A. N. (1991). Statement on Industrial Policy (India, Ministry of Industry). New Delhi: Government of India.
    <br>
    (9) Devesh Kapur, CENTER FOR THE ADVANCED STUDY OF INDIA https://casi.sas.upenn.edu/sites/default/files/bio/uploads/Causes_and_Consequences_of_IT_Boom.pdf
    <br>
    (10) "Infrastructure Development and Government Policy Since 1950" Encyclopedia of India. . Encyclopedia.com www.encyclopedia.com
    <br>
    (11) Aliyu AA, Amadu L. Urbanization, cities, and health: The challenges to Nigeria - A review. Ann Afr Med. 2017 Oct-Dec;16(4):149-158. doi: 10.4103/aam.aam_1_17. PMID: 29063897; PMCID: PMC5676403.
    <br>
    (12) GODFREY, BRIAN J. "REVISITING RIO DE JANEIRO AND SAO PAULO [*]." The Geographical Review, vol. 89, no. 1, Jan. 1999, p. 94. Gale Academic OneFile, link.gale.com/apps/doc/A59134195/AONE?u=anon~ed4aea11&sid=googleScholar&xid=88ce78e5.
    <br>
    (13) Elizabeth Riley, Jorge Fiori, Ronaldo Ramirez, Favela Bairro and a new generation of housing programmes for the urban poor, Geoforum, Volume 32, Issue 4, 2001, Pages 521-531, ISSN 0016-7185, https://doi.org/10.1016/S0016-7185(01)00016-1
    <br>
    (14) Library of Congress https://www.loc.gov/classroom-materials/immigration/global-timeline/
    <br>
    (15) The City University of New York https://www1.cuny.edu/sites/thehouse/milestones-for-the-house-i-live-in-a-history-of-housing-in-the-united-states/
    <br>
    (16) Elisheva Blas, "The Dwight D. Eisenhower National System of Interstate and Defense Highways: The Road to Success?". History Teacher 44.1 (2010): 127–142
    <br>
    (17) IBM https://www.ibm.com/docs/en/zos-basic-skills?topic=today-s360-turning-point-in-mainframe-history
    </P>
</body>
