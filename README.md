# To the Reader 
By : Jordan P. 
Last updated: March, 2021 <br />

*Please note, this is an ongoing project. I  clock-in a couple hours a week to move the needle but have no set timeline.*<br />

# Summary 
The subject or premise of this project, is to study the correlation between economic recessions and the use of vices such as tobacco and alcohol. An article published on WebMD, “[As Economy Goes Down, Drinking Goes Up]( https://www.webmd.com/mental-health/addiction/news/20111013/as-economy-goes-down-drinking-goes-up)” suggested that drinking and alcohol consumption are inversely correlated in the united states. 
<br />

Preliminary research of Canadian data shows that total alcohol spending across Canada and Canadian GDP are both positively correlated with a coefficient of 0.97. This simply confirms that people buy more when they have more money.
<br />

Cigarettes on the other hand show a negative correlation coefficient (-0.72) which is representative of the fact that most people in Canada no longer smoke. 
<br />

To be continue… 

![Indexed Chart](https://raw.githubusercontent.com/jporonovich/VicesAndEconomics/main/Pictures/LineChart_index.png?token=AFSI26HPZBA63GCGUSZ2UBTAKFMOE)

<br />

## Status


|**Section**<img width=200/>|**Language**<img width=200/>|**Status**<img width=200/>|
|:-|:-|:-|
|Analysis|Python & R |On-going|
|Automate Cleanse|Python|Complete|
|Calculate Per Capita|Python|Complete|
|Consolidate files|R|Complete|
|Prepare Shape file|R|Complete|
|LineChart|R|Complete|
|HeatMap|R|On-Going|


# Automate Data Cleansing & GDP per Capita **'Python'**
[To Repository](https://github.com/jporonovich/Pyhton_AutomateDataCleanse)
Microsoft Visual Studio <br />
Last updated: March, 2021 <br />
Status: Complete

*All data publicly available on StatsCan [(Click Here)](https://www150.statcan.gc.ca/n1//en/type/data?MM=1#tables).*

Prepared the below CSV files for analysis. Removed all non-pertinent information. (e.g. Drop rows and columns, new headers, ensure CSVs are ready to be manipulated). Merge alcohol sales and population information create new "Alcohol spending per capita" from 2000 to 2020 by province.


The reason I automated the data cleanse is so that I can create a cycle of puling data and uploading data to a dashboard with little or no human intervention.  

*Before & After Example.*

![before&after](https://raw.githubusercontent.com/jporonovich/Pyhton-Wrangling_DataCleanseAuto/main/Before%20%26%20After.PNG)

* [x] Automate Cleanse 
  * [x] Canada_GDP.CSV 
  * [x] AlcoholSales.CSV
  * [x] TabaccoSales.CSV
  * [x] CanadaPopulation.csv
* [x] Merge files and create new data 
  * [x] AlcoholSales.CSV
  * [x] CanadaPopulation.csv

### Snippet from 'CleanCSVFiles.py'
*full file available in repository*

 ```python 

    #This block of code  takes data from StatsCan isolates the tables, 
    #removes "clutter" and transposes the table from horizontal to vertical

    #Defining headers to pre-exisiting row and resetting header.   
    header_names = list(GDP.iloc[8])
    header_names[0] = NaN # setting first index to NA
    GDP.columns = header_names

    #Selecting relevant table
    GDP = GDP.iloc[10:41]

    #Converting strings to floats
    #Getting table dimensions
    Num_Col = len(GDP.columns)
    Num_row = len(GDP.index)

    #Indexing position at one to avoid string convertion error. 
    i_column = 1
    i_row = 0

    #removing "," from numbers and converting to floats
    while i_column < Num_Col:
        while i_row < Num_row:
            GDP.iloc[i_row,i_column]  = float(GDP.iloc[i_row, i_column].replace(",",""))
            i_row += 1
        i_column += 1
        i_row = 0

    #Saving to new file
    GDP.to_csv("Canada_GDP(Clean).csv", index = False)
   
 ```

# Interactive Dashboard & File Manipulation **'R'**
[To Repository](https://github.com/jporonovich/R.Shiny_InteractiveDashboard)
Microsoft Visual Studio <br />
Last updated: March, 2021 <br />
Status: Heatmap and dropdown menu completed. Next step, link user input and heatmap.


Consolidate and clean Alcohol, Tobacco, GDP and CPI data into files into three separate CSV files (Monthly, Quarterly, Yearly). Merge .shp file with Alcohol sales per capita csv file and finally Developed dashboard for visualization & analysis


![Dashboard visualization](https://raw.githubusercontent.com/jporonovich/R_-_DataWrangling_Dashboard-Shiny/main/Dashboard(Work-In%20Progress).PNG)


* [x] Consolidate files (Monthly, Quarterly, Annual) 
  * [x] AlcoholSales(Clean).csv
  * [x] Canada_CPI(Clean).csv
  * [x] Canada_GDP(Clean).csv
  * [x] TobaccoSales(Clean).csv

* [x] Merge Shape file(.shp) and Alcohol per capita  
  * [x] gpr_000b11a_e.shp
  * [x] consolidatedAlcoholPercapita.csv

* [ ] Interactive Dashboard.
  * [x] Line chart Percentage increase/decrease
  * [ ] Heat map of canada provincial alcohol sales _(Work in progess)_ 

### Snippet from Dashboard.R
*Full file available in repository*
 
``` r

#Dynamic Line chart
           
ggplot(ConsolidatedAnnual, aes(x = Year)) +
geom_line(aes(y = if (is.na(match("Dollar Value",input$SourceData))) {GDP.Prct.Chg} else {GDP}), 
    col = if (is.na(match("GDP",input$ThreeMetrics))) {NA} else {"#0e7bcf"}, 
    na.rm=TRUE, size = 1.15 )+ # Adding GDP % Change
geom_line(aes(y = if (is.na(match("Dollar Value",input$SourceData))) {Alcohol.Prct.Chg} else {Alcohol.Sales.CAD}),
    col = if (is.na(match("Alcohol",input$ThreeMetrics))) {NA} else {"#de9307"},
    na.rm=TRUE, size = 1.15) + # Adding Alcohol % Change
geom_line(aes(y = if (is.na(match("Dollar Value",input$SourceData))) {Tobacco.Prct.Chg} else {Tobacco.Sale.CAD}),
    col = if (is.na(match("Tobacco",input$ThreeMetrics))) {NA} else {"#08a65c"},
    na.rm=TRUE, size = 1.15) + # Adding tobacco % Change
    ylim(t = if (is.na(match("Dollar Value",input$SourceData))) {c(-20,20)} else {c(0,25000000)}) + #setting y range
    xlim(2000,2020)+
    xlab("Year") + #renaming x axis
    ylab(if (is.na(match("Dollar Value",input$SourceData))) {"Percentage Change(%)"} else {"Dollar Value CAD"})+ #renaming y axis
    ggtitle("Annual Expenditures & Growth/Decline")+
    theme(
        panel.background = element_blank(),
        panel.grid = element_blank(),
        #panel.grid.major.x = element_line(color = "gray50", size = 0.05),
        panel.grid.major = element_line(color = "gray50", size = 0.05),
        plot.title = element_text(size = 14, face = "bold.italic", color = "#0c73c2")

```
