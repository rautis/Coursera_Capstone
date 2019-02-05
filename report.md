# Open a new restaurant in the city of Helsinki, Finland

[business-problem](## Business problem)
Opening a new restaurant is a challenging task. Not only do you need to serve quality food with suitable pricing you need to select the correct location for the restaurant. Location is important from multiple factors. For example, it might be beneficial to locate the restaurant in an area with other restaurants to make it more accessible for example tourist who are just wandering and searching for a place to eat. But you might not want to locate an italian restaurant right next to another italian restaurant.

The purpose of this report is to identify potential restaurant concepts (i.e. italian, modern european, price category etc) and locations for a customer that is planning to open a new restaurant into Helsinki, capital of Finland. 

Based on the discussions with the customer the following criterias were identified as a key drivers for the analysis:
* What are the current restaurant concepts in Helsinki?
* Are there any restaurant concepts that are under represented in Helsinki
* The proposed location should be in an area with multiple other restaurants.
* There should not be a similar restaurant with a same concepts near by
* The neighborhood should be accessible easily via public transportation
* It beneficial to have bars or night clubs close by


[data](## Data)

For the data analysis the following data sources will be used:
* For existing restaurants and other venues such as bars we will use the Foursquare georaphical data APIs. 
* Public transportation data in the Helsinki area is available via the DigiTransit APIs.
* In addition we will use GeoJson file to visualize the Helsinki neighborhoods.

Foursquare APIs

From the Foursquare APIs we are mainly interested about the venues, venue location and venue details. The relevant REST queries are:
```https://api.foursquare.com/v2/venues/search?client_id=xxx&client_secret=xxx&ll=60.1671972,24.9532649770553&v=20180604&query=Food&radius=500&limit=30´´´
for a list of venues close to a given location. The query will return a JSON file that contains the found venues:
```{'meta': {'code': 200, 'requestId': '5c594a5d1ed2193b45233fce'},
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
 }´´´

From the response JSON we will capture the following data:
* Venue ID
* Venue name
* Detailed location of the venue

After we have obtained the list of venues we can use the venue ID to obtain more details about the venue.
```https://api.foursquare.com/v2/venues/5942993fc666664eab7febf4?client_id=xxx&client_secret=xxx&v=20180604´´´
The query returns a JSON with venue details:
```{'id': '5942993fc666664eab7febf4',
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
}}´´´

  From the venue details response JSON we will use:
  * Category data
  * description
  * venue rating
  * Price Category
  * venue tips

DigiTransit APIs

The API is based on GraphQL. Due to the requirement we are mainly interested about the public transportation stops close to the proposed restaurant location. For this purpose we can use the following query:
 ``` {
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
  }´´´

in which the lat and lon are the coordinates for our locations and radius is the search radius in meters. The query will return a JSON file containing the public transportation stops within the defined radius.

```{
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
  }´´´

Since we are mainly interested about the availability of the public transportation we will focus on number of stops within the radius. It is also possible to obtain more detailed route information for a particular stop but that is out of scope for this analysis.

