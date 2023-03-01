---
jupyter:
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.11.1
  nbformat: 4
  nbformat_minor: 5
---

<div class="cell markdown">

## New York City Airbnb Listings Dataset Background

Airbnb is an online market place specializing in short-term homestays.
The company acts as a broker and charges a commission from each booking.

The dataset describes the listing activity and metrics for Airbnb New
York City, NY, United States of America.

The data contains information about the hosts, prices and the
geographical location, including both categorial and quantitative data.

The dataset is sourced from the website
(<http://insideairbnb.com/get-the-data/>) and is available for public.

Sample Size:41533

</div>

<div class="cell code" execution_count="4" scrolled="false">

``` python
df.info()
```

<div class="output stream stdout">

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 41533 entries, 0 to 41532
    Data columns (total 18 columns):
     #   Column                          Non-Null Count  Dtype  
    ---  ------                          --------------  -----  
     0   id                              41533 non-null  int64  
     1   name                            41520 non-null  object 
     2   host_id                         41533 non-null  int64  
     3   host_name                       41528 non-null  object 
     4   neighbourhood_group             41533 non-null  object 
     5   neighbourhood                   41533 non-null  object 
     6   latitude                        41533 non-null  float64
     7   longitude                       41533 non-null  float64
     8   room_type                       41533 non-null  object 
     9   price                           41533 non-null  int64  
     10  minimum_nights                  41533 non-null  int64  
     11  number_of_reviews               41533 non-null  int64  
     12  last_review                     32140 non-null  object 
     13  reviews_per_month               32140 non-null  float64
     14  calculated_host_listings_count  41533 non-null  int64  
     15  availability_365                41533 non-null  int64  
     16  number_of_reviews_ltm           41533 non-null  int64  
     17  license                         1 non-null      object 
    dtypes: float64(3), int64(8), object(7)
    memory usage: 5.7+ MB

</div>

</div>

<div class="cell code" execution_count="2">

``` python
import pandas as pd

df = pd.read_csv('listings.csv', low_memory=False)
```

</div>

<div class="cell markdown">

1.  The focus of the experiment is information regarding the listing
    prices in the Boroughs of New York City, namely Manhattan and
    Brooklyn. Specifically, "Does Manhattan have higher Airbnb prices
    compared to Brooklyn?". The question is pursued as a personal
    inquiry, as I plan to stay in New York City in Summer 2023.

2.  

-   The two focus groups in comparison are the Boroughs of Manhattan and
    Brooklyn, listed in the column "neighbourhood_group".
-   The measured response variable is the listed price per night,
    "price".
-   The response variable is quantative with type int64.

1.  The expected outcome of the experiment is for the price listings in
    Manhattan to be higher than in Brooklyn, as Manhattan hosts the city
    center of New York City, thereby making the general prices in the
    area higher. However, a note of concern that could point otherwise
    —that the price listings in Brookyln is higher than Manhattan— is
    that the type of property in Manhattan is predominantly rooms, while
    Brooklyn hosts a large number of residental houses. Still, it is
    hypothesized that the listing price in Manhattan on average will be
    higher than Brooklyn.

2.  The data is gathered from insideairbnb's public dataset library,
    using the latest dataset listing of 2022.
    (<http://insideairbnb.com/get-the-data/>).

3.  In the case that the experiment had unlimited resources, the most
    accurate way to gather our data in addition to acquiring the data
    using the public data, each one of the hosts of the listings could
    be contacted. This would eliminate the human error, the misinput
    prices, in our data. Additionally, by contacting the hosts, the most
    accurate prices would be obtained through the power of bargaining,
    as the listing price often reflects a different price compared to
    the one the host would be willing to accept, often higher than the
    real price.

</div>

<div class="cell code" execution_count="5">

``` python
df.head(5)
```

<div class="output execute_result" execution_count="5">

         id                                      name  host_id  host_name  \
    0  5136  Spacious Brooklyn Duplex, Patio + Garden     7378    Rebecca   
    1  5203        Cozy Clean Guest Room - Family Apt     7490  MaryEllen   
    2  5121                           BlissArtsSpace!     7356      Garon   
    3  5178          Large Furnished Room Near B'way　     8967   Shunichi   
    4  2595                     Skylit Midtown Castle     2845   Jennifer   

      neighbourhood_group       neighbourhood  latitude  longitude  \
    0            Brooklyn         Sunset Park  40.66265  -73.99454   
    1           Manhattan     Upper West Side  40.80380  -73.96751   
    2            Brooklyn  Bedford-Stuyvesant  40.68535  -73.95512   
    3           Manhattan             Midtown  40.76457  -73.98317   
    4           Manhattan             Midtown  40.75356  -73.98559   

             room_type  price  minimum_nights  number_of_reviews last_review  \
    0  Entire home/apt    275              21                  3  2022-08-10   
    1     Private room     75               2                118  2017-07-21   
    2     Private room     60              30                 50  2019-12-02   
    3     Private room     68               2                559  2022-11-20   
    4  Entire home/apt    175              30                 49  2022-06-21   

       reviews_per_month  calculated_host_listings_count  availability_365  \
    0               0.03                               1               267   
    1               0.73                               1                 0   
    2               0.30                               2               322   
    3               3.38                               1                79   
    4               0.31                               3               365   

       number_of_reviews_ltm license  
    0                      1     NaN  
    1                      0     NaN  
    2                      0     NaN  
    3                     50     NaN  
    4                      1     NaN  

</div>

</div>

<div class="cell markdown">

-   Group 1: (price \| neighbourhood_group = 'Brooklyn')

-   Group 2: (price \| neighbourhood_group = 'Manhattan')

</div>

<div class="cell code" execution_count="6">

``` python
# Group 1
(df[df["neighbourhood_group"] == "Brooklyn"]["price"]).head(5)
```

<div class="output execute_result" execution_count="6">

    0    275
    2     60
    6    124
    7     68
    8    220
    Name: price, dtype: int64

</div>

</div>

<div class="cell code" execution_count="7">

``` python
# Group 2
(df[df["neighbourhood_group"] == "Manhattan"]["price"]).head(5)
```

<div class="output execute_result" execution_count="7">

    1     75
    3     68
    4    175
    5     65
    9     62
    Name: price, dtype: int64

</div>

</div>

<div class="cell markdown">

## Data Exploration

</div>

<div class="cell code" execution_count="9">

``` python
import seaborn as sns
import matplotlib.pyplot as plt
sns.set_theme(style="whitegrid")
```

</div>

<div class="cell code" execution_count="9">

``` python
df.columns
```

<div class="output execute_result" execution_count="9">

    Index(['id', 'name', 'host_id', 'host_name', 'neighbourhood_group',
           'neighbourhood', 'latitude', 'longitude', 'room_type', 'price',
           'minimum_nights', 'number_of_reviews', 'last_review',
           'reviews_per_month', 'calculated_host_listings_count',
           'availability_365', 'number_of_reviews_ltm', 'license'],
          dtype='object')

</div>

</div>

<div class="cell code" execution_count="10">

``` python
df = df[['id', 'name', 'host_id', 'host_name', 'neighbourhood_group',
       'neighbourhood', 'latitude', 'longitude', 'room_type', 'price',
       'minimum_nights', 'number_of_reviews', 
       #'last_review',
       #'reviews_per_month', 
       #'calculated_host_listings_count',
       #'availability_365', 'number_of_reviews_ltm', 'license'
        ]]
```

</div>

<div class="cell code" execution_count="11">

``` python
print("Total number of listings")

print(len(df))

print("Number of listings in Brookyln")

print(len(df[df["neighbourhood_group"] == "Brooklyn"]))

print("Number of listings in Manhattan")

print(len(df[df["neighbourhood_group"] == "Manhattan"]))
```

<div class="output stream stdout">

    Total number of listings
    41533
    Number of listings in Brookyln
    15688
    Number of listings in Manhattan
    17334

</div>

</div>

<div class="cell code" execution_count="12">

``` python
sns.histplot(data=df, x='neighbourhood_group', bins='auto')
plt.show()
```

<div class="output display_data">

![](32257d148477dc9bb4d3a14ed7af5596c9312579.png)

</div>

</div>

<div class="cell code" execution_count="13">

``` python
print('Number of Airbnb Listing by Borough')


df.groupby('neighbourhood_group')['id'].count()
```

<div class="output stream stdout">

    Number of Airbnb Listing by Borough

</div>

<div class="output execute_result" execution_count="13">

    neighbourhood_group
    Bronx             1587
    Brooklyn         15688
    Manhattan        17334
    Queens            6519
    Staten Island      405
    Name: id, dtype: int64

</div>

</div>

<div class="cell code" execution_count="14" scrolled="true">

``` python
print('Highest Airbnb Prices by Borough ')


df.groupby('neighbourhood_group')['price'].max()
```

<div class="output stream stdout">

    Highest Airbnb Prices by Borough 

</div>

<div class="output execute_result" execution_count="14">

    neighbourhood_group
    Bronx            95110
    Brooklyn         98159
    Manhattan        19750
    Queens           10000
    Staten Island    65115
    Name: price, dtype: int64

</div>

</div>

<div class="cell markdown">

#### Alternatively

</div>

<div class="cell code" execution_count="15">

``` python
print("Lowest Airbnb Price in Brooklyn")
print((df[df["neighbourhood_group"] == "Brooklyn"]["price"]).min())
print("Highest Airbnb Price in Brooklyn")
print((df[df["neighbourhood_group"] == "Brooklyn"]["price"]).max())



print("Lowest Airbnb Price in Manhattan")
print((df[df["neighbourhood_group"] == "Manhattan"]["price"]).min())
print("Highest Airbnb Price in Manhattan")
print((df[df["neighbourhood_group"] == "Manhattan"]["price"]).max())
```

<div class="output stream stdout">

    Lowest Airbnb Price in Brooklyn
    0
    Highest Airbnb Price in Brooklyn
    98159
    Lowest Airbnb Price in Manhattan
    0
    Highest Airbnb Price in Manhattan
    19750

</div>

</div>

<div class="cell code" execution_count="3" scrolled="true">

``` python
print('Airbnb Price Fluctuation by Borough ')


df.groupby('neighbourhood_group')['price'].std()
```

<div class="output stream stdout">

    Airbnb Price Fluctuation by Borough 

</div>

<div class="output execute_result" execution_count="3">

    neighbourhood_group
    Bronx            2399.694451
    Brooklyn         1007.186208
    Manhattan         533.557935
    Queens            314.116663
    Staten Island    3257.698551
    Name: price, dtype: float64

</div>

</div>

<div class="cell markdown">

### Question 4 - Assumption

</div>

<div class="cell code" execution_count="16">

``` python
df['room_type'].values
```

<div class="output execute_result" execution_count="16">

    array(['Entire home/apt', 'Private room', 'Private room', ...,
           'Entire home/apt', 'Entire home/apt', 'Entire home/apt'],
          dtype=object)

</div>

</div>

<div class="cell markdown">

#### Room Type Percentages by Borough

</div>

<div class="cell code" execution_count="63">

``` python
print("Percentage of Rooms vs Houses in Brooklyn")
print(len(df[(df['neighbourhood_group']== "Brooklyn") & (df["room_type"] == 'Private room')])/ len(df[df["neighbourhood_group"] == "Brooklyn"])*100)
print(len(df[(df['neighbourhood_group']== "Brooklyn") & (df["room_type"] == 'Entire home/apt')])/len(df[df["neighbourhood_group"] == "Brooklyn"])*100)

print("Number of Rooms vs Houses in Manhattan")
print(len(df[(df['neighbourhood_group']== "Manhattan") & (df["room_type"] == 'Private room')])/ len(df[df["neighbourhood_group"] == "Manhattan"])*100)
print(len(df[(df['neighbourhood_group']== "Manhattan") & (df["room_type"] == 'Entire home/apt')])/ len(df[df["neighbourhood_group"] == "Manhattan"])*100)
```

<div class="output stream stdout">

    Percentage of Rooms vs Houses in Brooklyn
    43.58745537990821
    55.30341662417134
    Number of Rooms vs Houses in Manhattan
    34.50444213684089
    63.280258451598016

</div>

</div>

<div class="cell code" execution_count="18">

``` python
df_hypothesis = df['room_type'].isin(['Entire home/apt', 'Private room'])
df[df_hypothesis]
```

<div class="output execute_result" execution_count="18">

                           id                                               name  \
    0                    5136           Spacious Brooklyn Duplex, Patio + Garden   
    1                    5203                 Cozy Clean Guest Room - Family Apt   
    2                    5121                                    BlissArtsSpace!   
    3                    5178                   Large Furnished Room Near B'way　   
    4                    2595                              Skylit Midtown Castle   
    ...                   ...                                                ...   
    41528  771962449581256963                                Romántico y natural   
    41529  771967712456918474                          Sunset Park Studio Sublet   
    41530  771971759808918693     9B5B Townhouse w/ Elevator &  Private Entrance   
    41531  771971822371481471  Huge 9B5B Townhouse w Elevator &  Private Entr...   
    41532  771975190766692224           Lovely  Big Studio for Rental Bronx City   

             host_id      host_name neighbourhood_group       neighbourhood  \
    0           7378        Rebecca            Brooklyn         Sunset Park   
    1           7490      MaryEllen           Manhattan     Upper West Side   
    2           7356          Garon            Brooklyn  Bedford-Stuyvesant   
    3           8967       Shunichi           Manhattan             Midtown   
    4           2845       Jennifer           Manhattan             Midtown   
    ...          ...            ...                 ...                 ...   
    41528  421601513    Juan Carlos           Manhattan  Washington Heights   
    41529     326495  Laura Adriana            Brooklyn         Sunset Park   
    41530  316920152        Allison           Manhattan         Murray Hill   
    41531  484979380        Natasha           Manhattan         Murray Hill   
    41532  426540801           Abul               Bronx           Unionport   

            latitude  longitude        room_type  price  minimum_nights  \
    0      40.662650 -73.994540  Entire home/apt    275              21   
    1      40.803800 -73.967510     Private room     75               2   
    2      40.685350 -73.955120     Private room     60              30   
    3      40.764570 -73.983170     Private room     68               2   
    4      40.753560 -73.985590  Entire home/apt    175              30   
    ...          ...        ...              ...    ...             ...   
    41528  40.847271 -73.943419     Private room     80               5   
    41529  40.638329 -74.016710  Entire home/apt     42              30   
    41530  40.746902 -73.978260  Entire home/apt   3888               2   
    41531  40.749596 -73.980798  Entire home/apt   3888               2   
    41532  40.832824 -73.852371  Entire home/apt     80               3   

           number_of_reviews  
    0                      3  
    1                    118  
    2                     50  
    3                    559  
    4                     49  
    ...                  ...  
    41528                  0  
    41529                  0  
    41530                  0  
    41531                  0  
    41532                  0  

    [40813 rows x 12 columns]

</div>

</div>

<div class="cell code" execution_count="19" scrolled="false">

``` python
sns.displot(
    df[df_hypothesis], x="room_type", col="neighbourhood_group",
    height=3, facet_kws=dict(margin_titles=False),
)
```

<div class="output execute_result" execution_count="19">

    <seaborn.axisgrid.FacetGrid at 0x11f30b610>

</div>

<div class="output display_data">

![](cbe847c2eab528a5506c86a6cf650525ffccd3df.png)

</div>

</div>

<div class="cell markdown">

### Observation

Accordingly, our assumption that Brooklyn has more houses than rooms
compared to Manhattan is false.

</div>

<div class="cell markdown">

## Target Statistics

</div>

<div class="cell code" execution_count="20">

``` python
print('The mean, median and the standart deviation of listing prices by Borough')


print(df.groupby('neighbourhood_group')['price'].agg(['mean', 'median', 'std']))
```

<div class="output stream stdout">

    The mean, median and the standart deviation of listing prices by Borough
                               mean  median          std
    neighbourhood_group                                 
    Bronx                180.816635    89.0  2399.694451
    Brooklyn             171.926441   119.0  1007.186208
    Manhattan            301.211665   175.0   533.557935
    Queens               135.656389    93.0   314.116663
    Staten Island        320.343210   100.0  3257.698551

</div>

</div>

<div class="cell code" execution_count="51">

``` python
sns.violinplot(data=df, x='price', y='neighbourhood_group')
```

<div class="output execute_result" execution_count="51">

    <AxesSubplot: xlabel='price', ylabel='neighbourhood_group'>

</div>

<div class="output display_data">

![](8715cb3b345df321b1705acca751aa7db57dc2a7.png)

</div>

</div>

<div class="cell markdown">

#### Removing Outliers

</div>

<div class="cell code" execution_count="22">

``` python
sns.boxplot(data=df, x='price', y= 'neighbourhood_group', showfliers =False)
```

<div class="output execute_result" execution_count="22">

    <AxesSubplot: xlabel='price', ylabel='neighbourhood_group'>

</div>

<div class="output display_data">

![](25a1be557bd2a9556b68ea4c67c66e5c63ba1ba1.png)

</div>

</div>

<div class="cell code" execution_count="48">

``` python
sns.boxplot(data=df, x='neighbourhood_group', y='price', hue='room_type')
plt.ylim(0,1000)
plt.show()
```

<div class="output display_data">

![](f99c81df0636945b632c0cbd614d41c1933e024f.png)

</div>

</div>

<div class="cell markdown">

### Target Observation

The mean and meadian Airbnb listing price in Manhattan is higher than
that of Brooklyn. This confirms our hypothesis that the listing price in
Manhattan on average will be higher than Brooklyn. Thus, answering the
target question "Does Manhattan have higher Airbnb prices than
Brooklyn?".

</div>

<div class="cell markdown">

## Interesting Statistics

</div>

<div class="cell code" execution_count="42">

``` python
df['neighbourhood'].value_counts() \
    .head(10) \
    .plot(kind = 'bar', color = 'red', title = 'Neighbourhoods with the Most Airbnb Listings')
```

<div class="output execute_result" execution_count="42">

    <AxesSubplot: title={'center': 'Neighbourhoods with the Most Airbnb Listings'}>

</div>

<div class="output display_data">

![](a5e992f92c6651b043f0dbb79f4cd6bf240b4c38.png)

</div>

</div>

<div class="cell markdown">

#### Relationship Between Number of Reviews and Price

</div>

<div class="cell code" execution_count="39">

``` python
sns.scatterplot(data=df, x='number_of_reviews', y='price',)
plt.ylim(0,2000) # Forceful removal of outliers, clearer to see trends
plt.xlim (0,1000)
plt.show()
```

<div class="output display_data">

![](365e8e0adf3faa10ace6e43d322b1b38aa7c8245.png)

</div>

</div>

<div class="cell code" execution_count="64">

``` python
sns.lineplot(data=df, x="number_of_reviews", y="price")
plt.ylim(0,2000)
plt.xlim (0,1000)
plt.show()
```

<div class="output display_data">

![](ba741f07921ad3374e04a0bddd5c4842e2377303.png)

</div>

</div>

<div class="cell markdown">

#### Observation:

There is no apparent correlation between the number of reviews and
price, as the data is too cluttered on both axis.

</div>

<div class="cell code" execution_count="36">

``` python
df.groupby('room_type')['price'].mean() \
    .plot(kind = 'bar', color ='orange', title= 'Airbnb Average Listing Prices by Room Type')
```

<div class="output execute_result" execution_count="36">

    <AxesSubplot: title={'center': 'Airbnb Average Listing Prices by Room Type'}, xlabel='room_type'>

</div>

<div class="output display_data">

![](5d9b93fa4fa025dff14b798a0c8194595022a028.png)

</div>

</div>

<div class="cell code" execution_count="22">

``` python
df_neighbourhoodprices = df.groupby('neighbourhood')['price'].mean() 
df_neighbourhoodprices
```

<div class="output execute_result" execution_count="22">

    neighbourhood
    Allerton           108.872340
    Arden Heights      115.400000
    Arrochar           132.117647
    Arverne            188.698276
    Astoria            124.817056
                          ...    
    Windsor Terrace    169.784483
    Woodhaven           93.557895
    Woodlawn           134.909091
    Woodrow            109.000000
    Woodside            85.230769
    Name: price, Length: 223, dtype: float64

</div>

</div>

<div class="cell code" execution_count="34" scrolled="false">

``` python
print('Most Expensive Neighbourhoods for Airbnb in New York City')
df_neighbourhoodprices.sort_values(ascending=False).head(30) \
    .plot(kind = 'bar', title = 'Most Expensive Neighbourhoods for Airbnb in New York City')
```

<div class="output stream stdout">

    Most Expensive Neighbourhoods for Airbnb in New York City

</div>

<div class="output execute_result" execution_count="34">

    <AxesSubplot: title={'center': 'Most Expensive Neighbourhoods for Airbnb in New York City'}, xlabel='neighbourhood'>

</div>

<div class="output display_data">

![](4ecb2288809d3444d78a2ca643c81caef41f9723.png)

</div>

</div>

<div class="cell code" execution_count="62">

``` python
df_hosts = df.groupby('host_name')['price'].mean() 

df_hosts.value_counts().head(20) \
    .plot(kind = 'bar', title = 'Most Common Prices for Airbnbs in New York City')
```

<div class="output execute_result" execution_count="62">

    <AxesSubplot: title={'center': 'Most Common Prices for Airbnbs in New York City'}>

</div>

<div class="output display_data">

![](e8862491d09a4425b88c40fefe440166190b07e1.png)

</div>

</div>

<div class="cell code" execution_count="61">

``` python
df['host_name'].value_counts().head(20)\
    .plot(kind = 'bar', title = 'Most Common Host Names for Airbnbs in New York City')
```

<div class="output execute_result" execution_count="61">

    <AxesSubplot: title={'center': 'Most Common Host Names for Airbnbs in New York City'}>

</div>

<div class="output display_data">

![](19ae93368decab9d7feba9d1963284849dd35ac8.png)

</div>

</div>

<div class="cell markdown">

### Geography Through Data

</div>

<div class="cell code" execution_count="71">

``` python
sns.scatterplot(data=df, x='latitude', y='price')
plt.ylim(0,2000)
```

<div class="output execute_result" execution_count="71">

    (0.0, 2000.0)

</div>

<div class="output display_data">

![](3c120ce8128695d289eea2e72be284dc86f0e99a.png)

</div>

</div>

<div class="cell markdown">

#### By looking at the latitude by price graph, we can clearly see where the city center, thereby where Manhattan lies.

</div>

<div class="cell code" execution_count="72">

``` python
sns.scatterplot(data=df, x='longitude', y='price')
plt.ylim(0,2000)
```

<div class="output execute_result" execution_count="72">

    (0.0, 2000.0)

</div>

<div class="output display_data">

![](186d93eb55b9ebf670680eaa8b62fb1949d970f1.png)

</div>

</div>

<div class="cell markdown">

#### Similarly, when the longitude is compared with the geographical map of New York City, the middle of the graph where 3 different boroughs lie, can be seen covering nearly all the price listings of Airbnbs.

</div>

<div class="cell markdown">

![](63830769.jpg)

</div>

<div class="cell code">

``` python
```

</div>
