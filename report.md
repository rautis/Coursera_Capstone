# Open a new restaurant in the city of Helsinki, Finland

## Business problem

Opening a new restaurant is a challenging task. Not only do you need to serve quality food with suitable pricing you need to select the correct location for the restaurant. Restaurant concept defines the type of food the restaurant serves and what the price range is. Location is important from multiple factors. For example, it might be beneficial to locate the restaurant in an area with other restaurants to make it more accessible for example tourist who are just wandering and searching for a place to eat. But you might not want to locate an italian restaurant right next to another italian restaurant.

The purpose of this report is to identify potential restaurant concepts (i.e. italian, modern european, price category etc) and locations for a customer that is planning to open a new restaurant into Helsinki, capital of Finland. 

Based on the discussions with the customer the following criterias were identified as a key drivers for the analysis:
* What are the current restaurant concepts in Helsinki?
* Are there any restaurant concepts that are under represented in Helsinki
* The proposed location should be in an area with multiple other restaurants.
* There should not be a similar restaurant with a same concepts near by
* The neighborhood should be accessible easily via public transportation
* It beneficial to have bars or night clubs close by


## Data

For the data analysis the following data sources will be used:
* For existing restaurants and other venues such as bars we will use the Foursquare georaphical data APIs. 
* Public transportation data in the Helsinki area is available via the DigiTransit APIs.
* In addition we will use GeoJson file to visualize the Helsinki neighborhoods.

### Foursquare APIs

From the Foursquare APIs we are mainly interested about the venues, venue location and venue details. The relevant REST queries are:
```https://api.foursquare.com/v2/venues/search?client_id=xxx&client_secret=xxx&ll=60.1671972,24.9532649770553&v=20180604&query=Food&radius=500&limit=30```
for a list of venues close to a given location. The query will return a JSON file that contains the found venues:
```
{'meta': {'code': 200, 'requestId': '5c594a5d1ed2193b45233fce'},
 'response': {'venues': [{'id': '5942993fc666664eab7febf4',
    'name': 'Areperia Colombian Street Food',
    'location': {'lat': 60.167583,
     'lng': 24.957868,
     'labeledLatLngs': [{'label': 'display',
       'lat': 60.167583,
       'lng': 24.957868}],
     'distance': 258,
     'cc': 'FI',
     'city': 'Helsinki',
     'state': 'Uusimaa',
     'country': 'Suomi',
     'formattedAddress': ['Helsinki', 'Suomi']},
    'categories': [{'id': '4bf58dd8d48988d152941735',
      'name': 'Arepa Restaurant',
      'pluralName': 'Arepa Restaurants',
      'shortName': 'Arepas',
      'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/food/arepas_',
       'suffix': '.png'},
      'primary': True}],
    'referralId': 'v-1549355613',
    'hasPerk': False},
    ...
 }
 ```

From the response JSON we will capture the following data:
* Venue ID
* Venue name
* Detailed location of the venue

