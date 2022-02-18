# Data Engineering Interview

One of the common things within Wanderu is the ingestion of partner data and merging it with Wanderu's own data set.

For this task you will be given 3 csv files:

* A 3rd party provided data dump of stations in France
* A subset of our `station` table
    * These values are used to filter search results
* A subset of our `station code` table
    * These values tie Wanderu stations to partner API codes

Your task will be to take the 3rd party data, upsert them into the `station` table and insert any `station code`s with the corresponding `sncf_id`.

The provided 3rd party data does not have any address information. You can use a geocoding service of your choice or a free account through [Geoapify](https://www.geoapify.com/) (limited to 3000 API calls/day).

### Additional notes

* Our existing tools are written in Python 3
* Pairing the 3rd party data with Wanderu's data can be done in code or using SQL.
* Your solution data set can be a csv or database dump.
    * We will want to see the final `station` and `station_code` data.
* The city ID (`wcityid`) for Lyon is `FRLYN`
* For new stations, you will need a new to generated a `wid`. To keep it simple, you can do something like: Take the friendly name, remove the vowels, prefix it with the country code and change the whole string to uppercase.

### Discussion

For the interview discussion, you should be prepared to discuss:

* Scaling your solution to several thousand stations.
* Detecting duplication or nearby stations.
* Other data integrity or completeness checks.

# Notes on the provided data

## 3rd party data
File: `partner_stations.csv`

This is raw data provided by a partner formatted as a CSV with a header.

## Station table 
File: `wanderu_station.csv`

This is a data dump from our database table that is used to store the station information and is used by our search algorithm.

* `wid` - Wanderu Station ID
* `loc` - The lat/lng stored in json
* `wcityid` - The (unique) Wanderu City ID
* `name` - The friendly name for the station
* `address` - The address in json
* `country` - The country-code

## Station code table 
File: `wanderu_station_code.csv`

This is a data dump from our database table that is used to store the relation between the Wanderu station ID and partner station ID.

* `wid` - Wanderu Station ID
* `partner_station_code` - Partner Station ID