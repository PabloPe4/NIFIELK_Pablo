# NIFIELK_Pablo

### Submission NIFI-ELK

API URL

https://data.cityofnewyork.us/resource/erm2-nwe9.json

### Docker-compose

| NIFI | 8080 | http://localhost:8080 |
|:----:|------|------------------------|

| Elasticsearch | 9200 | http://localhost:9200 |
|:-------------:|------|-----------------------|

| Kibana | 5601 | http://localhost:5601 |
|:----:|------|------------------------|


### NIFI Flow

<img width="964" alt="Screenshot 2021-01-14 at 22 07 20" src="https://user-images.githubusercontent.com/71548024/104651379-d9528580-56b7-11eb-8fc4-a98e59e86276.png">

### KIBANA

In DevOps:

We first create a new index transforming the type of "location" as geo_point:

```
PUT innew311nyemergencies
{
  "mappings": {
      "properties": {
        "location": {
          "type": "geo_point"
        }
      }
    }
  }
  
```
We then copy the data from the ingested index to the new index:

```
POST _reindex
{
  "source": {
    "index": "311nyemergencies"
  },
  "dest": {
    "index": "innew311nyemergencies"
  },
  "script": {
    "source": "ctx._source.location = ['lat': ctx._source.latitude, 'lon': ctx._source.longitude];ctx._source.remove('latitude');ctx._source.remove('longitude'); "
  }
}
```
### Dashboard 

(NY_Dashboard in recent)

<img width="1161" alt="Screenshot 2021-01-10 at 17 10 25" src="https://user-images.githubusercontent.com/71548024/104128253-cc0b6300-5366-11eb-85d6-4a535c6e01e7.png">
