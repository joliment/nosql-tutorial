<blockquote>
 {%= image_tag "/images/alan-kay.jpg", :alt => "[Alan Kay]" %}
 <p>
   Once you have something that grows faster than education grows,
   you’re always going to get a pop culture.
 </p>
 <p class="author">– Alan Kay</p>
</blockquote>

Referaty:

* [07.03.2012] **MapReduce** – Piotr Kamiński, Jacek Dzido, Olga Juhas
* [25.04.2012] **Hadoop** – Dawid Milewski, Marek Olszewski, Aleksandra Bucior
* [09.05.2012] **Wprowadzenie do Neo4j** – Adrian Modliszewski, Daniel Mielniczuk, Karol Bachewicz
* [16.05.2012] **Wizualizacja danych**, wprowadzenie do [d3.js](http://bost.ocks.org/mike/d3/workshop/) –
  Radosław Kwolek, Krzysztof Marzec, Rafał Ośko
* [23.05.2012] **Data is Messy**, [Google Refine](http://code.google.com/p/google-refine/),
  [Data Wrangler](http://vis.stanford.edu/wrangler/) – Marcin Brzeziński,  Łukasz Kędroń

## Co to jest „NoSQL”?

Termin „NoSQL” można rozszyfrować jako ***Not only SQL***. Carlo
Strozzi wprowadził ten termin w 1998 roku. W 2009 roku Eric Evans
użył terminu NoSQL w kontekście „the emergence of a
growing number of non-relational, distributed data stores”
([wikipedia](http://en.wikipedia.org/wiki/NoSQL)).

Więcej informacji o „NoSQL” zebrałem {%= link_to "tutaj", "/motywacja" %}.
Dodatkowo warto przeczytać artykuł Teda Newarda,
[The Vietnam of Computer Science](http://blogs.tedneward.com/2006/06/26/The+Vietnam+Of+Computer+Science.aspx).


## ElasticSearch

1. {%= link_to "Wyszukiwanie z ElasticSearch", "/elasticsearch" %}
1. {%= link_to "ElasticSearch w przykładach", "/elasticsearch-by-example" %}


## MongoDB

1. {%= link_to "Co to jest MongoDB?", "/mongodb" %}
1. {%= link_to "Interaktywna powłoka mongo", "/mongodb-shell" %}
1. {%= link_to "Język zapytań", "/mongodb-queries" %}
1. {%= link_to "Kopiowanie baz danych", "/mongodb-copydatabases" %}
1. {%= link_to "Sterowniki NodeJS i Ruby dla MongoDB", "/mongo" %}
1. {%= link_to "Agregacja danych", "/mongodb-aggregation" %}
1. {%= link_to "MapReduce w przykładach", "/mongodb-mapreduce" %}
1. {%= link_to "MapReduce Cookbook", "/mongodb-mapreduce-cookbook" %}

<!--
1. {%= link_to "Rails3 i MongoDB", "/mongodb-rails3" %}
1. {%= link_to "Masters & Slaves", "/mongodb-masters-slaves" %}
1. {%= link_to "Replikacja", "/mongodb-replikacja" %}
1. {%= link_to "Sharding", "/mongodb-sharding" %}
-->


<blockquote>
 <p>
   Humphry Davy zauważył, że gdy był pod wpływem gazu rozweselającego,
   przestał go boleć ząb mądrości. Niestety, nie wyciągnął
   logicznego wniosku, że należało wtedy wyrwać ten bolący ząb.
   Przez następne dwa pokolenia ludzie cierpieli na stołach
   operacyjnych. Znieczulenie po raz pierwszy zastosował
   dentysta Horace Wells dopiero w 1884 roku.
 </p>
 <p>
   Niektórzy uczeni twierdzą, że było to zarówno kulturalne jak i technologiczne
   zahamowanie. W końcu XVIII wieku nie dopuszczano nawet myśli
   o operacji bez bólu. Umiejętność radzenia sobie z bólem pacjenta
   (przede wszystkim przez szybkość dokonywania amputacji lub ekstrakcji)
   stanowiła zasadniczą część <b>umiejętności zawodowych</b>
   chirurga. Do rozpoczęcia bezbolesnych zabiegów potrzebna była
   <b>zmiana paradygmatu</b>.
 </p>
 <p class=""author">— S. Snow, Operations without pain</p>
</blockquote>

## CouchDB

1. {%= link_to "Oswajamy CouchDB", "/couchdb" %}
1. {%= link_to "Replikacja – jakie to proste!", "/couchdb-replication" %}
1. {%= link_to "Korzystamy z RESTFUL API", "/couchdb-crud" %}
1. {%= link_to "Funkcje Show", "/couchdb-show" %}
1. {%= link_to "NodeJS ← Couchapp + CouchClient + Cradle", "/node-couchapp" %}
1. {%= link_to "GeoCouch", "/couchdb-geo" %}
1. {%= link_to "Szablony Mustache w CouchDB", "/couchdb-mustache" %}
1. {%= link_to "Widok ≡ Map&#x200a;►Reduce (opcjonalnie)", "/couchdb-views" %}
1. {%= link_to "Generator przemówień i inne zastosowania…", "/couchdb-gp" %}
1. {%= link_to "Funkcje Lists", "/couchdb-lists" %}
1. {%= link_to "CouchDB & Ruby", "/couchdb-ruby" %}
1. {%= link_to "Rewrite – przepisywanie adresów URL", "/couchdb-rewrite" %}
1. {%= link_to "KansoJS framework dla CouchDB", "/couchdb-kansojs" %}
1. {%= link_to "Walidacja", "/couchdb-validation" %}
1. {%= link_to "ElasticSearch – odjazdowy „sweet spot”", "/couchdb-elasticsearch" %}

<!--

1. {%= link_to "CouchApp", "/couchdb-couchapp" %}
1. {%= link_to "Autentykacja", "/couchdb-authentication" %}
1. {%= link_to "Apache", "/couchdb-apache" %}

-->


## Redis

1. {%= link_to "Oswajamy bazę Redis", "/redis" %}


## Laboratorium

*  {%= link_to "Instalacja i konfiguracja baz: CouchDB, MongoDB, Redis oraz wyszukiwarki ElasticSearch", "/instalacja" %}
*  {%= link_to "Instalacja NodeJS i NPM", "/nodejs_npm" %}
*  {%= link_to "Zadania", "/zadania" %}


<blockquote>
 <p>
  In the relational databases world the data modeling process was
  mainly a single step activity: <b>design the schema based on
  normalization rules</b>. In the NoSQL world, designing the schema means
  <b>analyzing data access patterns</b>.
  Differently put the question shifted
  from <b>how do I store data</b> to
  <b>how will I access data</b>.
  </p>
  <p class="author">
  [<a href="http://nosql.mypopescu.com/post/5623952119/schema-design-in-schema-less-datastores">Schema Design in Schema-less Datastores</a>]
  </p>
</blockquote>

## Przykładowe bazy danych

* {%= link_to "Bazy CouchDB", "/couchdb-databases" %}
* {%= link_to "Kolekcje MongoDB", "/mongodb-databases" %}
* {%= link_to "Redis", "/redis-databases" %}

Raw data:

* [Raw Data na data.gov](https://explore.data.gov/catalog/raw/)


## Różne rzeczy

1. {%= link_to "Spidermonkey", "/couchdb-spidermonkey" %}
1. {%= link_to "Mustache – wąsate szablony", "/mustache" %}
1. {%= link_to "NodeJS", "/node" %}