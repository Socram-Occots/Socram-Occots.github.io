---
name: Marcos and Autsin's HW 10
tools: [Python, HTML, vega-lite]
image: assets/pngs/cars.png
description: For IS 445
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
  - justcharts
---

## Big Foot sightings in the USA (County)

<vegachart schema-url="{{ site.baseurl }}/assets/json/bcubcgHW1Plot1.json" style="width: 100%"></vegachart>

This plot features the Big Foot sightings in the USA from this [dataset](https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_bcubcg_fall2022/main/data/bfro_reports_fall2022.csv). Each brown dot represents a report along with the county it is from. The USA map in the background is taken from the vega lite example charts and used as a background for the data. The qualitative data itself is encoded in latitude and longitude and the map is used with topojson and projected in albersUsa. There was great difficulty in adapting the data set to fit map. Aside from the many missing values the provided dataset had, some of the latitude and/or longitude were not missing but incorrect. This caused them to appear in the top left corner. Thus, two sets of conditionals were made to filter out any data that did not reside in the latitudinal and longitudinal coordinates of the US. That data transformation was not done in the Python notebook and was done in the vega lite editor instead. This was done because the vega chart data was procesed directly using a url instead of creating a data frame. Thus, changing the data as a dataframe would not affect the oridinal chart. Currenly, the data is very messy and one can hardly see anything. Adding a color map to this will not help as the exessive amount of text is the problem. Instead of changing the text, the interactivity is supposted to provide a way to clear up the plot and make it readable. The alternative, removing text and adding a color map, would have provided depth to each data point. With a color map, another fact such as temperature or moon phase could have been communicated. There are no connections between the work done in homework 9 and this.
<div class="left">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/assets/json/bcubcgHW1Plot1.json" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/python_notebooks/Stocco_Paull_HW10.ipynb" text="The Analysis" %}
</div>