After we have obtained the list of venues we can use the venue ID to obtain more details about the venue.
```https://api.foursquare.com/v2/venues/5942993fc666664eab7febf4?client_id=xxx&client_secret=xxx&v=20180604```
The query returns a JSON with venue details:
```
{'id': '5942993fc666664eab7febf4',
 'name': 'Areperia Colombian Street Food',
 'contact': {'instagram': 'areperiahelsinki',
  'facebook': '1878881332377257',
  'facebookUsername': 'areperiahelsinki',
  'facebookName': 'Areperia - Colombian Street Food'},
 'location': {'lat': 60.167583,
  'lng': 24.957868,
  'labeledLatLngs': [{'label': 'display', 'lat': 60.167583, 'lng': 24.957868}],
  'cc': 'FI',
  'city': 'Helsinki',
  'state': 'Uusimaa',
  'country': 'Suomi',
  'formattedAddress': ['Helsinki', 'Suomi']},
 'canonicalUrl': 'https://foursquare.com/v/areperia-colombian-street-food/5942993fc666664eab7febf4',
 'categories': [{'id': '4bf58dd8d48988d152941735',
   'name': 'Arepa Restaurant',
   'pluralName': 'Arepa Restaurants',
   'shortName': 'Arepas',
   'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/food/arepas_',
    'suffix': '.png'},
   'primary': True},
  {'id': '4bf58dd8d48988d1cb941735',
   'name': 'Food Truck',
   'pluralName': 'Food Trucks',
   'shortName': 'Food Truck',
   'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/food/streetfood_',
    'suffix': '.png'}}],
 'verified': False,
 'stats': {'tipCount': 0},
 'url': 'https://www.areperiafoodtruck.com',
 'price': {'tier': 1, 'message': 'Cheap', 'currency': '€'},
 'likes': {'count': 0, 'groups': []},
 'dislike': False,
 'ok': False,
 'menu': {'type': 'Menu',
  'label': 'Menu',
  'anchor': 'View Menu',
  'url': 'https://www.areperiafoodtruck.com/menu',
  'mobileUrl': 'https://www.areperiafoodtruck.com/menu',
  'externalUrl': 'https://www.areperiafoodtruck.com/menu'},
 'allowMenuUrlEdit': True,
 'beenHere': {'count': 0,
  'unconfirmedCount': 0,
  'marked': False,
  'lastCheckinExpiredAt': 0},
 'specials': {'count': 0, 'items': []},
 'photos': {'count': 1,
  'groups': [{'type': 'checkin',
    'name': "Friends' check-in photos",
    'count': 0,
    'items': []},
   {'type': 'venue',
    'name': 'Venue photos',
    'count': 1,
    'items': [{'id': '599ad38958002c4ccea47e95',
      'createdAt': 1503318921,
      'source': {'name': 'Swarm for iOS', 'url': 'https://www.swarmapp.com'},
      'prefix': 'https://fastly.4sqi.net/img/general/',
      'suffix': '/69445382_nCkCMa1_wBSUD2CfdvoLOV5MpYb57zQPcVy_xqg3rCE.jpg',
      'width': 1440,
      'height': 1920,
      'user': {'id': '69445382',
       'firstName': 'Rudi',
       'lastName': 'Tamayo',
       'gender': 'female',
       'photo': {'prefix': 'https://fastly.4sqi.net/img/user/',
        'suffix': '/ZL2OD3AKV3R0DE5C.jpg'}},
      'visibility': 'public'}]}],
  'summary': '1 photo'},
 'reasons': {'count': 0, 'items': []},
 'description': 'Areperia is a food truck going around Helsinki selling authentic Colombian street food such as arepas and empanadas. Arepas and empanadas are made out of corn so therefore they are naturally gluten and dairy free.',
 'hereNow': {'count': 0, 'summary': 'Nobody here', 'groups': []},
 'createdAt': 1497536831,
 'tips': {'count': 0,
  'groups': [{'type': 'others', 'name': 'All tips', 'count': 0, 'items': []}]},
 'shortUrl': 'http://4sq.com/2ssRRKq',
 'timeZone': 'Europe/Helsinki',
 'listed': {'count': 0,
  'groups': [{'type': 'others',
    'name': 'Lists from other people',
    'count': 0,
    'items': []}]},
...
}}
```

  From the venue details response JSON we will use:
  * Category data
  * description
  * venue rating
  * Price Category
  * venue tips

### DigiTransit APIs

The API is based on GraphQL. Due to the requirement we are mainly interested about the public transportation stops close to the proposed restaurant location. For this purpose we can use the following query:
 ``` 
 {
    stopsByRadius(lat:60.199,lon:24.938,radius:500) {
      edges {
        node {
          stop { 
            gtfsId 
            name
          }
          distance
        }
      }
    }
  }
  ```

in which the lat and lon are the coordinates for our locations and radius is the search radius in meters. The query will return a JSON file containing the public transportation stops within the defined radius.

