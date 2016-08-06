* Geospatial Mapping with D3

** Project Setup
  - Install topojson command line tool (use npm install -g)
  - Install topojson: ```npm install```
  - Install gdal: ```brew install gdal```

** MakeFile Setup
  - must use tabs not spaces
  - Edit, then: make Makefile
  - Run: make build/cb_2015_us_county_20m.zip

** Convert Shape file to GeoJSON
  - convert county shape file:
    ```ogr2ogr -f GeoJSON counties.json build/cb_2015_us_county_20m.shp```

** Clip Shape file to a bounding box

Get bounding box coordinates here:
https://www.flickr.com/places/info/2347563

Use Gdal to create a file with data for bounding box only:
```
ogr2ogr
-f GeoJSON // output format
  counties-clipped.json // output file
  build/cb_2015_us_county_20m.shp // input file
  -clipsrc -124.4096 32.5343 -114.1308 42.0095
```

Ctrl+C, Ctrl+V version:
```ogr2ogr -f GeoJSON counties-clipped.json build/cb_2015_us_county_20m.shp -clipsrc -124.4096 32.5343 -114.1308 42.0095```

** Filter Shape file to get the geometry we want to draw
  - Get the fips code: a 2 digit ID for the state: https://en.wikipedia.org/wiki/Federal_Information_Processing_Standard_state_code
```
ogr2ogr
  -f GeoJSON // output format
  counties-filtered.json // output file
  build/cb_2015_us_county_20m.shp // input file
  -where "STATEFP='06'" // filtering
```

Ctrl+C, Ctrl+V version:
```ogr2ogr -f GeoJSON counties-filtered.json build/cb_2015_us_county_20m.shp -where "STATEFP='06'"```


** Create topojson
- topojson
  -o topocounties.json  // output
  counties.json // input

** Add D3 Projection to topojson
```
topojson -o topo-counties-projected.json --projection='width = 960, height = 600, d3.geo.albersUsa().scale(1280).translate([width / 2, height / 2])' counties.json
```

** Simplify JSON
topojson -o topo-counties-projected.json --projection='width = 960, height = 600, d3.geo.albersUsa().scale(1280).translate([width / 2, height / 2])' --simplify=.5 counties.json

