
## ElasticSearch
Elasticsearch est un serveur utilisant Lucene pour l'indexation et la recherche des données. Il fournit un moteur de recherche distribué et multi-entité à travers une interface REST.

Le contenu de chaque document est indexé.
Le document a un type 
Les types sont contenu dans un index.

Fonctionnement de l'API REST 
Requête :
PUT : création ou modification d’un document 
GET : récupération d’un document 
HEAD : test si un document existe 
DELETE : suppression d’un document

Réponse : Code HTTP et une JSON (sauf pour le HEAD)

Indexation de la donnée
tous les données de chaque champ sont indexés.
concatenation des données sur une chaine stocké dans un champ appelé _all.

Lorsque un document est ajouté:
_index : Où le document est stocké.
_type : Représente le mapping entre les champs et leurs types.
_id : L’identificant unique du document.


### Installation d'ElasticSearch
[(https://www.elastic.co/fr/downloads/)](https://www.elastic.co/fr/downloads/)

Lancement d'ElasticSearch

    ./bin/elasticsearch
Verifier le status :

    curl 'http://localhost:9200/?pretty'

Réponse :

      {"name" : "osboxes",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "E8EdpiDwSs2GCW6b-HOy-g",
      "version" : {
        "number" : "7.6.2",
        "build_flavor" : "default",
        "build_type" : "tar",
        "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
        "build_date" : "2020-03-26T06:34:37.794943Z",
        "build_snapshot" : false,
        "lucene_version" : "8.4.0",
        "minimum_wire_compatibility_version" : "6.8.0",
        "minimum_index_compatibility_version" : "6.0.0-beta1"
      }
2 appels possible à Elastic Search via deux ports :
avec Java API sur le port 9300 et Restful API sur le port 9200

**PUT** 

    curl -XPUT 'localhost:9200/megacorp/employee/1' -H 'Content-Type: application/json' -d '{"first_name":"Nath","last_name":"Ngn"}'
’index "megacorp" avec comme type "employee" et avec l’id "1"
Reponse

    {"_index":"megacorp","_type":"employee","_id":"1","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":0,"_primary_term":1}

GET

    curl -XGET 'localhost :9200/ megacorp / employee /1? pretty '
    
Reponse
    {
      "_index" : "megacorp",
      "_type" : "employee",
      "_id" : "1",
      "_version" : 1,
      "_seq_no" : 0,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "first_name" : "Nath",
        "last_name" : "Ngn"
      }
    }
    
SEARCH

    curl -XGET 'localhost:9200/megacorp/employee/_search?pretty'
Renvoie tous documents dont le type est employee.

    curl -XGET 'localhost:9200/megacorp/employee/_search?q=last_name:Smith&pretty'
Recherche les documents de type employee dont le last_name est égale à smith

FILTER

    curl -XGET 'localhost:9200/megacorp/employee/_search?pretty' -d '{"query":{"filtered":{"filter": {"range":{"age":{"gt" : 30}}},"query":{"match":{"last_name":"smith"}}}}}'
   
 range age supérieur à 
 match egale à
 
[https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html)
	
Connaitre le type des champs

    curl -XGET 'localhost:9200/megacorp/_mapping/employee?include_type_name=true&pretty'

## Cluster

M
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2MDY0MTc2NiwtMTY1MTI4MDI4NywtMT
M5NDMxNDg1NywzMDU2NzcyMjAsLTE3MzE1NzM0NzcsLTEwNjc3
NzM5NiwxODQ2NjUzMjE3LDE1NzYyMDYyNjEsLTM4MjUwNzE2OV
19
-->