```
{
  "data": {
    "stopsByRadius": {
      "edges": [
        {
          "node": {
            "stop": {
              "gtfsId": "HSL:1173123",
              "name": "Pasilan asema"
            },
            "distance": 105
          }
        },
        {
          "node": {
            "stop": {
              "gtfsId": "HSL:1173125",
              "name": "Pasilan asema"
            },
            "distance": 116
          }
        },
        ...
      ]
    }
  }
  ```

Since we are mainly interested about the availability of the public transportation we will focus on number of stops within the radius. It is also possible to obtain more detailed route information for a particular stop but that is out of scope for this analysis.

### More restaurant data

As it turns out to be the case that the Foursquare data lacks a lot of existing restaurants we will also use another data source. eat.fi is a popular web site for restaurant info. We will use their REST API to fetch restaurants missing from the Foursquare data. Unfortunately the eat.fi data is not as clean as the Foursquare data. Therefore we need to harmonize the restaurant categories, price information and review. Luckily this is quite simple to do. We will use string matching for the category data. For price information we will just compare the average price in euros from eat.fi and map it into the five categories used by the Foursquare. Finally for the reviews we will simply multiply the eat.fi review with 2 to get a comparable review.

eat.fi REST API returns JSON data in the following format:

```
[
  {
    "id": 2,
    "name": "Töölön torigrilli",
    "latitude": 60.179001,
    "longitude": 24.923599,
    "normalizedName": "toolon-torigrilli",
    "ratingAvgOverall": 5,
    "ratingAvgQuality": 5,
    "ratingAvgExperience": 5,
    "ratingAvgPrice": 5,
    "priceAvg": 11,
    "createdAt": "2008-09-08T11:52:24.000Z",
    "commentCount": 1,
    "commentsDisabled": 0,
    "livebookingsId": null,
    "pizzaovelleUrl": null,
    "registeredOwner": null,
    "sponsorStatus": 0,
    "sponsorStartDate": null,
    "sponsorEndDate": null,
    "description": "grilliruokaa, grilli",
    "sponsorMessageTitle": null,
    "sponsorMessageBody": null,
    "city": "Helsinki",
    "cityNormalized": "helsinki",
    "campaignId": null,
    "campaignStart": null,
    "campaignEnd": null,
    "offersList": null,
    "features": [
      2
    ],
    "types": [
      12,
      22
    ],
    "times": [
      {
        "type": "open",
        "day": 3,
        "start": "11:00:00",
        "end": "05:00:00",
        "base": 1,
        "startDate": "",
        "endDate": "",
        "isSpecial": 0
      }
    ],
    "images": []
  },
  ...
  ]

```

From the data we will collect:
* Name
* Latitude and Longitude
* ratingAvgOverall
* And priceAvg

## Methodology 

First we need to download the venue data from our data sources. This is handled via the provided API and the results are stored into a Pandas dataframe. The results looks like this:
|  name                                | categories         |     lat |     lng |   price |   review |
| :------------------------------------|:-------------------|--------:|--------:|--------:|---------:|
|  Töölön torigrilli                   | grill              | 60.179  | 24.9236 |       1 |       10 |
|  Ravintola Savotta                   | finnish restaurant | 60.1689 | 24.953  |       4 |       10 |
|  Merimies Pub                        | pizza place        | 60.1605 | 24.938  |       0 |       10 |
|  Theron Eat & Work Ruoholahden Tähti | restaurant         | 60.164  | 24.9082 |       0 |       10 |
|  Theron Eat & Work Sörnäinen         | restaurant         | 60.1896 | 24.9687 |       2 |       10 |

In total we have 409 different restaurants spread over 96 different categories.

In order to better understand how the restaurants are located let's plot them onto a map:

![Restaurants in Helsinki](/img/hki-restaurants.png)

