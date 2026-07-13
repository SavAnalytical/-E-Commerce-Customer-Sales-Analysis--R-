### E-Commerce Customer Sales-Analysis
*This project will analyze the utilization of the e-commerce (online) website in driving sales. Data collected at Blackwell's e-Commerce customers' transaction data over a year will be analysed to identify purchasing patterns between  online and in-store modes of purchase.*

---

### 🌐 Project Overview
The goal of this project is to identify purchasing patterns. While doing that, this project would like  to answer the following questions?

     - Do customers in different  regions spend more per transaction? ( North, South, East, and West) And which region spends the most? 
    
    -If older customers buy more online than in-store, according to our VP of sales, how much did they spend in total and per transaction? 
  
    -What amount of electronics do older customers purchase per transaction? Do they buy more electronics online compared to younger customers who buy in-store? 
  
    -Does the age of a customer influence how much customers spend per transaction and in total?

  
###  📊  Data source
  The primary dataset used for this analysis is the Demographic_Data(5).csv file, containing detailed information about each sale made by Blackwell Company.
  - **Size:** ~80,000 transactions
  - **Fields:** `purchase_channels`, `age`, `items`, `amount`, `region`.          
---

### 🛠 Tools & Libraries
  | Category | Tools |
|---|---|
| Language | R |
| Data Wrangling | `Excel`, `dplyr`, `tidyr`, `lubridate` |
| Visualization | `ggplot2`, `scales` |
| Reporting | R Markdown (`github_document`) |
---

### 🔍 Methodology
1. ** Data Cleaning** — handled missing values and removed duplicate transactions.
   
   🧹Removal of duplicates using the code below
    ```r
    ecom_clean <- ecom[!duplicated(ecom), ]
    ```
---

2. **Exploratory Data Analysis** — distribution checks, outlier detection, category-level summaries.

   *(outlier detection using boxplot)*
    ```r
    boxplot(ecom_clean$amount) 
    ```
    
**category-level summaries**

    ```r
    ecom_mapped$purchase_channels <- factor(ecom_mapped$purchase_channels,
                        levels = c(0, 1),
                        labels = c("online", "in-store"))
    ```
*factor() convert numeric region codes into meaningful categorical labels*

  ```r
ecom_mapped$region <- factor(ecom_mapped$region,
                        levels = c(1,2,3,4),
                        labels = c("North", "South", "East", "West"))
```
---

```r
ecom_mapped$region <- factor(ecom_mapped$region,
                        levels = c(1,2,3,4),
                        labels = c("North", "South", "East", "West"))
```

---   
3. **Customer age Segmentation** — Age analysis, to classify customers into tiers(Adult, Middle-Aged, and Ederly) based on the average age distributio; mean and median

```r
ecom_mapped$age_range <- cut(ecom_mapped$age,
                        breaks = c(17, 40, 60, 85),
                        labels = c("Adult", "Middle-Aged", "Elderly"),
                        right=TRUE)
````


4. ** 📈  Trend Analysis and 💹 Visualizations** — monthly revenue trends and seasonality






5. **Insight Synthesis** — translated statistical findings into business recommendations










