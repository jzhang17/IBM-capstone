# IBM-capstone
## Introduction 

The city of Philadelphia has the highest rate of gentrification and income gap across the United States. In this project, I want to explore the correlation between neighborhoods median income compares to the functions of different neighborhoods. The correlation can help business stakeholders to make decisions on locations and the business and growth opportunities in different neighborhood clusters. For example, what business opportunities exist in high income neighborhoods compares to low income neighborhoods.

## Data

The Foursquare location data is used to explore the functions of neighborhoods. The income data is scraped from websites with publicized data. The google Geocoding API is used to visualize the Foursquare data on a folium map. The neighborhoods are segmented into clusters and use a bar graph to verify the hypothesis. 

Geo-coding Zip code:
```python
latitude = []
longitude = []

for add_LL in zip_codes:
    url = 'https://maps.googleapis.com/maps/api/geocode/json?key={}&address={}'.format(API_key, add_LL)
    geographical_data = response['results'][0]['geometry']['location'] # get geographical coordinates
    response = requests.get(url).json()
    latitudes = geographical_data['lat']
    longitudes = geographical_data['lng']
    latitude.append(latitudes)
    longitude.append(longitudes)
    print(url)
    print(latitudes)
```
![Image of Clusters](https://static1.squarespace.com/static/5a273c2db7411c04ea67f2e5/t/5b6ded7aaa4a997d01a78224/1533930884023/download+%281%29.png?format=2500w)

## Methodology

The neighborhoods are segmented into 5 clusters with random cluster size but each cluster serves the same function with similar residential and commercial establishment. Because the clusters' sizes are random, three out of five clusters has very small cluster size but the mean varies greatly from each other, yet with small standard diviation from the mean. The sample size for the two larger clusters is smaller than 30 so 2-sample-t-test is performed as inferential statistical testing to verify the hypothesis. The test is set to two-tailed test because it is unclear if one cluster's mean is biger than the other.  

Inferential statistical testing:
```python
from scipy import stats

## Calculate the Standard Deviation
#Calculate the variance to get the standard deviation

#For unbiased max likelihood estimate we have to divide the var by N-1, and therefore the parameter ddof = 1
var_0 = grouped_transposed[0].var(ddof=1)
var_2 = grouped_transposed[2].var(ddof=1)

#std deviation
s = np.sqrt((var_0 + var_2)/2)

t = (grouped_transposed[0].mean() - grouped_transposed[2].mean())/(s*np.sqrt(2/len(grouped_transposed[0])))

f_test = 2*len(grouped_transposed[0]) - 2

#p-value after comparison with the t 
p = 1 - stats.t.cdf(t,df=df_test)


print("t = " + str(t))
print("p = " + str(2*p))
#Note that we multiply the p value by 2 because its a two tail t-test
```

Output:
```
t = 2.19200477138
p = 0.031019029026
```
![Image of Distribution](https://static1.squarespace.com/static/5a273c2db7411c04ea67f2e5/t/5b6deda3cd83661ba3d587bb/1533930920814/?format=750w)

## Results

Because the p-value is less than 0.05, we can conclude that cluster 0 on average has higher median income than cluster 2 with 95% of confidence intercal. Therefore, the hypothesis of median income has effect on neighborhood functions is verified. 

## Discussion

With higher income and commercial functions of the neighborhoods in cluster 0. The recommendation can be made that cluster 0 is more suitable for high-end commercial establishment, while residential bussiness establishments like restaurants should enter cluster 2 for it's higher growth potential. 

## Conclusion 

Gentrification is one of the most significant social phenomenon in the city of Philadelphia because it directly affects city development and wealth distribution, and the issue sometimes even implicates racial integration in the modern society. With the help of modern computing technology and publicly available data, we can get insight from the phenomenon to discover the correlation between wealth distribution and neighborhoods’ functions. With this correlation we can get business insights city development. 

## [Cluster Visualization](http://htmlpreview.github.io/?https://github.com/jzhang17/IBM-capstone/blob/master/cluster.html)
