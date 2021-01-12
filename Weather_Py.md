```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
from scipy.stats import linregress
```


```python
#Import Api Key
weather_api_key = "1b1f49a514a09ec6638879080888d881"

```


```python
# Incorporated citipy to determine city based on latitude and longitude
from citipy import citipy

# Output File (CSV)
output_data_file = "output_data/cities.csv"

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
```


```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
len(cities)
```




    574




```python
# Create a base url
base_url = "http://api.openweathermap.org/data/2.5/weather?units=imperial&appid=" + "1b1f49a514a09ec6638879080888d881"

# Counters
city_counter = 1
set_counter = 1

# Create the lists to hold relative data
cities_list= []
cloudiness = []
country = []
date = []
humidity = []
lat = []
lng = []
max_temp = []
wind_speed = []

print("Started")

# Create a query url for each city in the cities list to get json response
for i, city in enumerate(cities):
    
    # Group cities as sets of 50s
    if (i % 50 == 0 and i >= 50):
        set_counter += 1
        city_counter = 1
         
    # Create API url for each city
    query_url = base_url +"&q=" + city
    
    # Get json respose for each city
    response = requests.get(query_url).json()
    
    # Print the results 
    print(f"Processing Record {city_counter} of Set {set_counter} | {city}")
    
    # Increase city counter
    city_counter += 1
    
   # Add the values to the lists
    try:       
        cloudiness.append(response["clouds"]["all"])
        country.append(response["sys"]["country"])
        date.append(response["dt"])
        humidity.append(response["main"]["humidity"])
        lat.append(response["coord"]["lat"])
        lng.append(response["coord"]["lon"])
        max_temp.append(response["main"]["temp_max"])
        wind_speed.append(response["wind"]["speed"])
        cities_list.append(response["name"])
    except:
        print("City not found. Skipping...")
        pass
        
print("Completed")
```

    Started
    Processing Record 1 of Set 1 | batemans bay
    Processing Record 2 of Set 1 | taolanaro
    City not found. Skipping...
    Processing Record 3 of Set 1 | yellowknife
    Processing Record 4 of Set 1 | khatanga
    Processing Record 5 of Set 1 | yumbe
    Processing Record 6 of Set 1 | ushuaia
    Processing Record 7 of Set 1 | ilulissat
    Processing Record 8 of Set 1 | ust-kuyga
    Processing Record 9 of Set 1 | arraial do cabo
    Processing Record 10 of Set 1 | barrow
    Processing Record 11 of Set 1 | saint george
    Processing Record 12 of Set 1 | esil
    Processing Record 13 of Set 1 | rikitea
    Processing Record 14 of Set 1 | hasaki
    Processing Record 15 of Set 1 | ponta do sol
    Processing Record 16 of Set 1 | punta arenas
    Processing Record 17 of Set 1 | mizdah
    Processing Record 18 of Set 1 | mar del plata
    Processing Record 19 of Set 1 | butzow
    Processing Record 20 of Set 1 | lasa
    Processing Record 21 of Set 1 | dori
    Processing Record 22 of Set 1 | russell
    Processing Record 23 of Set 1 | bredasdorp
    Processing Record 24 of Set 1 | cape town
    Processing Record 25 of Set 1 | mataura
    Processing Record 26 of Set 1 | faanui
    Processing Record 27 of Set 1 | kimbe
    Processing Record 28 of Set 1 | bandarbeyla
    Processing Record 29 of Set 1 | umkomaas
    Processing Record 30 of Set 1 | kotri
    Processing Record 31 of Set 1 | varhaug
    Processing Record 32 of Set 1 | tidore
    City not found. Skipping...
    Processing Record 33 of Set 1 | ruidoso
    Processing Record 34 of Set 1 | busselton
    Processing Record 35 of Set 1 | vaini
    Processing Record 36 of Set 1 | cocachacra
    Processing Record 37 of Set 1 | narsaq
    Processing Record 38 of Set 1 | tasiilaq
    Processing Record 39 of Set 1 | pemangkat
    Processing Record 40 of Set 1 | longyearbyen
    Processing Record 41 of Set 1 | avarua
    Processing Record 42 of Set 1 | barentsburg
    City not found. Skipping...
    Processing Record 43 of Set 1 | chuy
    Processing Record 44 of Set 1 | xadani
    City not found. Skipping...
    Processing Record 45 of Set 1 | puerto escondido
    Processing Record 46 of Set 1 | atuona
    Processing Record 47 of Set 1 | carnarvon
    Processing Record 48 of Set 1 | tutoia
    Processing Record 49 of Set 1 | mirante do paranapanema
    Processing Record 50 of Set 1 | hobyo
    Processing Record 1 of Set 2 | jhikargachha
    City not found. Skipping...
    Processing Record 2 of Set 2 | lahij
    Processing Record 3 of Set 2 | tual
    Processing Record 4 of Set 2 | bengkulu
    Processing Record 5 of Set 2 | abu dhabi
    Processing Record 6 of Set 2 | clyde river
    Processing Record 7 of Set 2 | banmo
    City not found. Skipping...
    Processing Record 8 of Set 2 | albany
    Processing Record 9 of Set 2 | lavrentiya
    Processing Record 10 of Set 2 | lagoa
    Processing Record 11 of Set 2 | gusau
    Processing Record 12 of Set 2 | torbay
    Processing Record 13 of Set 2 | attawapiskat
    City not found. Skipping...
    Processing Record 14 of Set 2 | ballina
    Processing Record 15 of Set 2 | katobu
    Processing Record 16 of Set 2 | nanortalik
    Processing Record 17 of Set 2 | nikolskoye
    Processing Record 18 of Set 2 | saint anthony
    Processing Record 19 of Set 2 | kisangani
    Processing Record 20 of Set 2 | moose factory
    Processing Record 21 of Set 2 | ondorhaan
    City not found. Skipping...
    Processing Record 22 of Set 2 | san policarpo
    Processing Record 23 of Set 2 | jamestown
    Processing Record 24 of Set 2 | zabaykalsk
    Processing Record 25 of Set 2 | hami
    Processing Record 26 of Set 2 | nouadhibou
    Processing Record 27 of Set 2 | kapaa
    Processing Record 28 of Set 2 | kahului
    Processing Record 29 of Set 2 | puerto ayora
    Processing Record 30 of Set 2 | mandalgovi
    Processing Record 31 of Set 2 | hermanus
    Processing Record 32 of Set 2 | sola
    Processing Record 33 of Set 2 | aklavik
    Processing Record 34 of Set 2 | qaanaaq
    Processing Record 35 of Set 2 | saleaula
    City not found. Skipping...
    Processing Record 36 of Set 2 | san quintin
    Processing Record 37 of Set 2 | prabumulih
    Processing Record 38 of Set 2 | aksu
    Processing Record 39 of Set 2 | sorso
    Processing Record 40 of Set 2 | nakamura
    Processing Record 41 of Set 2 | ostrovnoy
    Processing Record 42 of Set 2 | urumqi
    Processing Record 43 of Set 2 | nanga eboko
    Processing Record 44 of Set 2 | port elizabeth
    Processing Record 45 of Set 2 | saskylakh
    Processing Record 46 of Set 2 | ahipara
    Processing Record 47 of Set 2 | pevek
    Processing Record 48 of Set 2 | vrede
    Processing Record 49 of Set 2 | touros
    Processing Record 50 of Set 2 | norman wells
    Processing Record 1 of Set 3 | matara
    Processing Record 2 of Set 3 | rocha
    Processing Record 3 of Set 3 | illoqqortoormiut
    City not found. Skipping...
    Processing Record 4 of Set 3 | cockburn town
    Processing Record 5 of Set 3 | mansa
    Processing Record 6 of Set 3 | jiddah
    City not found. Skipping...
    Processing Record 7 of Set 3 | butaritari
    Processing Record 8 of Set 3 | saint-philippe
    Processing Record 9 of Set 3 | ribeira grande
    Processing Record 10 of Set 3 | mocambique
    City not found. Skipping...
    Processing Record 11 of Set 3 | fare
    Processing Record 12 of Set 3 | hobart
    Processing Record 13 of Set 3 | port alfred
    Processing Record 14 of Set 3 | tuatapere
    Processing Record 15 of Set 3 | new norfolk
    Processing Record 16 of Set 3 | pacific grove
    Processing Record 17 of Set 3 | amderma
    City not found. Skipping...
    Processing Record 18 of Set 3 | quatre cocos
    Processing Record 19 of Set 3 | mago
    Processing Record 20 of Set 3 | faya
    Processing Record 21 of Set 3 | kaitangata
    Processing Record 22 of Set 3 | castro
    Processing Record 23 of Set 3 | bhimunipatnam
    Processing Record 24 of Set 3 | zig
    Processing Record 25 of Set 3 | dukat
    Processing Record 26 of Set 3 | marcona
    City not found. Skipping...
    Processing Record 27 of Set 3 | vagur
    Processing Record 28 of Set 3 | tarbagatay
    Processing Record 29 of Set 3 | kodiak
    Processing Record 30 of Set 3 | sao joao da barra
    Processing Record 31 of Set 3 | egvekinot
    Processing Record 32 of Set 3 | beringovskiy
    Processing Record 33 of Set 3 | cherskiy
    Processing Record 34 of Set 3 | cidreira
    Processing Record 35 of Set 3 | praya
    Processing Record 36 of Set 3 | boundiali
    Processing Record 37 of Set 3 | cherdyn
    Processing Record 38 of Set 3 | bathsheba
    Processing Record 39 of Set 3 | bardiyah
    Processing Record 40 of Set 3 | esmeralda
    Processing Record 41 of Set 3 | bolshoy tsaryn
    City not found. Skipping...
    Processing Record 42 of Set 3 | kamenka
    Processing Record 43 of Set 3 | airai
    Processing Record 44 of Set 3 | broome
    Processing Record 45 of Set 3 | mvuma
    Processing Record 46 of Set 3 | bluff
    Processing Record 47 of Set 3 | yemtsa
    Processing Record 48 of Set 3 | port hardy
    Processing Record 49 of Set 3 | obihiro
    Processing Record 50 of Set 3 | tommot
    Processing Record 1 of Set 4 | tsihombe
    City not found. Skipping...
    Processing Record 2 of Set 4 | codrington
    Processing Record 3 of Set 4 | viedma
    Processing Record 4 of Set 4 | sao filipe
    Processing Record 5 of Set 4 | coruche
    Processing Record 6 of Set 4 | east london
    Processing Record 7 of Set 4 | issoire
    Processing Record 8 of Set 4 | madang
    Processing Record 9 of Set 4 | virginia beach
    Processing Record 10 of Set 4 | tuktoyaktuk
    Processing Record 11 of Set 4 | chokurdakh
    Processing Record 12 of Set 4 | katsuura
    Processing Record 13 of Set 4 | meulaboh
    Processing Record 14 of Set 4 | lebu
    Processing Record 15 of Set 4 | half moon bay
    Processing Record 16 of Set 4 | college
    Processing Record 17 of Set 4 | georgetown
    Processing Record 18 of Set 4 | aksehir
    Processing Record 19 of Set 4 | khani
    Processing Record 20 of Set 4 | srivardhan
    Processing Record 21 of Set 4 | huron
    Processing Record 22 of Set 4 | kavieng
    Processing Record 23 of Set 4 | tchollire
    Processing Record 24 of Set 4 | logumkloster
    Processing Record 25 of Set 4 | maldonado
    Processing Record 26 of Set 4 | westport
    Processing Record 27 of Set 4 | shache
    Processing Record 28 of Set 4 | deloraine
    Processing Record 29 of Set 4 | lompoc
    Processing Record 30 of Set 4 | suntar
    Processing Record 31 of Set 4 | alta
    Processing Record 32 of Set 4 | maltahohe
    Processing Record 33 of Set 4 | kearney
    Processing Record 34 of Set 4 | kruisfontein
    Processing Record 35 of Set 4 | fortuna
    Processing Record 36 of Set 4 | dikson
    Processing Record 37 of Set 4 | abonnema
    Processing Record 38 of Set 4 | kualakapuas
    Processing Record 39 of Set 4 | samarai
    Processing Record 40 of Set 4 | vaitupu
    City not found. Skipping...
    Processing Record 41 of Set 4 | vao
    Processing Record 42 of Set 4 | thompson
    Processing Record 43 of Set 4 | sentyabrskiy
    City not found. Skipping...
    Processing Record 44 of Set 4 | bahia honda
    Processing Record 45 of Set 4 | bani walid
    Processing Record 46 of Set 4 | sioux lookout
    Processing Record 47 of Set 4 | mys shmidta
    City not found. Skipping...
    Processing Record 48 of Set 4 | port hedland
    Processing Record 49 of Set 4 | hilo
    Processing Record 50 of Set 4 | elbrus
    Processing Record 1 of Set 5 | thohoyandou
    Processing Record 2 of Set 5 | manta
    Processing Record 3 of Set 5 | lolua
    City not found. Skipping...
    Processing Record 4 of Set 5 | itacoatiara
    Processing Record 5 of Set 5 | batagay-alyta
    Processing Record 6 of Set 5 | yar-sale
    Processing Record 7 of Set 5 | sampit
    Processing Record 8 of Set 5 | abashiri
    Processing Record 9 of Set 5 | volot
    Processing Record 10 of Set 5 | iskateley
    Processing Record 11 of Set 5 | davila
    Processing Record 12 of Set 5 | pampa
    Processing Record 13 of Set 5 | moron
    Processing Record 14 of Set 5 | high level
    Processing Record 15 of Set 5 | cap malheureux
    Processing Record 16 of Set 5 | tabou
    Processing Record 17 of Set 5 | rio branco do sul
    Processing Record 18 of Set 5 | samusu
    City not found. Skipping...
    Processing Record 19 of Set 5 | igualada
    Processing Record 20 of Set 5 | wollongong
    Processing Record 21 of Set 5 | pirai do sul
    Processing Record 22 of Set 5 | avera
    Processing Record 23 of Set 5 | gat
    Processing Record 24 of Set 5 | acarau
    Processing Record 25 of Set 5 | quelimane
    Processing Record 26 of Set 5 | aquiraz
    Processing Record 27 of Set 5 | port blair
    Processing Record 28 of Set 5 | noumea
    Processing Record 29 of Set 5 | nizhneyansk
    City not found. Skipping...
    Processing Record 30 of Set 5 | kavaratti
    Processing Record 31 of Set 5 | hofn
    Processing Record 32 of Set 5 | zhuhai
    Processing Record 33 of Set 5 | constitucion
    Processing Record 34 of Set 5 | wazzan
    City not found. Skipping...
    Processing Record 35 of Set 5 | daphne
    Processing Record 36 of Set 5 | leticia
    Processing Record 37 of Set 5 | bosaso
    Processing Record 38 of Set 5 | san cristobal
    Processing Record 39 of Set 5 | kirakira
    Processing Record 40 of Set 5 | chapais
    Processing Record 41 of Set 5 | haines junction
    Processing Record 42 of Set 5 | isangel
    Processing Record 43 of Set 5 | wasilla
    Processing Record 44 of Set 5 | yershichi
    Processing Record 45 of Set 5 | tiksi
    Processing Record 46 of Set 5 | kirkenaer
    Processing Record 47 of Set 5 | burnie
    Processing Record 48 of Set 5 | assiniboia
    Processing Record 49 of Set 5 | poum
    Processing Record 50 of Set 5 | lorengau
    Processing Record 1 of Set 6 | kegayli
    City not found. Skipping...
    Processing Record 2 of Set 6 | buala
    Processing Record 3 of Set 6 | abnub
    Processing Record 4 of Set 6 | pangnirtung
    Processing Record 5 of Set 6 | padang
    Processing Record 6 of Set 6 | tachov
    Processing Record 7 of Set 6 | diffa
    Processing Record 8 of Set 6 | fairbanks
    Processing Record 9 of Set 6 | price
    Processing Record 10 of Set 6 | laguna
    Processing Record 11 of Set 6 | maniitsoq
    Processing Record 12 of Set 6 | bilma
    Processing Record 13 of Set 6 | libertador general san martin
    Processing Record 14 of Set 6 | hithadhoo
    Processing Record 15 of Set 6 | dilla
    Processing Record 16 of Set 6 | rodeo
    Processing Record 17 of Set 6 | clemson
    Processing Record 18 of Set 6 | esperance
    Processing Record 19 of Set 6 | ketchikan
    Processing Record 20 of Set 6 | rio grande
    Processing Record 21 of Set 6 | birin
    Processing Record 22 of Set 6 | adre
    Processing Record 23 of Set 6 | hirara
    Processing Record 24 of Set 6 | guadalajara
    Processing Record 25 of Set 6 | leningradskiy
    Processing Record 26 of Set 6 | shubarshi
    Processing Record 27 of Set 6 | praia
    Processing Record 28 of Set 6 | broken hill
    Processing Record 29 of Set 6 | mangrol
    Processing Record 30 of Set 6 | mahebourg
    Processing Record 31 of Set 6 | talnakh
    Processing Record 32 of Set 6 | muros
    Processing Record 33 of Set 6 | parkes
    Processing Record 34 of Set 6 | muyezerskiy
    Processing Record 35 of Set 6 | ambodifototra
    City not found. Skipping...
    Processing Record 36 of Set 6 | pisco
    Processing Record 37 of Set 6 | nome
    Processing Record 38 of Set 6 | ca mau
    Processing Record 39 of Set 6 | sept-iles
    Processing Record 40 of Set 6 | islamkot
    Processing Record 41 of Set 6 | bay saint louis
    Processing Record 42 of Set 6 | aksarka
    Processing Record 43 of Set 6 | ada
    Processing Record 44 of Set 6 | geraldton
    Processing Record 45 of Set 6 | anadyr
    Processing Record 46 of Set 6 | majene
    Processing Record 47 of Set 6 | la suiza
    Processing Record 48 of Set 6 | marzuq
    Processing Record 49 of Set 6 | mazagao
    Processing Record 50 of Set 6 | provideniya
    Processing Record 1 of Set 7 | deputatskiy
    Processing Record 2 of Set 7 | severo-kurilsk
    Processing Record 3 of Set 7 | leua
    Processing Record 4 of Set 7 | bowen
    Processing Record 5 of Set 7 | dingle
    Processing Record 6 of Set 7 | sitka
    Processing Record 7 of Set 7 | gollere
    City not found. Skipping...
    Processing Record 8 of Set 7 | havre-saint-pierre
    Processing Record 9 of Set 7 | bethel
    Processing Record 10 of Set 7 | cayenne
    Processing Record 11 of Set 7 | krasnoselkup
    Processing Record 12 of Set 7 | saldanha
    Processing Record 13 of Set 7 | elk plain
    Processing Record 14 of Set 7 | amurzet
    Processing Record 15 of Set 7 | sungaipenuh
    Processing Record 16 of Set 7 | port lincoln
    Processing Record 17 of Set 7 | kamenskoye
    City not found. Skipping...
    Processing Record 18 of Set 7 | fallon
    Processing Record 19 of Set 7 | tuni
    Processing Record 20 of Set 7 | iqaluit
    Processing Record 21 of Set 7 | manado
    Processing Record 22 of Set 7 | pueblo nuevo
    Processing Record 23 of Set 7 | taltal
    Processing Record 24 of Set 7 | heihe
    Processing Record 25 of Set 7 | alofi
    Processing Record 26 of Set 7 | alyangula
    Processing Record 27 of Set 7 | ixtapa
    Processing Record 28 of Set 7 | shimoda
    Processing Record 29 of Set 7 | naze
    Processing Record 30 of Set 7 | ongandjera
    Processing Record 31 of Set 7 | teya
    Processing Record 32 of Set 7 | blagoveshchenka
    Processing Record 33 of Set 7 | zhicheng
    Processing Record 34 of Set 7 | dumas
    Processing Record 35 of Set 7 | bud
    Processing Record 36 of Set 7 | souillac
    Processing Record 37 of Set 7 | lafiagi
    Processing Record 38 of Set 7 | jati
    Processing Record 39 of Set 7 | lishui
    Processing Record 40 of Set 7 | vestmannaeyjar
    Processing Record 41 of Set 7 | srednekolymsk
    Processing Record 42 of Set 7 | cabo san lucas
    Processing Record 43 of Set 7 | guerrero negro
    Processing Record 44 of Set 7 | perbaungan
    Processing Record 45 of Set 7 | alta floresta
    Processing Record 46 of Set 7 | palabuhanratu
    City not found. Skipping...
    Processing Record 47 of Set 7 | jiexiu
    Processing Record 48 of Set 7 | belushya guba
    City not found. Skipping...
    Processing Record 49 of Set 7 | mehamn
    Processing Record 50 of Set 7 | hamilton
    Processing Record 1 of Set 8 | luganville
    Processing Record 2 of Set 8 | bambous virieux
    Processing Record 3 of Set 8 | sao jose da coroa grande
    Processing Record 4 of Set 8 | saint-georges
    Processing Record 5 of Set 8 | qui nhon
    Processing Record 6 of Set 8 | husavik
    Processing Record 7 of Set 8 | rio abajo
    Processing Record 8 of Set 8 | upernavik
    Processing Record 9 of Set 8 | bellingham
    Processing Record 10 of Set 8 | kokopo
    Processing Record 11 of Set 8 | milingimbi
    City not found. Skipping...
    Processing Record 12 of Set 8 | nchelenge
    Processing Record 13 of Set 8 | ankang
    Processing Record 14 of Set 8 | ancud
    Processing Record 15 of Set 8 | coracao de jesus
    Processing Record 16 of Set 8 | saint-joseph
    Processing Record 17 of Set 8 | dickinson
    Processing Record 18 of Set 8 | olafsvik
    Processing Record 19 of Set 8 | skalistyy
    City not found. Skipping...
    Processing Record 20 of Set 8 | oranjestad
    Processing Record 21 of Set 8 | copiapo
    Processing Record 22 of Set 8 | lata
    Processing Record 23 of Set 8 | lixourion
    Processing Record 24 of Set 8 | bentiu
    Processing Record 25 of Set 8 | kaeo
    Processing Record 26 of Set 8 | aguimes
    Processing Record 27 of Set 8 | buin
    Processing Record 28 of Set 8 | corn island
    Processing Record 29 of Set 8 | tautira
    Processing Record 30 of Set 8 | juruti
    Processing Record 31 of Set 8 | bhakkar
    Processing Record 32 of Set 8 | great yarmouth
    Processing Record 33 of Set 8 | edd
    Processing Record 34 of Set 8 | sur
    Processing Record 35 of Set 8 | elizabeth city
    Processing Record 36 of Set 8 | jumla
    Processing Record 37 of Set 8 | verkhoyansk
    Processing Record 38 of Set 8 | kovdor
    Processing Record 39 of Set 8 | road town
    Processing Record 40 of Set 8 | maragogi
    Processing Record 41 of Set 8 | todos santos
    Processing Record 42 of Set 8 | araouane
    Processing Record 43 of Set 8 | namibe
    Processing Record 44 of Set 8 | nanakuli
    Processing Record 45 of Set 8 | nishihara
    Processing Record 46 of Set 8 | nouakchott
    Processing Record 47 of Set 8 | mackay
    Processing Record 48 of Set 8 | nagua
    Processing Record 49 of Set 8 | dergachi
    Processing Record 50 of Set 8 | komsomolskiy
    Processing Record 1 of Set 9 | yakima
    Processing Record 2 of Set 9 | jurbarkas
    Processing Record 3 of Set 9 | inuvik
    Processing Record 4 of Set 9 | burayevo
    Processing Record 5 of Set 9 | ironton
    Processing Record 6 of Set 9 | porto novo
    Processing Record 7 of Set 9 | san patricio
    Processing Record 8 of Set 9 | khandbari
    Processing Record 9 of Set 9 | gao
    Processing Record 10 of Set 9 | burns lake
    Processing Record 11 of Set 9 | manokwari
    Processing Record 12 of Set 9 | bolungarvik
    City not found. Skipping...
    Processing Record 13 of Set 9 | cocorit
    Processing Record 14 of Set 9 | ocosingo
    Processing Record 15 of Set 9 | salaga
    Processing Record 16 of Set 9 | comodoro rivadavia
    Processing Record 17 of Set 9 | brazzaville
    Processing Record 18 of Set 9 | vostok
    Processing Record 19 of Set 9 | hashtrud
    Processing Record 20 of Set 9 | karratha
    Processing Record 21 of Set 9 | velikooktyabrskiy
    Processing Record 22 of Set 9 | rudnichnyy
    Processing Record 23 of Set 9 | ponta delgada
    Processing Record 24 of Set 9 | lujan
    Processing Record 25 of Set 9 | acton vale
    Processing Record 26 of Set 9 | palana
    Processing Record 27 of Set 9 | puerto del rosario
    Processing Record 28 of Set 9 | cambridge
    Processing Record 29 of Set 9 | marrakesh
    Processing Record 30 of Set 9 | klaksvik
    Processing Record 31 of Set 9 | manadhoo
    Processing Record 32 of Set 9 | casper
    Processing Record 33 of Set 9 | ambon
    Processing Record 34 of Set 9 | bundaberg
    Processing Record 35 of Set 9 | yenagoa
    Processing Record 36 of Set 9 | canutama
    Processing Record 37 of Set 9 | teguldet
    Processing Record 38 of Set 9 | sao felix do xingu
    Processing Record 39 of Set 9 | blagoyevo
    Processing Record 40 of Set 9 | toliary
    City not found. Skipping...
    Processing Record 41 of Set 9 | valdez
    Processing Record 42 of Set 9 | moyale
    Processing Record 43 of Set 9 | christchurch
    Processing Record 44 of Set 9 | zhigansk
    Processing Record 45 of Set 9 | mount gambier
    Processing Record 46 of Set 9 | gallup
    Processing Record 47 of Set 9 | sainte-maxime
    Processing Record 48 of Set 9 | bajos de haina
    Processing Record 49 of Set 9 | sorland
    Processing Record 50 of Set 9 | san blas
    Processing Record 1 of Set 10 | omsukchan
    Processing Record 2 of Set 10 | murdochville
    Processing Record 3 of Set 10 | san jose
    Processing Record 4 of Set 10 | mayo
    Processing Record 5 of Set 10 | plouzane
    Processing Record 6 of Set 10 | menongue
    Processing Record 7 of Set 10 | leona vicario
    Processing Record 8 of Set 10 | arroyo
    Processing Record 9 of Set 10 | poshekhonye
    Processing Record 10 of Set 10 | mukilteo
    Processing Record 11 of Set 10 | fereydunshahr
    Processing Record 12 of Set 10 | yumen
    Processing Record 13 of Set 10 | altus
    Processing Record 14 of Set 10 | jomalig
    City not found. Skipping...
    Processing Record 15 of Set 10 | cedar city
    Processing Record 16 of Set 10 | lazaro cardenas
    Processing Record 17 of Set 10 | korla
    Processing Record 18 of Set 10 | manicore
    Processing Record 19 of Set 10 | manakara
    Processing Record 20 of Set 10 | kotelnich
    Processing Record 21 of Set 10 | makakilo city
    Processing Record 22 of Set 10 | holme
    Processing Record 23 of Set 10 | chicama
    Processing Record 24 of Set 10 | boa vista
    Processing Record 25 of Set 10 | bolshaya murta
    City not found. Skipping...
    Processing Record 26 of Set 10 | puerto madero
    Processing Record 27 of Set 10 | luderitz
    Processing Record 28 of Set 10 | salisbury
    Processing Record 29 of Set 10 | srandakan
    Processing Record 30 of Set 10 | grindavik
    Processing Record 31 of Set 10 | anchorage
    Processing Record 32 of Set 10 | meadow lake
    Processing Record 33 of Set 10 | ruwi
    Processing Record 34 of Set 10 | dhadar
    Processing Record 35 of Set 10 | dongfeng
    Processing Record 36 of Set 10 | galle
    Processing Record 37 of Set 10 | temaraia
    City not found. Skipping...
    Processing Record 38 of Set 10 | bangkalan
    Processing Record 39 of Set 10 | dolny kubin
    Processing Record 40 of Set 10 | kwinana
    Processing Record 41 of Set 10 | sambava
    Processing Record 42 of Set 10 | watertown
    Processing Record 43 of Set 10 | kholodnyy
    Processing Record 44 of Set 10 | eisiskes
    Processing Record 45 of Set 10 | mokhotlong
    Processing Record 46 of Set 10 | bonavista
    Processing Record 47 of Set 10 | victoria
    Processing Record 48 of Set 10 | gidole
    Processing Record 49 of Set 10 | at-bashi
    Processing Record 50 of Set 10 | te anau
    Processing Record 1 of Set 11 | gouyave
    Processing Record 2 of Set 11 | angoche
    Processing Record 3 of Set 11 | ceres
    Processing Record 4 of Set 11 | ndele
    Processing Record 5 of Set 11 | mahajanga
    Processing Record 6 of Set 11 | kidal
    Processing Record 7 of Set 11 | sinkat
    City not found. Skipping...
    Processing Record 8 of Set 11 | lalmohan
    Processing Record 9 of Set 11 | carutapera
    Processing Record 10 of Set 11 | iranduba
    Processing Record 11 of Set 11 | reftinskiy
    Processing Record 12 of Set 11 | bambanglipuro
    Processing Record 13 of Set 11 | goderich
    Processing Record 14 of Set 11 | alihe
    Processing Record 15 of Set 11 | maceio
    Processing Record 16 of Set 11 | tocantinopolis
    City not found. Skipping...
    Processing Record 17 of Set 11 | bhindar
    Processing Record 18 of Set 11 | san ramon de la nueva oran
    Processing Record 19 of Set 11 | kamensk-shakhtinskiy
    Processing Record 20 of Set 11 | vila do maio
    Processing Record 21 of Set 11 | cumaru
    Processing Record 22 of Set 11 | yerofey pavlovich
    Processing Record 23 of Set 11 | diego de almagro
    Processing Record 24 of Set 11 | ulagan
    Processing Record 25 of Set 11 | taoudenni
    Processing Record 26 of Set 11 | batagay
    Processing Record 27 of Set 11 | ust-maya
    Processing Record 28 of Set 11 | turkistan
    Processing Record 29 of Set 11 | sechura
    Processing Record 30 of Set 11 | kilrush
    Processing Record 31 of Set 11 | alotau
    City not found. Skipping...
    Processing Record 32 of Set 11 | la ronge
    Processing Record 33 of Set 11 | namatanai
    Processing Record 34 of Set 11 | hambantota
    Processing Record 35 of Set 11 | erdemli
    Processing Record 36 of Set 11 | mangaluru
    Processing Record 37 of Set 11 | labuhan
    Processing Record 38 of Set 11 | fort nelson
    Processing Record 39 of Set 11 | adeje
    Processing Record 40 of Set 11 | dzhalil
    Processing Record 41 of Set 11 | kpandu
    Processing Record 42 of Set 11 | necochea
    Processing Record 43 of Set 11 | kalabo
    Processing Record 44 of Set 11 | general roca
    Processing Record 45 of Set 11 | balabac
    Processing Record 46 of Set 11 | nikolayevsk-na-amure
    Processing Record 47 of Set 11 | karakol
    Processing Record 48 of Set 11 | barawe
    City not found. Skipping...
    Processing Record 49 of Set 11 | saint-augustin
    Processing Record 50 of Set 11 | flin flon
    Processing Record 1 of Set 12 | sao raimundo nonato
    Processing Record 2 of Set 12 | basoko
    Processing Record 3 of Set 12 | champerico
    Processing Record 4 of Set 12 | marsh harbour
    Processing Record 5 of Set 12 | petit goave
    Processing Record 6 of Set 12 | himora
    City not found. Skipping...
    Processing Record 7 of Set 12 | wilmington island
    Processing Record 8 of Set 12 | abiy adi
    City not found. Skipping...
    Processing Record 9 of Set 12 | jackson
    Processing Record 10 of Set 12 | caceres
    Processing Record 11 of Set 12 | ust-kamchatsk
    City not found. Skipping...
    Processing Record 12 of Set 12 | sosua
    Processing Record 13 of Set 12 | kielce
    Processing Record 14 of Set 12 | rantepao
    Processing Record 15 of Set 12 | zolotinka
    City not found. Skipping...
    Processing Record 16 of Set 12 | tumannyy
    City not found. Skipping...
    Processing Record 17 of Set 12 | tucumcari
    Processing Record 18 of Set 12 | dicabisagan
    Processing Record 19 of Set 12 | birao
    Processing Record 20 of Set 12 | junnar
    Processing Record 21 of Set 12 | sheridan
    Processing Record 22 of Set 12 | inta
    Processing Record 23 of Set 12 | tilichiki
    Processing Record 24 of Set 12 | kargopol
    Completed



