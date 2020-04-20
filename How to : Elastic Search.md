---


---

<h2 id="elasticsearch">ElasticSearch</h2>
<p>Elasticsearch est un serveur utilisant Lucene pour l’indexation et la recherche des données. Il fournit un moteur de recherche distribué et multi-entité à travers une interface REST.</p>
<p>Le contenu de chaque document est indexé.<br>
Le document a un type<br>
Les types sont contenu dans un index.</p>
<p>Fonctionnement de l’API REST<br>
Requête :<br>
PUT : création ou modification d’un document<br>
GET : récupération d’un document<br>
HEAD : test si un document existe<br>
DELETE : suppression d’un document</p>
<p>Réponse : Code HTTP et une JSON (sauf pour le HEAD)</p>
<p>Indexation de la donnée<br>
tous les données de chaque champ sont indexés.<br>
concatenation des données sur une chaine stocké dans un champ appelé _all.</p>
<p>Lorsque un document est ajouté:<br>
_index : Où le document est stocké.<br>
_type : Représente le mapping entre les champs et leurs types.<br>
_id : L’identificant unique du document.</p>
<h3 id="installation-delasticsearch">Installation d’ElasticSearch</h3>
<p><a href="https://www.elastic.co/fr/downloads/">(https://www.elastic.co/fr/downloads/)</a></p>
<p>Lancement d’ElasticSearch</p>
<pre><code>./bin/elasticsearch
</code></pre>
<p>Verifier le status :</p>
<pre><code>curl 'http://localhost:9200/?pretty'
</code></pre>
<p>Réponse :</p>
<pre><code>  {"name" : "osboxes",
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
</code></pre>
<p>2 appels possible à Elastic Search via deux ports :<br>
avec Java API sur le port 9300 et Restful API sur le port 9200</p>
<p><strong>PUT</strong></p>
<pre><code>curl -XPUT 'localhost:9200/megacorp/employee/1' -H 'Content-Type: application/json' -d '{"first_name":"Nath","last_name":"Ngn"}'
</code></pre>
<p>’index “megacorp” avec comme type “employee” et avec l’id “1”<br>
Reponse</p>
<pre><code>{"_index":"megacorp","_type":"employee","_id":"1","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":0,"_primary_term":1}
</code></pre>
<p>GET</p>
<pre><code>curl -XGET 'localhost:9200/megacorp/employee/1? pretty '

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
</code></pre>
<p>SEARCH</p>
<pre><code>curl -XGET 'localhost:9200/megacorp/employee/_search?pretty'
</code></pre>
<p>Renvoie tous documents dont le type est employee.</p>
<pre><code>curl -XGET 'localhost:9200/megacorp/employee/_search?q=last_name:Smith&amp;pretty'
</code></pre>
<p>Recherche les documents de type employee dont le last_name est égale à smith</p>
<p>FILTER</p>
<pre><code>curl -XGET 'localhost:9200/megacorp/employee/_search?pretty' -d '{"query":{"filtered":{"filter": {"range":{"age":{"gt" : 30}}},"query":{"match":{"last_name":"smith"}}}}}'
</code></pre>
<p>range age supérieur à<br>
match egale à</p>
<p><a href="https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html">https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html</a></p>
<p>Connaitre le type des champs</p>
<pre><code>curl -XGET 'localhost:9200/megacorp/_mapping/employee?include_type_name=true&amp;pretty'
</code></pre>
<h2 id="cluster">Cluster</h2>
<p><a href="https://2014.rmll.info/slides/343/rmll2014_elasticsearch_rpignolet.pdf">https://2014.rmll.info/slides/343/rmll2014_elasticsearch_rpignolet.pdf</a></p>
<p><a href="https://markheath.net/post/exploring-elasticsearch-with-docker">https://markheath.net/post/exploring-elasticsearch-with-docker</a></p>
<p>curl -XGET ‘localhost:9200/test-index/_doc/1?pretty’</p>
<p>Avec la lib elastic</p>
<pre><code>res = es.index(index="megacorp", id=1,doc_type="employee", body=doc)
</code></pre>
<p>Il faut que</p>

