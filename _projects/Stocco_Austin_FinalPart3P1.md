---
name: Marcos and Autsin's Final Part 3.1
tools: [Python, HTML, vega-lite]
image: assets/pngs/cars.png
description: For IS 445
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
  - justcharts
---

# The GPA Timeline of UIUC Classes (2010-2022)

Group 13

Stocco and Paull

## Average GPA of Each Course by College in UIUC by Year and Term

<vegachart schema-url="{{ site.baseurl }}\assets\json\UIUCgpaTimeLine.json" style="width: 100%"></vegachart>

### How to use the visualization
Our initial visualization is an interactive line chart depicting the average GPA per subject, year, and term. The chart zooms in and out when you scroll and highlights a line when you hover over it, turning the others grey. Additionally, hovering over a point shows the average GPA, subject, year-term, and max withdrawals. The chart starts by displaying all subjects, but users can choose a specific college in UIUC via the drop-down menu. For instance, selecting IS from the menu narrows the chart to show only INFO and IS subjects.

### About the chart
We're using the [dataset](https://github.com/wadefagen/datasets/tree/master/gpa) from Prof. Wade Fagen-Ulmschneider at UIUC which after some data formatting allows us to find the average GPA data for most courses.  We chose the plasma color scheme because it includes a gradient from yellow to purple covering the colors orange and blue in between which are the school colors for UIUC. This visualization allows the user to investigate many things. The user can see how the GPA of a course has changed overtime from 2010 to 2021. Users can use this to compare themselves to the average GPA of previous students. This can also help students get a gauge on how hard their respective major is or even help them decide what major to choose. One caveat about this chart is how they Year and Terms are ordered. For the most part, they are ordered correctly. However, winter terms are automatically placed after summer when they should be ordered after the fall terms. 

<div class="center">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/assets/json/UIUCgpaTimeLine.json" text="Altair Json" %}
</div>


## Average GPA of Each Course by Chunks in UWN by Year and Term

<vegachart schema-url="{{ site.baseurl }}\assets\json\UMNgpaTimeLine.json" style="width: 100%"></vegachart>

## How to use the visualization 
Similar to the first visualization the second one is a line chart. The chart has the same interactive features as the first chart but displays different data. The chart zooms in and out when you scroll and highlights a line when you hover over it, turning the others grey. Additionally, hovering over a point shows the average GPA, subject, and year-term. The chart starts by displaying all subjects, but users can choose a chunk containing 29 subjects with no specific grouping via the drop-down menu.

### About the chart
For this visualization we are using a [dataset](https://github.com/DannyG72/UMN-Grade-Dataset) from the University of Minnesota, the dataset includes UMN’s grade distribution data. We had to do some data formatting for this dataset as well in order to get average GPA and the year and term set correctly. Similar conclusions from the first visualization can be drawn from this chart.

<div class="center">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/assets/json/UMNgpaTimeLine.json" text="Altair Json" %}
</div>


## GPA distribution for 12 classes in NED University.

<img src="{{ site.baseurl }}\assets\pngs\NEDCGPA2.png" style="width: 100%">

Our third data visualization is from Ahmad Jalal Masood and sourced on [Kaggle](https://www.kaggle.com/code/ahmadjalalmasood123/eda-on-grades-of-students). Ahmad Jalal Masood is a Data Science Consultant at Freelancer Islamabad, Islamabad Capital Territory, Pakistan. The data used in the dataset comes from Muhammad Shayan. Muhammad Shayan is a student at NED University Karachi, Sindh, Pakistan. This visualization shows a series of box plots that show a list of 12 courses and the distribution of their GPA. It also includes outliers for the cumulative GPA of the students. This visualization shows a different way of looking at courses average GPA. The use of the box plot gives more information for a given course in comparison to the line chart. The box plot not only displays the average GPA but also shows the distribution of all of the students who have taken the course. This allows for more accurate predictions and insights about what GPA majority of students recieve.  

<div class="left">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/assets/pngs/NEDCGPA2.png" text="Image" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/python_notebooks/Stocco_Paull_Part3.ipynb" text="Jupyter Notebook" %}
</div>