```python
# Create a dictionary to keep data 
weather_data = {
     "City": cities_list,
     "Cloudiness": cloudiness,
     "Country": country,
     "Date": date,
     "Humidity": humidity,
     "Lat": lat,
     "Lng": lng,
     "Max Temp": max_temp,
     "Wind Speed": wind_speed    
 }

# Create the data frame and count variables for each columns
weather_df = pd.DataFrame(weather_data)
weather_df.count()
weather_df.to_csv("Weather_Py.csv")
```


```python
# Display the data frame
weather_df.head()
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
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Batemans Bay</td>
      <td>89</td>
      <td>AU</td>
      <td>1610412519</td>
      <td>40</td>
      <td>-35.7167</td>
      <td>150.1833</td>
      <td>84.99</td>
      <td>4.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Yellowknife</td>
      <td>20</td>
      <td>CA</td>
      <td>1610412519</td>
      <td>92</td>
      <td>62.4560</td>
      <td>-114.3525</td>
      <td>8.60</td>
      <td>9.22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Khatanga</td>
      <td>100</td>
      <td>RU</td>
      <td>1610412519</td>
      <td>89</td>
      <td>71.9667</td>
      <td>102.5000</td>
      <td>-18.33</td>
      <td>3.44</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Yumbe</td>
      <td>0</td>
      <td>UG</td>
      <td>1610412519</td>
      <td>33</td>
      <td>3.4651</td>
      <td>31.2469</td>
      <td>71.02</td>
      <td>4.16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ushuaia</td>
      <td>75</td>
      <td>AR</td>
      <td>1610412519</td>
      <td>76</td>
      <td>-54.8000</td>
      <td>-68.3000</td>
      <td>46.40</td>
      <td>13.80</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a scatter plot for temperature and latitude
plt.scatter(weather_df["Lat"], weather_df["Max Temp"], marker = "o", facecolor = "navy", edgecolor="blue")
plt.title("Max Temperature vs. City Latitude (%s)" % time.strftime("%x"))
plt.xlabel("Max Temperature (F)")
plt.ylabel("Latitude")
plt.grid()
plt.savefig("Max Temp vs Lat.png")
plt.show()
```

