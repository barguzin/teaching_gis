# Running Spatial Queries on SQLite - Spatialite Stack 

This workshop will demonstrate some functionality of spatial querying in modern geodatabases. 

## Prerequisites

* QGIS installed on a computer 
* DBeaver Community Edition installed on a computer 

---

## Installing libspatial gui 

Follow instructions for Windows and Mac here: https://github.com/ucdavisdatalab/Spatial_SQL/blob/master/Install.md

---

## Getting Data 

We will be working with OpenStreetMap data compiled by Mateusz Ilba and available on [OSF page](https://osf.io/2ym95/). Please navigate to[this page](https://osf.io/2ym95/files/osfstorage) and download the file named *db_small.sqlite*. 

> Databases (for SQLite SpatiaLite) were created from publicly available OpenStreetMap data for Poland (https://www.openstreetmap.org/copyright). The db_small database comprises data for the area of the city of Kraków in the Małopolskie Province. The db_medium database comprises data from the entire Małopolskie Province. The db_large database, in addition to the Małopolskie Province, covers the Podkarpackie and Dolnośląskie Provinces. The db_v_large database covers the entire country. 

> This Spatialite database is for parallel processing of spatial queries, test databases queries, create new method for indexing and test programs based on spatial data.

> Available data tables (name_type):
> * buildings_points
> * landuse_polygons
> * pois_polygons
> * roads_lines
> * waterways_lines

> License: Open Data Commons Open Database License (ODbL) OpenStreetMap Foundation (OSMF). Please see license.txt file

---

## What is SQLite and Spatialite? 

> SQLite is a software library that provides a relational database management system. The lite in SQLite means lightweight in terms of setup, database administration, and required resources. SQLite has the following noticeable features: self-contained, serverless, zero-configuration, transactional. [SQLiteTutorial.net](https://www.sqlitetutorial.net/what-is-sqlite/)

> SQLite is an in-process library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine. The code for SQLite is in the public domain and is thus free for use for any purpose, commercial or private. SQLite is the most widely deployed database in the world with more applications than we can count, including several high-profile projects. [SQLite.org](https://www.sqlite.org/about.html)

### Why use SQLite? 

1. **SQLite is serverless** 
SQLite does not have a separate server process. SQLite reads and writes directly to ordinary disk files. A complete SQL database with multiple tables, indices, triggers, and views, is contained in a single disk file.
2. **SQLite is compact** 
SQLite is a compact library. With all features enabled, the library size can be less than 750KiB
3. **SQlite is self-contained**
SQLite is "stand-alone" or "self-contained" in the sense that it has very few dependencies. It runs on any operating system, even stripped-down bare-bones embedded operating systems. SQLite uses no external libraries or interfaces (other than a few standard C-library calls described below).
4. **SQLite is zero-configuration** 
SQLite does not need to be "installed" before it is used. There is no "setup" procedure. There is no server process that needs to be started, stopped, or configured.
5. **SQlite is transactional** 
A transactional database is one in which all changes and queries appear to be Atomic, Consistent, Isolated, and Durable (ACID). SQLite implements serializable transactions that are atomic, consistent, isolated, and durable, even if the transaction is interrupted by a program crash, an operating system crash, or a power failure to the computer. 

## What is SpatiaLite 

The official web page of the project is available on [gaia-gis.it](https://www.gaia-gis.it/gaia-sins/index.html)

> SpatiaLite is an open source library intended to extend the SQLite core to support fully fledged Spatial SQL capabilities. SQLite is intrinsically simple and lightweight (see above). 
> 
> SpatiaLite is smoothly integrated into SQLite to provide a complete and powerful Spatial DBMS (mostly OGC-SFS compliant). Using SQLite + SpatiaLite you can effectively deploy an alternative open source Spatial DBMS roughly equivalent to PostgreSQL + PostGIS. 

You can read more about SpatiaLite on the [official website](https://www.gaia-gis.it/fossil/libspatialite/index)

---

## Connecting the database 

### Connect db to DBeaver (Issues running spatial queries on Windows)

1. Database > New Database Connection 
2. Choose SQLite > Next > Path: Open (and navigate to your .sqlite file that you just saved in the previous steps)
3. Finish
4. Investigate entity relations using ER diagram 

### Connect db to QGIS

1. Open QGIS 
2. Open Data Source Manager > Choose SpatiaLite on the left pane > New and navigate to your .sqlite file that you just saved in the previous steps
3. Click Connect. Five features will appear. Select them all holding shift and clicking on all of them (OR do successively). 
4. We need some information on where this is located. Let's add a basemap. QGIS uses plugin to add this funtionality. 
   1. Navigate to Plugins > Manage and install plugins 
   2. Search for 'QuickMapServices' > Install. Close the menu after installation. 
   3. Navigate to Web > QuickMapServices > Settings > More Services > Get Contributed Pack. This will download more basemap providers. 
   4. Add the basemap to the map: Web > QuickMapServices > OSM > OSM Standard (or choose any other basemap with Krakov coverage)

---

## Running queries on db 

```sql
SELECT *
from buildings_points bp ;
```

### Check for textual representation of geometry 

```sql
select 
   ST_ASTEXT(Geometry) 
from 
   roads_lines;
```

> How is 'Geometry' stored? 

### Check for coordinate reference system 

```sql
select 
   ST_SRID(Geometry) 
from 
   roads_lines;
```

> What is the SRID number? Find the units of measurement and some other info on epsg.io

### Calculate distance of roads 

```sql
select 
	name, st_length(Geometry)
from roads_lines;
```

> Take this further and calculate top 10 lengthy road segments in Krakov. Hint: you will need to sort on the length and filter out records that do not have names! 

### Count the number of road segments by fclass

```sql
SELECT 
   fclass, count(osm_id) as cnt_segments
FROM 
   roads_lines
GROUP BY fclass; 
```

> Add the total length of all road segments to this table. *HINT: use ST_LENGTH() function*. 

> Calculate average *maxspeed* by fclass. 

> Calculate total number of bridges in data. 

> Calculate total number of tunnels in data (0) 

### Spatial Join 

```sql
SELECT 
	*
FROM roads_lines rl, (select * from landuse_polygon where osm_id='4519924') as lp
WHERE ST_INTERSECTS(rl.Geometry, lp.geometry);
```