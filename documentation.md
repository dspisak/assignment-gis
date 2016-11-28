 # Zobrazovanie dopravných nehôd - Dokumentácia
 
 Aplikácia zobrazuje nehody v meste Des Moines štátu Iowa. Aplikácia umožňuje:
 - zadať počiatočný a koncový bod a na vyhľadanej ceste nájsť všetky autonehody
 - podľa počtu autonehôd farebne zvýrazniť kritickosť cesti vo vzťahu k autonehodám
 - zobraziť šatistický prehľad, ktorý informuje o:
    - dĺžde trasy
    - počte nehôd (rozlišuje medzi smrteĺnými a ľahkými nehodami)
    - počte nehôd na križovatkách
    - počte križovatiek
    - počte autonehôd na km
    
Aplikácia je vytvorená ako webová aplikácia, zložená z klientskej a serverovej časti. 

# Klientská časť
Klientská časť je realizovaná jednou statickou html stránkou (index.html), ktorá je rozdelená na dve časti. Prvá obsahuje formulár pre zadávanie vstupných údajov, resp. obsahuje tabuľku s výsledkami. Na vytvorenie tejto časti boli použité technológie HTML, CSS (bootstrap), ruby a javascript. 
Druhá časť statickej stránky zobrazuje mapu, ktorá je vytvorená pomocou mapbox.js. Zdrojom dát pre zobrazovanie autonehôd je geojson (na adrese /index.json), ktorý je vytvorený na serverovskov stranou aplikácie. 

# Serverovská časť
Serverovská časť je vytvorená pomocou rámca Ruby on Rails, ktorý pracuje nad databázovým systém postgres, ktorý je rozšírený o posgis. Pre prácu s geografickými objektami bol použitý gem RGeo. Geojson, ktorý je zobrazovaný klienskou časťou vytvára jbuilder. Serverovská časť využíva aj externú api pre vytvorenie trasy z počiatočného bodu ku koncovému. Celá komunikácia s touto API službou je realizovaná v triedou Direction (/lib/direction.rb). Všetky spúšťané queries (komunikácia s databázou) sú poskytované triedou MyGeoQuery (/lib/queries.rb). 

# Data
Data boli získané z dvoch zdrojov. Prvým bol Open Street Maps, ktorý bol zdrojom dát pre jedntolivé cestné komunikácie. Druhým zdrojom bola stránka https://catalog.data.gov/dataset/crash-data, ktorá obsahovala informácie o autonehodách v štáte Iowa. Pre import dát z Open Street Maps bol použítý nástroj osm2pgsql, ktorý vytvoril niekoľko tabuliek s geodátami. Pre potreby aplikácie bola použitá len tabuľka planet.osm.roads. Dáta o autonehodách boli v csv, ktorý bol imporotovaný do databázy pomocou skriptu. 

# API
Aplikácia poskytuje jednu REST službu (API), ktorá umožňuje zobraziť všetky autonehody na vyhľadanej ceste pre vybrané roky a na základe zvolených dostupných zobrazovacích kitérií:
`/tmp?sourceLat=41.57151997231558&sourceLong=-93.61454486846924&destLat=41.570187552463736&destLong=-93.61364364624023&year2013=true&year2014=true&year2015=true&showCrashes=1`

Odpoveďou na volanie API je vytvorený geojson pomocou jbuildera, ktorý okrem jednotlivých nehôd obsahuje aj trasu zo začiatočného bodu do koncového.

![Screenshot](https://github.com/dspisak/assignment-gis/blob/master/screenshot.png?raw=true)