![png](output_7_0.png)

ANALYSIS: For this time of the year, January 2021, the max temperature of the cities listed shows that those with the latitude in the negative have the higher temperatutres. This is more than likely due to the position o



```python
# Create a scatter plot for humidity and latitude
plt.scatter(weather_df["Lat"], weather_df["Humidity"], marker = "o", facecolor = "navy", edgecolor="blue")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.title("City Latitude vs Humidity (%s)" % time.strftime("%x"))
plt.savefig("Lat vs Humidity.png")
plt.grid()
plt.show()
```

![png](output_8_0.png)

ANALYSIS: Cities that are in the 50-60 degrees of Latitude appear to have 80-90% of humidity. 



```python
# Create a scatter plot for latitdue and cloudiness
plt.scatter(weather_df["Lat"], weather_df["Cloudiness"], marker = "o", facecolor = "navy", edgecolor="blue")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.title("City Latitude vs Cloudiness (%s)" % time.strftime("%x"))
plt.savefig("Lat vs Cloudiness.png")
plt.grid()
plt.show()
```

![png](output_9_0.png)

ANALYSIS: The cloudiness extremes, 0% or 100%, have concentration in cities in all latitudes, both positive and negative. 



```python
# Create a scatter plot for Wind Speed and Latitude 
plt.scatter(weather_df["Lat"], weather_df["Wind Speed"], marker = "o", facecolor = "navy", edgecolor="blue")
plt.xlabel("Latitude")
plt.ylabel("Speed (mph)")
plt.title("City Latitude vs Wind Speed (%s)" % time.strftime("%x"))
plt.savefig("Lat vs Wind Speed.png")
plt.grid()
plt.show()
```

