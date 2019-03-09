
# Heroes of Pymoli Consumer Data Analysis

* The majority of purchases are made by male players (84%). However, even though female players spent much less total, their average total purchase (4.47 dollars) is higher compared to males (4.07 dollars). 


* Highest proportion of players falls in the 20-24 age range (45%), they also have the highest total purchase value. However, the highest average purchase per personal falls in the age range of 35-39 and, interestingly, in <10 years olds.


* “Oathbreaker” tops the most profitable and the most popular item, with 12 purchase counts and a total purchase value of $50.76. “Fiery Glass Crusader” and “Nirvana” are also on the list of top 5 most profitable and most popular items. On the list of the 5 most popular items, “Pursuit, Cudgel of Necromancy” is not one of the 5 most profitable since its listing price is much lower than the others on the top list.  


```python
import pandas as pd 
filepath = "Resources/purchase_data.csv"
pymoli_df = pd.read_csv(filepath)

pymoli_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>



## Player Count
- Display total number of players


```python
player_count = len(pymoli_df["SN"].unique())
playercount = pd.DataFrame({"Total Players": [player_count]})
playercount
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)
- Run basic calculations to obtain number of unique items, average price, etc.
- Create a summary data frame to hold the results 
- Display the summary data frame


```python
unique = len(pymoli_df["Item ID"].unique()) 
average = round(pymoli_df["Price"].mean(),2) 
purchase = pymoli_df["Purchase ID"].count() 
revenue = pymoli_df["Price"].sum()

PurchSummary = pd.DataFrame({"Number of Unique Items":[unique],
                             "Average Purchase Price":[average],
                             "Total Number of Purchases":[purchase],
                             "Total Revenue":[revenue]})
#Formatting
PurchSummary["Average Purchase Price"] = PurchSummary["Average Purchase Price"].map("${:.2f}".format)
PurchSummary["Total Revenue"]= PurchSummary["Total Revenue"].map("${:.2f}".format)
PurchSummary
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Purchase Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics
- Percentage and Count of Male Players
- Percentage and Count of Female Players
- Percentage and Count of Other / Non-Disclosed


```python
#Create df for unique players
nodupe_players_df = pymoli_df.drop_duplicates(["SN"]) 

#Get total gender count, then rename the column
genderdemo_df = pd.DataFrame(nodupe_players_df["Gender"].value_counts()) 
genderdemo_df = genderdemo_df.rename(columns = {"Gender": "Total Count"}) 

#Adding a column for % of players
genderdemo_df["Percentage of Players"] = (genderdemo_df["Total Count"] / player_count)*100

#Formatting
genderdemo_df["Percentage of Players"]= genderdemo_df["Percentage of Players"].map("{:.2f}%".format)

genderdemo_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>484</td>
      <td>84.03%</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>81</td>
      <td>14.06%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.91%</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)
- Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender
- Create a summary data frame to hold the results
- Display the summary data frame


```python
#Purchase count
purchasecount = pymoli_df.groupby("Gender")["Purchase ID"].count()

#Create Dataframe 
purchase_df = pd.DataFrame(purchasecount)
purchase_df = purchase_df.rename(columns={'Purchase ID':"Purchase Count"})

#Calc average purchase price, then add to dataframe
averageprice = pymoli_df.groupby("Gender")["Price"].mean()
purchase_df["Avg Purchase Price"] = averageprice

#Calc total purchase value
totalprice = pymoli_df.groupby("Gender")["Price"].sum()
purchase_df["Total Purchase Value"] = totalprice

#Avg total purchase per person
avgtot = totalprice/nodupe_players_df["Gender"].value_counts()
purchase_df["Avg Total Purchase/Person"] = avgtot

#Formatting
purchase_df["Avg Purchase Price"]=purchase_df["Avg Purchase Price"].map("${:.2f}".format)
purchase_df["Total Purchase Value"]=purchase_df["Total Purchase Value"].map("${:.2f}".format)
purchase_df["Avg Total Purchase/Person"]=purchase_df["Avg Total Purchase/Person"].map("${:.2f}".format)

purchase_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Avg Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Total Purchase/Person</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics
- Establish bins for ages
- Categorize the existing players using the age bins. Hint: use pd.cut()
- Calculate the numbers and percentages by age group
- Create a summary data frame to hold the results
- Optional: round the percentage column to two decimal points
- Display Age Demographics Table


```python
#Establish Bins
bins = [0,9,14,19,24,29,34,39,100]
group_names = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"]
    
