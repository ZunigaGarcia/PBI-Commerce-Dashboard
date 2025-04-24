# PBI Commerce Dashboard
The following will be an explanation of the dashboard, its components, and some general insights, remember that the dashboard consisted of a final project for educational purposes, the insights are somewhat self-explanatory, since the original dataset was not very large.


$${\color{red}In \space advance, \space the \space screenshots, \space formulas, \space and \space components \space of \space the \space .PBIX \space file \space are \space in \space Spanish, \space since \space the \space dataset \space and \space the \space project \space were \space for \space a \space native \space Spanish \space speaker.}$$

## Overview
The dashboard consists of an operational and general view of some commercial aspects of a chain of stores, I sought to apply an attractive design with soft colors and a data segmenter to filter the information shown in the graphs at a continental level. 
While there are other insights that could have been added, (such as top countries with higher profits/sales, performance difference between sales channels, KPIs based on figures such as profits versus region/country, etc) we chose to leave the ones that seem more general and relevant, without overloading the dashboard or the visual load of it.

![imagen](https://github.com/user-attachments/assets/d332b42f-6567-4bad-820e-ff5e8b7679a8)

## Power Query Transformation
The following transformation was performed when importing the data:
1. Top rows removed (There was a leftover row as a title in the original dataset that made all other columns blank)
2. Headers promoted, the real column names were set as headers
3. Type changed1: Power Query changes the types automatically when promoting headers.
4. Currency format: The currency symbol was removed from the original dataset which did not allow the columns to be read as numbers
5. Changed type price-cost: The data type of the price and cost columns was reassigned from text to fixed decimal to recognize decimal points and decimal commas.
Immediately Power Query identifies it as currency.
After the transformation in Power Query the data was ready for the Dashboard creation.

## Medidas Creadas

**Gross Profit:** Subtraction of the selling price minus the purchase price of the products.
DAX Formula: 

`Ganancia Bruta = SUM('farmacias'[ Importe_venta_total ]) - SUM('farmacias'[ Importe_coste_total ])`

**Product margin:** Calculation of the aggregate profit margin left by the sale of the different products, expressed as a percentage, based on the purchase cost versus their selling cost.
DAX Formula:

`Margen Producto = 
CALCULATE(
    DIVIDE(
        SUM('farmacias'[ Importe_venta_total ]) - SUM('farmacias'[ Importe_coste_total ]),
        SUM('farmacias'[ Importe_venta_total ]),
        0
    ),
    VALUES('farmacias'[Tipo_de_producto])
)`

**Delivery Time:** Total number of days between the date of the order and the date of shipment of a product.
DAX Formula:

`“Tiempo de Entrega = DATEDIFF('farmacias'[Fecha_pedido], 'farmacias'[Fecha_envío], DAY)”`

**Average Lead Time:** Average of the total of the previous calculated measure “Lead Time”, both measures were used to group these averages by product type in a table.
DAX Formula:

`Promedio Tiempo de Entrega = AVERAGE('farmacias'[Tiempo de Entrega])`

## Design and Graphics/Charts
### Data Segmenter
To the total left a data segmenter was added so that the graphs are adjusted according to the zone of choice, different countries can be chosen using CTRL + Left Click.
It was done by zones because the countries are too numerous to be considered practical, in addition to this, the ring chart “Quantity of orders by zone” is exempt from being affected by this segmenter, since it would not make sense for it to do so.
Similarly, the Product Type vs. Product Margin matrix is not affected by the segmenter either, since the Margin column is composed in absolute values of the entire dataset by the DAX formula of the measure.

### Sales by type of product and Profits by Product
The first graph shows the number of orders for the different products, whose quantity is similar, with the detail that the most sold products are apparently food products.
The second graph shows the profits from the sale of each type of product, where it can be seen that, although household products and cosmetics are not the best sellers in terms of quantity (the latter being the least sold), they represent the highest profits (Profit), while food products represent the lowest profits.

![imagen](https://github.com/user-attachments/assets/400a0725-e02a-4cdb-89f4-fdba788800df)

### Profit by year and month
Gross Profit represented over time in a line graph, in this case, from the oldest date in the dataset to the most recent (in a real environment it would change to be a certain range established with a date-based measure, say n quarters or any other prior time period of choice), it is evident that the rebound dates have been in both August 2021 and 2022, based on the previous graphs, presumably from cosmetics and/or appliance sales.
A downward trend line (dotted line) is observed, as while those August occasions have been upturns, from the middle of 2021 to the last date of the dataset there is less stability in gains in the other months.

![imagen](https://github.com/user-attachments/assets/65791c37-8b41-4bbb-b1a8-a23aa26dc743)

### Amount of orders by zone
Ring chart showing the values and total percentage of the amount of sales in each zone, showing that the predominant zones in the market are Africa, Europe and Asia, with North America being the region with the least impact.
In the center is shown the total gross profit, the sum of which is 382.03 million €.

![imagen](https://github.com/user-attachments/assets/276de8e7-d48e-4b3a-b715-7703a46441ab)

### Orders by priority
Ring chart showing that orders are distributed homogeneously by priority, with almost 25% of each category. This chart could be more useful in a context where orders are updated in real time, and in a season where the priorities of current orders fluctuate, in order to make decisions based on this.

![imagen](https://github.com/user-attachments/assets/e4e42e98-e47e-4f17-8ca6-ce4c7de628cd)

### Product Margin and Average Delivery Time Matrices
The remaining graphs are matrices representing:
1. The profit margin generated by each type of product (clothing being the highest, as has traditionally been the case), and meat products the lowest.
2. The average delivery time for each type of product, again with clothing being the highest (longest), with a difference of 7 days with the lowest average, which is occupied by meat products.

![imagen](https://github.com/user-attachments/assets/43b76652-c9a3-4c9b-8f13-31876dfeefec)