![png](output_10_0.png)

ANALYSIS: The wind speed does not reach beyond 20 mph anywhere in the world with consistency, aside from a couple of anomolies. 



```python
# Create Northern And Southern Hemisphere Data Frame

Northern_Hemisphere=weather_df[weather_df["Lat"]>0]
Southern_Hemisphere=weather_df[weather_df["Lat"]<0]
```


```python
# Define Linear Regression Function
def Linear_Regression(x_values,y_values,xlabel,ylabel,annotate_coorid=(0,0)):
    (slope, intercept, rvalue, pvalue, stderr) = linregress(x_values, y_values)
    regress_values = x_values * slope + intercept
    line_eq = "y = " + str(round(slope,2)) + "x + " + str(round(intercept,2))
    plt.scatter(x_values,y_values)
    plt.plot(x_values,regress_values,"r-")
    plt.annotate(line_eq,annotate_coorid,fontsize=15,color="red")
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.title(f"{xlabel} vs {ylabel}")
    plt.show()
```


```python
#Run Linear Regression
Linear_Regression(Northern_Hemisphere["Max Temp"],Northern_Hemisphere["Lat"],"Max Temprature (F)","Latitude")
```


![png](output_13_0.png)



```python
Linear_Regression(Southern_Hemisphere["Max Temp"],Southern_Hemisphere["Lat"],"Max Temprature (F)","Latitude",(35,-5))

```

