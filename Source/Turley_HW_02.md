Turley_ProjectHW_02
================
sally_turley
2023-03-02

`{r setup, include=FALSE} knitr::opts_chunk$set(echo = TRUE)`

``` {r}
readcsv()

View(Project02_SuperStoreOrders)

superstore <- Project02_SuperStoreOrders
```

## Create a Summary Statistic

`{r super store orders} summary(superstore)`

# The data show that the data is clean and does not have an abundance of

values which need to be removed. In fact, there are none that I can
identify. All of the data has the same length so we are able to
manipulate it easily.

The data ranges from 2011 to 2014 and examines the sales, quantity
purchased, discounts recieved, profits earned, shipping costs and the
urgency (priority) assisgned to the order.

The data is structured around the order_ID. A new order ID is created
for each purchase so a single customer can have many order IDs.

The dates are not in date format and will need to be lubridated.

Quantities are clearly discrete, and can be converted into integers
instead of doubles. (though im not sure that this is necessarily
adventageous at this time).

## Orders Over Time by Segment

``` {r}

gg_check <- "ggplot2"

# checking to see if the package is installed, and installing it if not

if(!requireNamespace(gg_check, quietly = TRUE)) 
{
install.packages(gg_check)
}
library(ggplot2)

```

\#Plot of Orders 2011- 2014 broken down into customer segments

``` {r}

order_df <- subset(superstore, select =c("order_id","order_date","year", "segment")

head(order_df)

unique(order_df$segment)

ggplot(order_df, aes(x = year, fill = segment)) +  geom_bar() +
  xlab("Year of Order") +
  ylab("Count of Orders") +
  ggtitle("Orders by Year and Segment") +
  theme(axis.text.x = element_text(angle = 45, vjust = 0.5)) +
  scale_fill_manual(values = c("#CCCCFF", "#1164B4","#FF7F50"))
```

## Which segment is the best seller?

# We can compare segments by calculating the proportion of sales per segment both yearly and overall. This way we might be able to see changes over time in the growth or decline of various sectors.

``` {r}

orders_by_segment <- aggregate(order_df$order_id, by = list(order_df$segment, order_df$year), FUN = length)
colnames(orders_by_segment) <- c("segment", "year", "count")
orders_by_segment <- orders_by_segment[order(orders_by_segment$year, decreasing = FALSE),]
View(orders_by_segment)


ggplot(orders_by_segment, aes(x = year, y = count, color = segment)) +
  geom_line(size = 1.5) +
  scale_color_manual(values = c("#CCCCFF", "#1164B4", "#FF7F50")) +
  xlab("Year") +
  ylab("Count of Orders") +
  ggtitle("Gross Orders by Segment per Year") +
  theme(legend.position = "right") 

```

# Question 4

## Bar chart of Regional Orders

Provide your analysis of which region receives the most orders and which
region receives the fewest. Ensure you provide the numbers to validate
that the longer bar corresponds to the maximum number of orders. Invert
the axis coordinates and display the bars in the reverse order of the
number of orders by region. Replace the bars’ color with the color of
your choosing. I used the color blue in the graph I made. Additionally,
ensure that the axis is labeled “Region.”

``` {r}

unique(superstore$region)

orders_by_region <- aggregate(superstore$order_id,superstore$count by = list(region = superstore$region), FUN = length)
colnames(orders_by_region) <- c("Region", "Order Count")
View(orders_by_region)


# aggregate orders by region
orders_by_region <- aggregate(order_id ~ region, data = superstore, FUN = length)
colnames(orders_by_region) <- c("Region", "Order Count")
orders_by_region <- orders_by_region[order(orders_by_region$`Order Count`, decreasing = TRUE),]




# custome colors
library(RColorBrewer)

n <- 13
palette <- colorRampPalette(brewer.pal(9, "PiYG"))(n)

# Plot

ggplot(data = orders_by_region, aes(x = reorder(factor(Region),-"Order_Count"), y = Order_Count)) +
  scale_fill_manual(values =  palette) +
  geom_bar(stat = "identity") +
  ylab("Region") +
  xlab("Count of Orders") +
  ggtitle("Count of Orders by Region") +
  theme_bw() +
  coord_flip()
  
print(n)
print(palette)
  
  
```

``` {r}
library(RColorBrewer)

n <- 13
palette <- colorRampPalette(brewer.pal(9, "RdPu"))(n)
```

``` {r}
# Aggregate orders by region
orders_by_region <- aggregate(order_id ~ region, data = superstore, FUN = length)
colnames(orders_by_region) <- c("Region", "Order Count")
orders_by_region <- orders_by_region[order(orders_by_region$`Order Count`, decreasing = FALSE),]

# Define custom color palette
library(RColorBrewer)
n <- 13
palette <- colorRampPalette(brewer.pal(9, "PiYG"))(n)

# Create bar chart
ggplot(data = orders_by_region, aes(x = reorder(factor(Region), +`Order Count`), y = `Order Count`)) +
  geom_bar(stat = "identity", fill = palette[1:length(orders_by_region$`Order Count`)], color = "grey") +
  geom_text(aes(label = `Order Count`), vjust = -0.5) +
  ylab("Count of Orders") +
  xlab("Region") +
  ggtitle("Count of Orders by Region") +
  theme_bw() +
  coord_flip()
  
```

# Question 5

## Profits by Region

\`\`\` {r}

\#second pallet palette2 \<- colorRampPalette(brewer.pal(9,
“YlOrRd”))(n)

store_data \<- aggregate(profit \~ region, data = superstore, FUN = sum)
colnames(store_data) \<- c(“region”, “total_profit”) store_data \<-
store_data\[order(store_data\$total_profit, decreasing = FALSE),\]
View(store_data)

ggplot(store_data, aes(x = region, y = total_profit)) +
geom_polygon(aes(fill = region), color = “black”, alpha = 0.5)+
scale_y\_continuous(labels = scales::number_format(scale = 1))+
geom_bar(stat = “identity”, fill = palette2 , alpha = 0.8) +
scale_fill_manual(values = palette) + coord_polar() + theme_minimal() +
theme(legend.position = “none”) + labs(title = “Total Profit by Region”,
x = NULL, y = NULL)

store_data %\>% mutate(total_profit = sum(profit)) %\>%

group_by(region) %\>% summarize(total_profit = sum(profit)) %\>%

…