It looks like that most of the restaurants are located within few neighborhoods. To get a better undestanding let's plot a density map over the different neighborhoods. But before we can do this we need to map the restaurants into the neighborhoods. This can be achieved by loading the Helsinki GeoJSON file that contains the polygon of neighborhood coordinates. Then we can iterate over the restaurants and check whether the restaurant coordinates are inside the neighborhood polygon. After this process the dataframe looks like this:

|  neighborhood   | name                                | categories         |     lat |     lng |   price |   review |
| :---------------|:------------------------------------|:-------------------|--------:|--------:|--------:|---------:|
|  Taka-Töölö     | Töölön torigrilli                   | grill              | 60.179  | 24.9236 |       1 |       10 |
|  Kruununhaka    | Ravintola Savotta                   | finnish restaurant | 60.1689 | 24.953  |       4 |       10 |
|  Punavuori      | Merimies Pub                        | pizza place        | 60.1605 | 24.938  |       0 |       10 |
|  Ruoholahti     | Theron Eat & Work Ruoholahden Tähti | restaurant         | 60.164  | 24.9082 |       0 |       10 |
|  Hermanninmäki  | Theron Eat & Work Sörnäinen         | restaurant         | 60.1896 | 24.9687 |       2 |       10 |

Then we can group the restaurants using the neighborhood and count the number of venues and plot the color coded density onto the map.

![Restaurant density in Helsinki neighborhoods](/img/hki-restaurants-density.png)

It is easy to see from the map that majority of the restaurants is clustered into few neighborhoods. Since one of the requirements from the customer was to focus on the areas that already have plenty of restaurants let's select the neighborhoods that have 10 or more active restaurants. After this we can focus on the following neighborhoods:
* Taka-Töölö
* Kruununhaka
* Punavuori
* Kamppi
* Kaartinkaupunki
* Siltasaari
* Etu-Töölö
* Ullanlinna
* Kluuvi

Then we would like to know how similar these neighborhoods are with respect to the restaurant types. For this purpose we use k-means clustering to identify similar neighborhoods. But first let's see what kind of restaurants are most common in different neighborhoods:

|  neighborhood    | 1st Most Common Venue   | 2nd Most Common Venue   | 3rd Most Common Venue   | 4th Most Common Venue   | 5th Most Common Venue   | 6th Most Common Venue   | 7th Most Common Venue   | 8th Most Common Venue   | 9th Most Common Venue   | 10th Most Common Venue   |
| :----------------|:------------------------|:------------------------|:------------------------|:------------------------|:------------------------|:------------------------|:------------------------|:------------------------|:------------------------|:-------------------------|
|  Etu-Töölö       | restaurant              | cafe                    | finnish restaurant      | danish                  | chinese restaurant      | pizzeria                | japanese restaurant     | fast,fresh, fit,        | falafel                 | russian restaurant       |
|  Kaartinkaupunki | scandinavian restaurant | finnish restaurant      | restaurant              | steak-restaurant        | indian restaurant       | greek restaurant        | german                  | european                | seafood                 | coffee shop              |
|  Kamppi          | restaurant              | cafe                    | italian restaurant      | chinese restaurant      | sushi restaurant        | nepalese restaurant     | mexican restaurant      | japanese restaurant     | coffee shop             | pub                      |
|  Kluuvi          | restaurant              | cafe                    | italian restaurant      | pizza place             | gastropub               | finnish restaurant      | mexican restaurant      | fast kebab              | indian restaurant       | american restaurant      |
|  Kruununhaka     | finnish restaurant      | italian restaurant      | pizza place             | scandinavian restaurant | cafe                    | restaurant              | asian restaurant        | korean                  | tasty authentic         | german cafe              |
|  Punavuori       | restaurant              | japanese restaurant     | italian restaurant      | cafe                    | pizza place             | asian restaurant        | finnish restaurant      | coffee shop             | design restaurant       | deli / shop              |
|  Siltasaari      | restaurant              | cafe                    | nepalese restaurant     | fast kebab              | soup place              | coffee shop             | chinese restaurant      | sushi restaurant        | falafel                 | fast,fresh, fit,         |
|  Taka-Töölö      | restaurant              | cafe                    | japanese restaurant     | indian restaurant       | asian restaurant        | italian restaurant      | finnish restaurant      | pizza place             | mexican restaurant      | nepalese restaurant      |
|  Ullanlinna      | pizza place             | finnish restaurant      | restaurant              | italian restaurant      | french restaurant       | russian restaurant      | spanish restaurant      | cafe-terrace            | international           | german                   |

