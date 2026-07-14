### E-Commerce Customer Sales-Analysis
*This project will analyze the utilization of the e-commerce (online) website in driving sales. Data collected at Blackwell's e-Commerce customers' transaction data over a year will be analysed to identify purchasing patterns between  online and in-store modes of purchase.*




### Table of Contents
[Project Overview](#ProjectOverview)

[Data source](#Datasource)

[Tools & Libraries](#Tools&Libraries)

[Methodology](#Methodology)

[Trend Analysis and 💹 Visualizations](#TrendAnalysisand💹Visualizations)

[Recommendation](#Recommendation)

[Author](#Author)





### 🌐 Project Overview
The goal of this project is to identify purchasing patterns. While doing that, this project aims to answer the following questions.

     -  Total Revenue across each region?
     
     - Total transaction across each region?
     
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
    
   --- 
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


4. ### 📈  Trend Analysis and 💹 Visualizations —
     
     Q1: Total Revenue
```r
ggplot(df2, aes(x = region, y = amount)) +
  stat_summary(
    fun = sum,
    geom = "bar",
    fill = "darkgreen",
    width = 0.7) +
  stat_summary(
    fun = sum,
    geom = "text",
    aes(label = paste0("₦", comma(..y..))),
    vjust = -0.25,# vertical justification, controls the vertical position of the text.
    size = 5,
    fontface = "bold") +
  scale_y_continuous(
    labels = function(x) paste0("₦", comma(x))) +
  labs(
    title = "Total Revenue by Region",
    x = "Region",
    y = "Total Revenue") +
  theme_minimal()
```
<img width="1174" height="603" alt="Screenshot 2026-07-13 165133" src="https://github.com/user-attachments/assets/80f9972f-4aa8-4937-bd16-cf3842fbe260" />

---
Q2: Total Transactions across each Region

```r
ggplot(ecom_mapped, aes(x = region)) +
  geom_bar(fill = "steelblue") +
  geom_text(
    stat = "count",
    aes(label = ..count..),
    position = position_dodge(width = 0.8),
    vjust = -0.3,
    size = 4,
    fontface = "bold" ) +
   scale_y_continuous(
    labels = function(x) paste0("₦", comma(x))) +
  labs(
    title = "Number of Transactions by Region",
    x = "Region",
    y = "Transaction Count") +
  theme_minimal()
```
<img width="1208" height="605" alt="Screenshot 2026-07-14 143432" src="https://github.com/user-attachments/assets/c290dbbb-11e9-4ab2-bf74-d4ec6094866a" />

---
 Q3:Average transaction across each Region
 
```r

ggplot(avg_amount, aes(x = region, y = amount, fill = region)) +
  geom_col(width = 0.7) +
  geom_text(
    aes(label = paste0("₦", comma(round(amount, 0)))),
    vjust = -0.4,
    size = 5,
    fontface = "bold") +
  labs(
    title = "Average Amount Spent per Transaction across each Region",
    x = "Region",
    y = "Average Amount (₦)") +
  scale_fill_brewer(palette = "Set1") +
  scale_y_continuous(
    labels = function(x) paste0("₦", comma(x))) +
  theme_minimal()
```
<img width="1213" height="629" alt="Screenshot 2026-07-13 163054" src="https://github.com/user-attachments/assets/7c875871-2e18-4231-8dd1-0b6addb7f578" />

---
---
Q4: If older customers buy more online than in-store, according to our VP of sales, how much did they spend?

```r
ggplot(ecom_mapped, aes(x = age_range, fill = purchase_channels)) +
  geom_bar(position = position_dodge(width = 0.9)) +
  geom_text(
    stat = "count",
    aes(label = comma(..count..)),
    position = position_dodge(width = 0.9),
    vjust = -0.5,
    size = 3 ) +
  scale_y_continuous(labels = comma, expand = expansion(mult = c(0, 0.1))) +
  labs(
    title = "Customer Age-Range by Purchase Model",
    x = "Age Range",
    y = "Number of Transactions",
    fill = "Purchase Mode") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```
<img width="1206" height="619" alt="Screenshot 2026-07-13 164311" src="https://github.com/user-attachments/assets/61d8fc93-5e08-43b4-9b76-332c5f61ea86" />

---
Q5: Does the age of a customer determine the amount spent on electronics?

```r
ggplot(df4, aes(x = age_range, y = amount)) +
  stat_summary(
    fun = sum,
    geom = "bar",
    fill = "darkgreen") +
  stat_summary(
    fun = sum,
    geom = "text",
    aes(label = comma(..y..)),  # adds commas to numbers on top
    vjust = -0.5,
    size = 3) +
  scale_y_continuous(labels = comma, expand = expansion(mult = c(0, 0.1))) +  # y-axis with commas
  labs(
    title = "Total Amount Spent by Age Range",
    x = "Age Range",
    y = "Total Amount") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```
<img width="1204" height="607" alt="Screenshot 2026-07-14 144342" src="https://github.com/user-attachments/assets/15991d9a-64a4-4cda-829e-c619ae6164dc" />



### Recommendation

✅Customer segmentation reveals a clear pattern:

 -Adults and middle-aged customers dominate purchases

 These groups contribute the highest revenue

🔥 These customers are likely: - Financially stable - More willing to invest in electronics

Business Implication:- Target ads toward working-class age groups - Offer premium bundles and upgrades



✅ Regional Performance: Where Revenue is Really Coming From;

At first glance, transaction volume suggests that the West region dominates customer activity. However, a deeper look reveals something more interesting: Some regions generate higher revenue with fewer transactions

This indicates *higher-value purchases per customer*

🔥 Not all customers are equal — some regions prioritize quality (high spend) over quantity (many transactions).

Business Implication: Blackwell should: - Focus premium product marketing in high-value regions - Avoid judging performance based only on transaction count'


## 👤 Author
*If you found this project useful or interesting, a ⭐ on the repo is appreciated!*




