# Auto Calculate Weekly Metrics

Previously, I had an ad-hoc request from my team to automatically calculate the previous week/s metrics from an existing dataset. Then output the result on a graph for our weekly presentation. The metrics should auto-update as soon as the first day of the new week rolled around. The request was dry and clear. However, I thought it would be interesting to allow a stakeholder to select how many previous weeks they want to go back in order to acquired the metrics. 

### __Goal:__
Say, a stakeholder wants to look at simple sale metrics for an entire week of 3 weeks ago instead of the previous week. The interactive worksheet should allow the user to input a numeric integer and should automatically aggregate metrics for that week. The worksheet should include a table breakdown of the aggregation of sale records by total unit sold of all items as well as total orders placed.  

### __Breakdown:__
__Data:__

[Weekly Metrics](https://docs.google.com/spreadsheets/d/1JBvwVCxa1fj_U2gtwcAsULVV9XeZANCeYM-Z-YGJdnA/edit?usp=sharing) is a sample dataset I created randomly for demonstration purposes. It contains sale records of an office supply store. __DISCLAIMER:__ this is not reflective of my current or past works at my employment. This is meant for personal education and providing learning tips or tricks. 

- Table Schema:

| field             | type     |
| ----------------- | -------- |
|Date               | DATE     |
|Order Id           | INTEGER  |
|Units Ordered      | INTEGER  |
|Item Id            | INTEGER  |
|Item Type          | STRING   |
|Customer Id        | INTEGER  |

__Data Aggregation:__

To keep it simple, metrics are the aggregated sum of the total unit ordered of each item type. The aggregation will be grouped into days of the week, with Monday as the starting date. Finally, each day will also have aggregated sum of the total order placed.   

![image1](https://github.com/mbo0000/RandomStuffs/blob/main/SpreadSheet%20Things/Weekly%20Metric/images/image1.PNG)

__User Input:__

In cell `J7` of the `Data` tab is a cell where a stakeholder can input the whole numeric number of how many weeks ago they would like to obtain. Due to the limit of available data, the max is 3 weeks. This should be the only place where the user is allowed to interact with the worksheet in a __final production__ worksheet.

![image2](https://github.com/mbo0000/RandomStuffs/blob/main/SpreadSheet%20Things/Weekly%20Metric/images/image2.PNG)

__Functions:__

My approach is to obtain the date of the previous Sunday. For example, as of writing this, the date for today is 02/4/2022. Therefore, the previous date for Sunday is 01/30/2022. From there, the rest of the week can be calculated by taking that Sunday - 1, and so on until Monday. The formula to obtain the previous Sunday is `=TODAY() - WEEKDAY(TODAY(), 2)`. The `2` parameter in `WEEKDAY` function is to indicate Monday as the starting day of the week. However, in my worksheet, `TODAY()`s will be replaced with the literal 02/04/2022 in cell `J6`. The actual formula is `$J$6- WEEKDAY($J$6,2)`

In order for the date range to update based on the input from the user, I expanded on the previous formula with `($J$6- WEEKDAY($J$6,2)) - (7*($J$7-1))` in cell `P16`. In short, `$J$6- WEEKDAY($J$6,2))` will give me the previous Sunday date. `(7*($J$7-1)` will result in the total number of days to subtract based on user input. If the input is 2 weeks ago, the Sunday of 2 weeks ago is equal to the previous Sunday date - 7 additional days.

__Output:__

As shown below, the chart output is a stacked column chart with stacks of the item types. In addition, I also threw in the Total Order Placed metric. This will give the stakeholder an additional insight into sale activities where units sold alone might not be adequate

![image3](https://github.com/mbo0000/RandomStuffs/blob/main/SpreadSheet%20Things/Weekly%20Metric/images/image3.PNG)
