# VAFinalProject

## Background 

This project is a Visual Analytics Dashboard created by Bryce Walsh, Nikhil Bhatia-Lin, and Charlie Caron for the Visual Analytics course at Tufts University in the Fall of 2020. The goal of the project was to build a dashboard to solve one of the mini challenges that were part of the VAST challenge held at the VAST conference in 2019.

Information on the VAST challenge 2019: https://vast-challenge.github.io/2019/overview.html

Information on the specific mini-challenge we endeavored to solve: https://vast-challenge.github.io/2019/MC2.html

## Description of the Dashboard
A live demo of the dashboard is available at: https://bryce-walsh.github.io/VAFinalProject/map.html

Our system is divided into two views. On the left is a map of St. Himark where the color
of each neighborhood maps to the estimated average radiation level of that neighborhood
(Green=Low, Red=High). There is a checkbox on the lower left that toggles whether the map
will alter the opacity of a neighborhood based on the average uncertainty of that neighborhood
(as calculated by the data processing pipeline below). The system also shows the locations of
all of the static sensors and mobile sensors as blue and yellow dots respectively. The user can
mouse over these dots to see a tooltip which displays the id of the sensor as well as it’s typeand uncertainty value. They can also mouse over a neighborhood and the name of the neighborhood as well as its radiation and uncertainty values for the current timestamp will
appear in the upper left corner of the map area.
The right view consists of two line graphs of the average radiation level over time as
calculated from either the mobile sensors (top) or the static sensors (bottom). The slider across
the top controls the timestamp displayed on the map and is lined up with X-axis of these graphs
so that the user can drag the slider to view the map at a particular timestamp which they find
interesting on the graph. There is also a play/pause button which allows the user to watch the
data on the map play out over time.

## Data Analysis Process
Most of our data analysis took place in calculating the uncertainty values for each sensor
and then creating a combined value for each neighborhood. In order to create an uncertainty
value, we joined the static and mobile datasets, and selected only rows in which there was a
mobile sensor within 0.7 kilometers of a static sensor. 0.7 kilometers was chosen because it
was close enough that we thought the readings should be roughly the same, but far enough that
most mobile sensors would pass within this range. We treated the static sensor as ground truth,
so they had an uncertainty of 0. The uncertainty value that we calculated for each mobile sensor
was the average difference between the mobile sensor and static sensor readings when they
were within 0.7 km. For sensors that didn’t pass within this range, we gave the maximum
uncertainty value of 32.
Once we had uncertainty values for each sensor, we calculated uncertainty and radiation
values for each neighborhood. A neighborhood’s radiation value was calculated based on the
average reading of the sensors within the bounding rectangle of a neighborhood (the rectangle
that fully captures the neighborhood) plus a buffer of 20px (since we thought that sensors
slightly outside the neighborhood would still be pertinent). The uncertainty value was calculated
based on the average uncertainty of the sensors within this bounding rectangle, divided by the
number of sensors in the neighborhood (since more sensors means less uncertainty).
The data from the mini challenge has readings for every sensor taken every five seconds
across the 4 day period. In order to decrease the size of the dataset and speed up data
processing, we preprocessed the data by averaging the readings for every 5 second period
across each 10 minute period. The result was a dataset with granularity down to every 10
minutes which we used for the above analysis.

## Findings
Based on the data in the static and mobile sensor graphs, the earthquake occurs on
Thursday evening at around 4 pm. It is at this point that we see a large spike in both the mobile
sensor and static sensor readings. The east side of the town seems to have been hit hardest,
and has the highest radiation readings. Particularly in Safe Town, Cheddarford, Terrapin
Springs, Wilson Forest, and Scenic Vista, there are very high readings that indicate highradiation. In the southeast corner, there are readings above 1000, but also higher uncertainty
levels.
