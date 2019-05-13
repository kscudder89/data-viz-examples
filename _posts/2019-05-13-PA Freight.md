---
title: "PA Freight"
date: 2019-05-13
published: true
tags: [dataviz, altair, vega-lite, observable, holoviews]
excerpt: "Embedding interactive charts on static pages using Jekyll."
altair-loader:
  altair-chart1: "charts/PaFreightAltairMap2.json"
  altair-chart2: "charts/PaFreightByModeAltairChart.json"
  

toc: true
toc_sticky: true
---

This post will show interactive charts produced using [Altair](https://altair-viz.github.io) of [PA Freight Data] (https://ops.fhwa.dot.gov/freight/freight_analysis/faf/index.htm)

## Chart 1- Total PA Freight from 2012 to 2017

Below is a map of the total KTons of freight delivered to each state originating from Pennsylvania.

<div id="altair-chart1"></div>

Below is the python code used to create the above map:

```python
data  = alt.InlineData(values=PA_freight_grouped_json,
                       format=alt.DataFormat(property='features',type='json'))

# plot map, where variables ares nested within `properties`, 
chart = alt.Chart(data).mark_geoshape().properties(
    width=700,
    height=400,
    projection={"type":'albersUsa'},
    title = "KTons of Freight Originating in Pennsylvania (2012 - 2017)"
).encode(
    tooltip=[alt.Tooltip('properties.DMS DEST:N', title='State'), 
             alt.Tooltip('properties.KTons:Q', 
                         title="Total KTons"
                        )
            ],
    color=alt.Color('properties.KTons:Q', 
                    title="KTons of Freight", 
                    scale=alt.Scale(scheme='redpurple',type='log'))
)
chart
```

## Chart 2- PA Freight by Mode

Below is a chart showing the total KTons of freight BY MODE delivered to each state originating from Pennsylvania.

<div id="altair-chart2"></div>

Below is the python code used to create the above map:

```python
chart = alt.Chart(PA_freight_byMode).mark_bar().encode(
    x=alt.X('KTons:Q', aggregate='sum', sort='descending', stack='normalize'),
    y='DMS DEST',
    color='DMS_MODE:N',
    ).interactive()

chart
```