![png](output_14_0.png)

ANALYSIS: The lower the latitude the higher the temperature in the Northern Hemisphere. 

```python
Linear_Regression(Northern_Hemisphere["Humidity"],Northern_Hemisphere["Lat"],"Max Temprature (F)","Latitude",(20,60))

```


![png](output_15_0.png)



```python
Linear_Regression(Southern_Hemisphere["Humidity"],Southern_Hemisphere["Lat"],"Max Temprature (F)","Latitude",(20,-15))

```

![png](output_16_0.png)

ANALYSIS:  Both hemispheres have a larger cluster of Max temperatures at the higher latitude.



```python
Linear_Regression(Northern_Hemisphere["Cloudiness"],Northern_Hemisphere["Lat"],"Max Temprature (F)","Latitude",(25,5))

```


![png](output_17_0.png)



```python
Linear_Regression(Southern_Hemisphere["Cloudiness"],Southern_Hemisphere["Lat"],"Max Temprature (F)","Latitude",(20,-50))

```

![png](output_18_0.png)

ANALYSIS: The norhern hemisphere has a fairly even distribution but the extremes of 0 and 100% cloudiness have more cities present.



```python
Linear_Regression(Northern_Hemisphere["Wind Speed"],Northern_Hemisphere["Lat"],"Max Temprature (F)","Latitude",(25,5))

```


![png](output_19_0.png)



```python
Linear_Regression(Southern_Hemisphere["Wind Speed"],Southern_Hemisphere["Lat"],"Max Temprature (F)","Latitude",(5,-50))

```


![png](output_20_0.png)

 ANALYSIS: The windspeed, in either hemisphere does not reach past 25 mph.

```python

```
