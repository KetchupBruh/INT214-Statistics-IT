# Dataset
choose Superstore Sales Dataset (Data from Rohit Sahoo,Kaggle)

## Step
1. Explore the dataset
2. Transform data with dplyr and finding insight the data (Learning)
3. Visualization with GGplot2 (Learning)

## Step 1 Explore the dataset
```
# Import library
library(readr)      
library(dplyr)      
library(DescTools)      
library(ggplot2)    
library(scales)    

#Import dataset
super <- read.csv("https://raw.githubusercontent.com/sit-2021-int214/036-Mobile-App-Store/master/assignment/HomeWork04/HW04_63130500148/Superstore%20Sales%20Dataset.csv")

```
datatype ทั้งหมด
```
glimpse(super)
Rows: 9,800
Columns: 18
$ Row.ID        <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, ~
$ Order.ID      <chr> "CA-2017-152156", "CA-2017-152156", "CA-2017-138688", "US-2016-108966", "US-2016-108966", "CA-2015-~
$ Order.Date    <chr> "08/11/2017", "08/11/2017", "12/06/2017", "11/10/2016", "11/10/2016", "09/06/2015", "09/06/2015", "~
$ Ship.Date     <date> 2020-11-11, 2020-11-11, 2020-06-16, 2020-10-18, 2020-10-18, 2020-06-14, 2020-06-14, 2020-06-14, 20~
$ Ship.Mode     <chr> "Second Class", "Second Class", "Second Class", "Standard Class", "Standard Class", "Standard Class~
$ Customer.ID   <chr> "CG-12520", "CG-12520", "DV-13045", "SO-20335", "SO-20335", "BH-11710", "BH-11710", "BH-11710", "BH~
$ Customer.Name <chr> "Claire Gute", "Claire Gute", "Darrin Van Huff", "Sean O'Donnell", "Sean O'Donnell", "Brosina Hoffm~
$ Segment       <chr> "Consumer", "Consumer", "Corporate", "Consumer", "Consumer", "Consumer", "Consumer", "Consumer", "C~
$ Country       <chr> "United States", "United States", "United States", "United States", "United States", "United States~
$ City          <chr> "Henderson", "Henderson", "Los Angeles", "Fort Lauderdale", "Fort Lauderdale", "Los Angeles", "Los ~
$ State         <chr> "Kentucky", "Kentucky", "California", "Florida", "Florida", "California", "California", "California~
$ Postal.Code   <int> 42420, 42420, 90036, 33311, 33311, 90032, 90032, 90032, 90032, 90032, 90032, 90032, 28027, 98103, 7~
$ Region        <chr> "South", "South", "West", "South", "South", "West", "West", "West", "West", "West", "West", "West",~
$ Product.ID    <chr> "FUR-BO-10001798", "FUR-CH-10000454", "OFF-LA-10000240", "FUR-TA-10000577", "OFF-ST-10000760", "FUR~
$ Category      <chr> "Furniture", "Furniture", "Office Supplies", "Furniture", "Office Supplies", "Furniture", "Office S~
$ Sub.Category  <chr> "Bookcases", "Chairs", "Labels", "Tables", "Storage", "Furnishings", "Art", "Phones", "Binders", "A~
$ Product.Name  <chr> "Bush Somerset Collection Bookcase", "Hon Deluxe Fabric Upholstered Stacking Chairs, Rounded Back",~
$ Sales         <dbl> 261.9600, 731.9400, 14.6200, 957.5775, 22.3680, 48.8600, 7.2800, 907.1520, 18.5040, 114.9000, 1706.~
```

ข้อมูลของ dataset Super_Sales
|               |      Column Name      | Details of Column              |  Dataype  |
| ------------- |:-------------------:  | :---------------------------:  |  -----: |
| 1             | Row.ID                | หมายที่ของแถวนั้นๆ                |  Int |
| 2             | Order.ID              | หมายที่ของคำสั่งซื้อ                | Character  |
| 3             | Order.Date            | วันที่ของคำสั่งซื้อ                  |  Character |
| 4             | Ship.Date             | วันที่จัดส่ง                        | Character  |
| 5             | Ship.Mode             | รูปแบบการจัดส่ง                   | Character  |
| 6             | Customer.ID           | หมายเลขประจำตัวของลูกค้า          | Character  |
| 7             | Customer.Name         | ชื่อของลูกค้า                     |  Character |
| 8             | Segment               | ประเภทของลูกค้า(เช่น องค์กร)      |  Character |
| 9             | Country               | ประเทศที่ลูกค้าอยู่                 | Character  |
| 10            | City                  | เมืองที่ลูกค้าอยู่                    |  Character |
| 11            | State                 | รัฐที่ลูกค้าอยู่                      | Character  |
| 12            | Postal.Code           | รหัสไปรษณีย์ที่ลูกค้าอยู่             |  Int |
| 14            | Region                | ภูมิภาคที่ลูกค้าอยู่                  |  Character |
| 15            | Product.ID            | หมายเลขสินค้า                     |  Character |
| 16            | Category              | หมวดหมู่สินค้า                      | Character  |
| 17            | Sub.Category          | หมวดหมู่ย่อยของสินค้า                | Character  |
| 18            | Product.Name          | ชื่อสินค้า                            | Character  | 
| 19            | Sales                 | ราคาสินค้า                          | Double  | 

