# Project Overview
## [To GitHub Repository](https://github.com/jporonovich?tab=repositories)
## To the Reader 
Last updated: April, 2021 <br />
By : Jordan P. <br />

*Please note, this is an ongoing project. I  clock-in a couple hours a week to move the needle but have no set timeline.*<br />

## Premise 
The subject or premise of this project is to study the correlation between economic recessions and the use of vices such as alcohol, cannabis, and tobacco. This project is done to polish my programing skills. While most of the analysts can easily be completed in MS excel or Google Sheets, the way of the future is through computer languages. <br />

An article published on WebMD, “[As Economy Goes Down, Drinking Goes Up]( https://www.webmd.com/mental-health/addiction/news/20111013/as-economy-goes-down-drinking-goes-up)” suggested that drinking and alcohol consumption are inversely correlated in the United States. <br />

Initial research shows that between 2000 and 2020 alcohol spending and Canadian GDP are both positively correlated not negatively correlated (Coef: 0.97). The alcohol industry grew and shrank with the Canadian economy over the last 20 years. However, this period captures a 20-year trend not consumer purchasing behavior during an economic recession.<br />

An inverse trend is seen in the tobacco industry. From 2004 to 2020 cigarettes sales and canadian GDP were negatively correlated with a Corr. coefficient of -0.72. In other words, tobacco sales decreased as the Canadian economy increased. This trend is representative of Health Canada's war on tobacco and part of a larger smoking cessation movement.<br />

Taking a closer look, the latest annualized Canadian GDP data shows a -5.4% Year-Over-Year (“YoY”) 2019 to 2020. In the same period, alcohol sale increased 7.3% and monthly cannabis sales increased over 30% YoY.<br />

Contrary to alcohol and cannabis sales, tobacco sales fell -3.3% YoY which is a larger decrease than the -2.4% Compound Annual Growth Rate(“CAGR”) over the last 15 years. This is surprising as the year following the 2009 Canadian recession where GDP decreased roughly -2% tobacco sales soared 15%. <br />


These are the question I hope to answer by the end of this project.<br />
* Will alcohol sales remain high or will sales revert towards the average?
*	Cannabis sales have exploded, is this due to the novelty, the fact it’s a nascent industry, the recession, will there be a correction?
*	Will we see an up tick in tobacco sales and is the 2010 example a lagging indicator? 
<br />

*All great question and all in due course...*
<br />

Sources: Statistics Canada. All CSV files available in GitHub repository.

![Indexed Chart](https://raw.githubusercontent.com/jporonovich/VicesAndEconomics/main/Pictures/LineChart_index.png?token=AFSI26D2IILKJBCQJ4RJYOTALT2E2)

<br />

## Project Status


|**Section**<img width=250/>|**Tools**<img width=100/>|**Status**<img width=100/>|
|:-|:-|:-|
|Discussion & Analysis|*na*|On-going|
|Automate Cleanse|Python|Complete|
|Per Capita spending|Python|Complete|
|Consolidate files|R & Python|Complete|
|Prepare Shape file|R|Complete|
|Dashboard: LineChart|R|Complete|
|Dashboard: HeatMap|R|On-Going|
|Static Visualizations & Tables|R|Ongoing|
|Regression Model|R or Python|Ongoing|

## Automate Data Cleansing & GDP per Capita **'Python'**
[To Repository](https://github.com/jporonovich/Pyhton_AutomateDataCleanse)<br />
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

## Interactive Dashboard & File Manipulation **'R'**
[To Repository](https://github.com/jporonovich/R.Shiny_InteractiveDashboard)<br />
RStudio <br />
Last updated: March, 2021 <br />
Status: Heatmap and dropdown menu completed. Next step, link user input and heatmap.
<br />

Styling will be completed once all the core funcitons are in place. <br />
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