Then lets cluster the neighborhoods into 5 clusters:
|    Cluster Labels | neighborhood    |
| -----------------:|:----------------|
|                 2 | Etu-Töölö       |
|                 5 | Kaartinkaupunki |
|                 3 | Kamppi          |
|                 3 | Kluuvi          |
|                 1 | Kruununhaka     |
|                 3 | Punavuori       |
|                 4 | Siltasaari      |
|                 3 | Taka-Töölö      |
|                 1 | Ullanlinna      |

And visualize the clusters on a map:

![Helsinki neighborhoods clustered on restaurants](/img/hki-restaurants-cluster.png)

Based on clustering we can see that:

* Cluster 1 seems to be driven by the Finnish and italian cuisine
* Cluster 2 has  more generic restaurants and the only danish restaurant in the list.
* Cluster 3 are quite similar driven by "generic" restaurants and cafes. And this makes sense since these areas also contain a lot of businesses and therefore a lot of lunch restaurants and cafes.
* Cluster 4 has similar generic restaurants but it also has more fast food type places
* Cluster 5 seems to be drive by the more upscale restaurants and restaurants with more specific profile.

Next we need to investigate the restaurant types. Top 15 restaurant categories in Helsinki are:
| categories             | count |
|:-----------------------|------:|
| restaurant	         |     50|	
| cafe                   |	   30|
| finnish restaurant	 |     20|
| italian restaurant	 |     18|
| pizza place	         |     14|
| japanese restaurant	 |     12|
| chinese restaurant	 |     11|
| coffee shop	         |     10|
| asian restaurant	     |     10|
| nepalese restaurant	 |      9|
| mexican restaurant	 |      8|
| sushi restaurant	     |      8|
| indian restaurant	     |      6|
| scandinavian restaurant|	    6|

Based on the order two key points can be identified:

1. Restaurant needs a clear concept - there are plenty of generic restaurants in Helsinki
2. Finnish and Asian cuisine is quite well covered

Next let's investigate the how well restaurants are reviewed. In order to do this let's define a pivot table between neighborhood, price category and reviews. The result can be seen in the following graph:

![Helsinki neighborhoods pivoted on price and reviews](/img/avg-rating-price.png)


Finally the customer requested an area with a functioning public transportation. To estimate this we obtained the neighborhood area from the GeoJSON data, found the center point and used an "overlay circle" on the center point to give a search radius. Then we used the digitransit API to fetch the stops within a given circle. Finally we divided the number of public transportation stops with the neighborhood area. This provided the following estimates:
|  neighborhood    |    stops |
| :----------------|---------:|
|  Kamppi          | 38.7163  |
|  Kluuvi          | 25.3876  |
|  Punavuori       | 24.1782  |
|  Siltasaari      | 20.9414  |
|  Kaartinkaupunki | 14.9778  |
|  Taka-Töölö      | 12.8598  |
|  Ullanlinna      | 10.7906  |
|  Kruununhaka     | 10.1483  |
|  Etu-Töölö       |  6.24516 |

And plotted on a bar chart:
![Helsinki neighborhood stops](/img/hki-stops.png)

It needs to be noted that these figures are only estimates since the way how the neighborhood area is calculated and the overlay circle are also estimates.

## Results 


section where you discuss the results.

## Discussion 
section where you discuss any observations you noted and any recommendations you can make based on the results.

## Conclusion 
section where you conclude the report.