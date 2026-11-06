---
name: Illinois State Buildings with Altair
tools: [Python, Altair, vega-lite]
image: assets/pngs/building_inventory.png
description: Two interactive views of the Illinois state building inventory, made in Python with Altair and exported to vega-lite.
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
  - justcharts
---

# Illinois State Buildings with Altair

## Plot 1: Buildings by decade constructed and usage

<vegachart schema-url="{{ site.baseurl }}/assets/json/buildings_by_decade_usage.json" style="width: 100%"></vegachart>

This stacked bar chart shows how many state buildings were constructed in each decade, broken down by usage type. Decade is an ordinal encoding on the x-axis, since the decades are ordered bins. Building count is quantitative on the y-axis, and usage is nominal and mapped to color. Usage types have no natural order, so I colored by usage with the categorical tableau10 scheme, which gives each type a distinct hue and fits our ten categories. In the notebook, I dropped rows where the construction year or square footage was 0 (unknown) and fixed two truncated usage labels. I then grouped the four rarest usage types into "Other," binned construction years into decades (pooling everything before 1900 into one "Pre-1900" bar), and counted buildings per decade and usage with a groupby.

## Plot 2: Floor space of the 15 largest agencies

<vegachart schema-url="{{ site.baseurl }}/assets/json/agency_sqft_slider.json" style="width: 100%"></vegachart>

This horizontal bar chart shows the total square footage held by the 15 agencies with the most floor space. Agency is nominal on the y-axis, sorted by bar length, and total square footage is quantitative on the x-axis. Color is a second quantitative variable: the average square footage per building. Because this is a magnitude, I used the sequential viridis scheme, which is perceptually uniform and colorblind-friendly. I put it on a log scale because the averages range from under 1,000 to about 70,000 sq ft. The color separates agencies with a few large buildings, like the universities, from agencies with many small ones, like Natural Resources. In the notebook, I used the same cleaned data as in Plot 1, kept the top 15 agencies by total square footage, and summed square footage and building counts per agency per construction year. In the chart spec, vega-lite filters those rows by the slider year, sums each agency, and calculates the average building size.

## Interactivity

In Plot 1, clicking a legend entry highlights that usage type and fades the others. This makes one category easy to follow across decades even when it sits in the middle of a stack. Tooltips give the exact counts. In Plot 2, a year slider limits the chart to buildings built in or before the selected year. Dragging the slider shows when each agency's footprint was built, which a single static total can't show. For example, the University of Illinois grew mostly in the 1950s–60s, while Corrections grew mostly in the 1980s–90s.

<div class="left">
{% include elements/button.html link="https://github.com/UIUC-iSchool-DataViz/is445_data/blob/main/building_inventory.csv" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/PantsAreCool/PantsAreCool.github.io/blob/main/python_notebooks/building_inventory_altair.ipynb" text="The Analysis" %}
</div>