## Step 2 Transform data with dplyr and finding insight the data (Learning)
1.สินค้าที่มีหมวดหมู่หลักเป็น Furniture และ หมวดหมู่ย่อยเป็น table 7 ตัวแรก
```
super %>% select(Row.ID,Category,Sub.Category) %>% filter(Category == "Furniture" ,Sub.Category == "Tables") %>% head(7)
```

Result
```
  Row.ID  Category Sub.Category
1      4 Furniture       Tables
2     11 Furniture       Tables
3     25 Furniture       Tables
4    118 Furniture       Tables
5    126 Furniture       Tables
6    202 Furniture       Tables
7    227 Furniture       Tables
```
2.หาราคารวมของ sales ในแต่ละ state

`group by ใช้จัดกลุ่มข้อมูล`
```
super %>% group_by(State) %>% select(State,Sales) %>% summarise(sum_sales = sum(Sales)) 
```

Result
```
# A tibble: 49 x 2
   State                sum_sales
   <chr>                    <dbl>
 1 Alabama                 19511.
 2 Arizona                 35273.
 3 Arkansas                11678.
 4 California             446306.
 5 Colorado                31842.
 6 Connecticut             13384.
 7 Delaware                27323.
 8 District of Columbia     2865.
 9 Florida                 88437.
10 Georgia                 48219.
# ... with 39 more rows
```
3.หาชื่อสินค้าที่เป็น หมวดหมู่ย่อยเป็น phone และราคามากกว่า 500 โดยเรียงจากน้อยไปมาก

`arrange ใช้สำหรับเรียงข้อมูลจากน้อยไปมาก (ถ้าใช้ desc ใน arrange จะจากมากไปน้อย)`
```
super %>% select(Product.ID ,Sub.Category ,Sales) %>% filter(Sub.Category == "Phones" ,Sales > 500) %>% arrange(Sales)
```

Result
```
         Product.ID Sub.Category    Sales
1   TEC-PH-10003273       Phones  503.960
2   TEC-PH-10002597       Phones  503.960
3   TEC-PH-10000441       Phones  503.960
4   TEC-PH-10000369       Phones  503.960
5   TEC-PH-10003691       Phones  503.960
6   TEC-PH-10004042       Phones  508.768
7   TEC-PH-10002555       Phones  517.900
8   TEC-PH-10003505       Phones  519.680
9   TEC-PH-10003580       Phones  519.792
10  TEC-PH-10000526       Phones  537.544
# ... with 198 more rows
```
4.หาจำนวนสินค้าที่มี segment เป็น consumer
```
super %>% select(Product.ID ,Segment) %>% filter(Segment == "Consumer") %>% count()
```

Result
```
     n
1 5101
```
5.จงหา Product ID ที่มีราคาสูงที่สุด โดยมี Segment เป็น Home Office
```
super %>% filter(Segment == "Home Office" , Sales == max(Sales))  %>% select(Product.ID ,Segment ,Sales)
```

Result
```
       Product.ID     Segment    Sales
1 TEC-MA-10002412 Home Office 22638.48
```
6.หาราคาสินค้าที่มากที่สุดในเดือนนั้นๆ (รวมทุกปี)
```
super$Ship.Date <- as.Date(super$Ship.Date, format = "%d/%m/%y")
super %>% mutate(month = Month(Ship.Date)) %>% group_by(month) %>% summarise(Max_prices = max(Sales)) %>% arrange(month)
```

Result
```
   month Max_prices
   <int>      <dbl>
 1     1      5444.
 2     2      8750.
 3     3     22638.
 4     4      9100.
 5     5      8400.
 6     6      4477.
 7     7      8188.
 8     8      4416.
 9     9      9450.
10    10     17500.
11    11     10500.
12    12      9893.
```

## Step 3 Visualization with GGplot2 (Learning)
1.กราฟที่แสดงถึงประเภทการค้นส่งสินค้าทั้งหมดที่เคยสั่งชื้อสินค้า
`geom.text คือข้อมูลที่อยากให้แสดงผลบนกราฟ`
`theme_void คือการเอาพื้นหลังออก`
```
graph_ship <- data.frame(table(super$Ship.Mode))
graph_ship <- graph_ship %>% rename("ship"=Var1,"count"=Freq)

graph_ship %>% ggplot(aes("x",y=count,fill=ship)) + 
               geom_bar(stat="identity", color="black") +
               coord_polar("y") + theme_void() +
               geom_text(aes(label = percent(count/sum(count))),
               position = position_stack(vjust = 0.49))
```

Result

![plot1](https://github.com/KetchupBruh/Power-BI-and-R-language/blob/main/images%20and%20dataset/plot1.png)

2.กราฟที่เเสดง category ของสินค้าทั้งหมด
```
Cat_plot <- ggplot(super,aes(x=Category)) + geom_bar(fill="#FF4118")
Cat_plot + ggtitle("Number of Category") +
           xlab("Category") + ylab("Number of Category")
```
Result

![plot2](https://github.com/KetchupBruh/Power-BI-and-R-language/blob/main/images%20and%20dataset/plot2.png)