pymoli_df['Age Range'] = pd.cut(pymoli_df["Age"], bins, labels= group_names)
```


```python
#Remove dupliate names
nodupe_players_df = pymoli_df.drop_duplicates(["SN"]) 

#Groupby Age Categories
agecount = nodupe_players_df.groupby("Age Range")["SN"].count()
agepercent =(agecount/player_count*100).map("{:.2f}%".format)

agedemo_df = pd.DataFrame({"Total Count": agecount,
                           "Percentage of Players": agepercent})
agedemo_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95%</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58%</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38%</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08%</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)


```python
#Purchase count
count = pymoli_df.groupby("Age Range")["Purchase ID"].count()

#Avg Purchase Price
avgprice = (pymoli_df.groupby("Age Range")["Price"].mean()).map("${:.2f}".format)

#Total Purchase Value
totalpurchase = pymoli_df.groupby("Age Range")["Price"].sum()
totalpurchase2 = totalpurchase.map("${:.2f}".format)

#Average Purchase Total per person
avgtotal = (totalpurchase/agecount).map("${:.2f}".format)

#Put into DataFrame
purchase_age_df = pd.DataFrame({"Purchase Count":count,
                                "Avg Purchase Price":avgprice,
                               "Total Purchase Value":totalpurchase2,
                               "Avg Total Purchase/Person": avgtotal})

purchase_age_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Avg Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Total Purchase/Person</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

Identify the the top 5 spenders in the game by total purchase value, then list (in a table): 
    - SN
    - Purchase Count
    - Average Purchase Price
    - Total Purchase Value


```python
#Purchase count, avg price, total value
purchcount = pymoli_df.groupby("SN")["Purchase ID"].count()
purchprice = pymoli_df.groupby("SN")["Price"].mean()
totalvalue = (purchcount*purchprice)

#Format
purchprice= purchprice.map("${:.2f}".format)

#Put in Dataframe
toppymoli_df = pd.DataFrame({"Purchase Count": purchcount,
                             "Average Purchase Price": purchprice,
                             "Total Purchase Value": totalvalue})
#Sort
toppymoli_df = toppymoli_df.sort_values("Total Purchase Value", ascending=False)

#Formatting
toppymoli_df["Total Purchase Value"]= toppymoli_df["Total Purchase Value"].map("${:.2f}".format)

toppymoli_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items
Identify the 5 most popular items by purchase count, then list (in a table):
 - Item ID
 - Item Name
 - Purchase Count
 - Item Price
 - Total Purchase Value


```python
#Calculate purchase count, item price, total value
purchase = pymoli_df.groupby(["Item ID", "Item Name"])

purchasecount = purchase["Item ID"].count() #Number of rows in ea group
itemprice = purchase["Price"].sum()/purchase["Item ID"].count()
totalvalue = purchase["Price"].sum() #Sum of price in ea group

#Add to Dataframe
item_df = pd.DataFrame({"Purchase Count": purchasecount,
                          "Item Price":itemprice,
                        "Total Purchase Value": totalvalue})
#Sort                         
popular_df = item_df.sort_values('Purchase Count', ascending=False)


#Formatting
popular_df["Item Price"]= popular_df["Item Price"].map("${:.2f}".format)
popular_df["Total Purchase Value"]= popular_df["Total Purchase Value"].map("${:.2f}".format)

popular_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>8</td>
      <td>$1.02</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items
Identify the 5 most profitable items by total purchase value, then list (in a table):
- Item ID
- Item Name
- Purchase Count
- Item Price
- Total Purchase Value


```python
#Sort
profitable_df = item_df.sort_values('Total Purchase Value', ascending=False)
profitable_df.head()

#Formatting
profitable_df["Item Price"]=profitable_df["Item Price"].map("${:.2f}".format)
profitable_df["Total Purchase Value"]=profitable_df["Total Purchase Value"].map("${:.2f}".format)

profitable_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>92</th>
      <th>Final Critic</th>
      <td>8</td>
      <td>$4.88</td>
      <td>$39.04</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


