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

<vegachart schema-url="{{ site.baseurl }}/assets/json/bcubcgHW1Plot1inter.json" style="width: 100%"></vegachart>

This plot features the Big Foot sightings in the USA from this [dataset](https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_bcubcg_fall2022/main/data/bfro_reports_fall2022.csv). Each red dot represents a report along with the county it is from. The USA map in the background is taken from the Vega Lite example charts and used as a background for the data. The qualitative data itself is encoded in latitude and longitude and the map is used with topojson and projected in albersUsa. There was great difficulty in adapting the data set to fit the map. Aside from the many missing values the provided dataset had, some of the latitude and/or longitude were not missing but incorrect. This caused them to appear in the top left corner. Thus, two sets of conditionals were made to filter out any data that did not reside in the latitudinal and longitudinal coordinates of the US. That data transformation was not done in the Python notebook and was done in the vega lite editor instead. This was done because the vega chart data was procesed directly using a url instead of creating a data frame. Thus, changing the data as a dataframe would not affect the oridinal chart. With a color map, another fact such as temperature or moon phase could have been communicated. However, the congestion between the points is still apparent. Thus, the chart was made interactive to relieve this. There are no connections between the work done in homework 9 and this.

### Interactivity

The first plot's interactivity comes from the ability to select certain points and increase its opacity. This allows the audience to focus on one point at a time rather than be overwhelmed by all of them at once. This chart's interactivty would have been futher developed and provided information such as the county. However, numerours and compatabilty issues between Altair and Vega-Lite proved this task too much for the time constraint. Vega-Lite itself provides interactivity. For example, this is the first iteration of the interactive plot:

<vegachart schema-url="{{ site.baseurl }}/assets/json/bcubcgHW1Plot1vegalite.json" style="width: 100%"></vegachart>

This plot's json file was manually made with vega cod and without the use of Altair. The layered text and overall smoother interactivness is currently not supported on Altair.

<div class="left">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/assets/json/bcubcgHW1Plot1inter.json" text="Altair Json" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/assets/json/bcubcgHW1Plot1vegalite.json" text="Vega-Lite Json" %}
</div>


## Bigfoot Sightings per State

<vegachart schema-url="{{ site.baseurl }}/assets/json/bcubcgHW1Plot2.json" style="width: 100%"></vegachart>

In this visualization we are showing a bar plot of the number of reported bigfoot sighting per state. On the x-axis we have the states where sightings were reported and on the y-axis we have the number of sightings. I chose a to use a color scale for the bars to add a visual effect the lighter is for lower numbers and darker for higher numbers. I chose the color brown because I associate bigfoot with the color brown because of his theorized brown hairy body. While it is a pretty basic plot it allows for the viewer to quickly identify the differences in sightings based on state. No data transformation was done. The url was used directly within Altair. There are no connections between the work done in homework 9 and this.

<div class="left">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/assets/json/bcubcgHW1Plot2.json" text="Altair Json" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/Socram-Occots/Socram-Occots.github.io/blob/main/python_notebooks/Stocco_Paull_HW10.ipynb" text="Jupyter Notebook" %}
</div>