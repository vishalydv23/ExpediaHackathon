# EXPEDIA GROUP HACKATHON
### SOCIALLY RESPONSIBLE AND INCLUSIVE TOURISM


***The code in this repo was mostly written in 2 days of Hackathon.*** 

**Problem:** 
we need you to identify clusters of accommodation that bring the most positive impact to the community, allowing a wider range of actors to participate in travel and tourism as consumers and/or providers.
**Solution:**
The approach to solve this problem was
* Find out all possible indicators that can impact a community due to tourism from data. Involve even the weaker indicators which will be harder to involve if using the normal approach mentioned above. And let the algorithm pick the stronger ones for you. 	
  * Group these indicators into categories like
    * Economic impact
    * Cultural impact
    * Environmental impact
* Cluster each category differently on the same data of counties along with high tourism per population cluster.
* Do the county morphology of categories cluster. 
* Find the overlapping counties between clusters of high tourism and the indicators of categories. Demonstrating which counties have high value for indicators and also have high tourism per population. 

Going through each step in more details. 
* Find out all possible indicators that can impact a community due to tourism from data. Involve even the weaker indicators which will be harder to involve if using the normal approach mentioned above. And let the algorithm pick the stronger ones for you.
  
  The challenge was to pick all possible indicators that can be useful. For that, I started with the official data source provided to us. And used Panda’s Profiling to get a detailed insight of the data. Panda’s profiling cut down hours when it comes to EDA. Which of course is most time talking steps while solving a data science problem. Here are 4 of the screenshots of some results of the profiling report for the main Expedia dataset provided. 

  ![alt text](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/pandasProfiling1.JPG "Pandas Profiling Example")
  ![alt text](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/pandasProfiling2.JPG "Pandas Profiling Example")
  ![alt text](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/pandasProfiling3.JPG "Pandas Profiling Example")
  ![alt text](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/pandasProfiling4.JPG "Pandas Profiling Example")

  This helped to quickly make decisions and find out identifiers.
  
  Now, we created 4 categories under which data indicators were to be collected. These indicators were designed taking inspiration from European Tourism Indicators System (ETIS) for sustainable destination management [Link](https://ec.europa.eu/growth/sectors/tourism/offer/sustainable/indicators_en)
  Also, as mentioned previously, the approach we are taking to solve the problem is kind of like reverse engineering. We take all senseful indicators and then rely on the algorithm to tell us which of the indicators has more impact on the community. Therefore some of the indicators picked here under each category are more obvious than others.
  
  Following are the Categories and indicators with data source
  1. Tourism rate per population - *population here is taken from US Census data provided for 2018*.
    a. Lodging Inventory Bucket Non-Vacational Rental - *Expedia dataset*
    b. Lodging Inventory Bucket Vacation Rental - *Expedia dataset*
    c. Lodging Inventory Bucket Vacation Rental To Population - *Expedia dataset*
    d. Air inbound popularity bucket Ratio To Population - *Expedia dataset*
    e. People Employed in Arts, Entertainment, Recreational, Accomodation and Food ratio to total population. - [*Open Census Data, SafeGraph*](https://www.safegraph.com/open-census-data)
  2. Economic Social Impact
    a.  Median HouseHold Income - [*Open Census Data, SafeGraph*](https://www.safegraph.com/open-census-data)
    b.  Unemployment Rate - [*Open Census Data, SafeGraph*](https://www.safegraph.com/open-census-data)
    c.  Vacant Housing Units Ratio to total houses - [*Open Census Data, SafeGraph*](https://www.safegraph.com/open-census-data)
    d. Families Under Poverty ratio to total families - [*Open Census Data, SafeGraph*](https://www.safegraph.com/open-census-data)
    e. Total Employed In Own Small Business Ratio to population - [*Open Census Data, SafeGraph*](https://www.safegraph.com/open-census-data)
  3. Cultural social impact
    a.  Minority Population Ratio to Population - *US Census data provided for 2018*
    b.  Structure Built Year Before 1939 Ratio to total structures in the county - [*Open Census Data, SafeGraph*](https://www.safegraph.com/open-census-data)
    c.  Customer Satisfaction Average Review Rating - *Expedia dataset*
    d.  Customer Satisfaction Average Star Rating - *Expedia dataset*
  4.  Environmental Social Impact
    a.  Water Usage per population - [*USGS, Link*](https://pubs.er.usgs.gov/publication/cir1441)
    b.  Number of Bicycle Users per all means of transportation - [*Open Census Data, SafeGraph*](https://www.safegraph.com/open-census-data)
    c.  Air Qulaity PM : 2.5 PPM - [*United States Environmental Protection Agency*](https://www.epa.gov/air-trends/air-quality-cities-and-counties)

  Most of these attributes didn’t have missing values. The once which had some, didn’t contain normal data distribution, so we used a **KNN algorithm** to fill out these missing values. 
  *Note: Some attributes were divided by population of counties from the 2016 census and some with the 2018 census just because of the missing values. We assumed that the population didn’t change much between this time.*
  
* Cluster each category differently on the same data of counties along with high tourism per population cluster.
  For each of the categories mentioned above we applied the clustering algorithm separately. Starting with the number of tourists per population clustering. Following algorithms were tried.
    1.  **KMeans clustering**
    2.  **Agglomerative Clustering**
    3.  **DBSCAN (Density-Based Clustering of Applications with Noise) clustering**
    4.  **Mean Shift Clustering**
    5.  **Gaussian Mixture with Expectations Maximization (EM) Clustering**
  
  Good old K means clustering gave the best results. This is the silhouette score of clustering for tourism per population. 
  ![silhouette](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/silhouette.JPG "silhouette score of clustering for tourism per population")

  As the data in the other categories is very similar to tourism per population categories and due to less time to implement it we assumed that K means will work best for other categories as well. 
  
  To see whether we have achieved what we were trying to do, we checked the first county of the cluster which should have a high value of tourism per population ratio. It was Denali Borough in Alaska. And Denali National Park is Alaska’s most popular land attraction. Proving that we were on the right track.

*  Do the county morphology of categories cluster
  This step in the process was to recognize which of the indicators were prominent in each cluster of different categories. We used Radar plot to achieve this. This is the **radar plot** of the culture category. It was divided into 4 clusters and we can see that cluster 1 and cluster 2 indicate a strong influence of Structure built before 1939 and minority to population ratio in deciding the cluster.   
  ![radarPlot](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/radarPlot.JPG "radar plot of the culture category")

*  Find the overlapping counties between clusters of high tourism and the indicators of categories. Demonstrating which counties have high value for indicators and also have high tourism per population.
  Finally as we have all the information ready with us. We can start to play and overlap the clusters of different categories. But before we go and Talk about how this technique helps to cherry pick counties along with their clusters which have a positive impact on society. Let’s take a look at the visualization that we achieved. 
  
    We used **Apache superset for visualization**. Once we had all the required data in a dataframe we wrote a mySQL Script that loads the data into the tool and created a complete dashboard to interact with the data. 

    These are the screenshots of  some visualizations from the dashboard.
    
    ![AltText](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/visualization1.JPG "Result Visualization")
    ![AltText](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/visualization2.JPG "Result Visualization")
    ![AltText](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/visualization3.JPG "Result Visualization")
    ![AltText](https://github.com/vishalydv23/ExpediaHackathon/blob/master/data/ReadmeStore/visualization4.JPG "Result Visualization")

    Now let's take an example to see if this process worked. 

    We can see from the radar plot that the unemployment rate of  cluster 1 in category economics is really low and tourism per population for cluster 1 in the tourism category is high. There are 25 overlapping counties in these two clusters. The 1st datapoint of these 25 values is county haines borough alaska. And we can see from this [**report**](https://www.hainesalaska.gov/sites/default/files/fileattachments/tourism/page/1424/haines_winter_visitor_study_revised.pdf) that tourism is bringing down unemployment in this county significantly during winters. 