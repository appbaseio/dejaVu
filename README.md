dejavu: The Missing Web UI for Elasticsearch
====

[![GitHub License](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/appbaseio/dejavu/dev/LICENSE.md)  ![React Version](https://img.shields.io/badge/react-v15.6-brightgreen.svg)
[![Code Climate](https://codeclimate.com/github/appbaseio/dejaVu/badges/gpa.svg)](https://codeclimate.com/github/appbaseio/dejaVu)

1. **[Dejavu: Intro](#1-dejavu-intro)**   
2. **[Features](#2-features)**  
  a. [Filter Views](#filter-views)  
  b. [Modern UI Elements](#modern-ui-elements)  
  c. [Realtime Data Updates](#realtime-data-updates)  
  d. [Importing JSON or CSV Data](#importing-json-or-csv-data)  
3. **[Comparison](#3-comparison-with-other-data-browsers)**
4. **[Roadmap](#4-roadmap)**
5. **[Build Locally](#5-build-locally)**  
6. **[Get Dejavu](#6-get-dejavu)**  
  a. [Docker Installation](#docker-installation)  
  b. [Hosted Alternatives](#hosted-alternatives)

---

### 1. Dejavu Intro

**dejavu** is the missing web UI for Elasticsearch. Existing web UIs leave much to be desired or are built with server side page rendering techniques (I am looking at you, Kibana).

Thus started the journey of dejavu, with a goal to build a modern Web UI (no page reloads, infinite scroll, filtered views, realtime updates) with 100% client side rendering. It is available today as a [hosted app](https://opensource.appbase.io/dejavu/live), [chrome extension](https://chrome.google.com/webstore/detail/dejavu/jopjeaiilkcibeohjdmejhoifenbnmlh) and as a [docker image](https://hub.docker.com/r/appbaseio/dejavu/).

Starting version `1.0.0`, dejavu is the only web UI that supports importing data via JSON and CSV files.

---

### 2. Features

#### Filter Views

![Filter Views](http://i.imgur.com/sE90O10.gif)

Sort through the data, find things visually, hide irrelevant data and make sense of all the numbers and dates. Filters work by identifying data mappings from the Elasticsearch index. If dejavu sees a ``string`` field, it will provide filters for **search**, **has** and **has not** and is also mindful if the data is analyzed. Similarly a numeric field allows filtering on ranges and a date field allows filtering data by dates.

dejavu also supports local filters like column sorting and showing a subset of columns. 

dejavu also supports query filtered view, where a user can write an Elasticsearch query and the data view will be filtered according to the query.

#### Modern UI elements

![Pagination](http://i.imgur.com/IAX0kLX.gif)

It's not uncommon to have thousands of records in a type. dejavu supports an infinite scroll based UI, pagination is so old school.

dejavu also supports browsing data from multiple types and bulk deletions. It also let's you add new records and update existing records.

#### Realtime Data Updates

![Realtime data updates](http://i.imgur.com/z0Ey4BN.gif)

dejavu uses a websockets based API and subscribes for data changes for the current filtered view. For this to work, the Elasticsearch server needs to support a websockets based publish API. Currently, you can take advantage of this feature by hosting your data with appbase.io.

#### Importing JSON or CSV Data

![Importing JSON or CSV files](https://i.imgur.com/qro6e9Q.gif)


This newly supported feature allows importing CSV or JSON data directly into Elasticsearch through a guided data mappings configuration.

---

### 3. Comparison with other data browsers

|     Features     |                                                    dejavu                                                    | ES-head | ES-kopf | ES-browser |                                 Kibana                                |
|:----------------:|:------------------------------------------------------------------------------------------------------------:|:------------------:|:------------------:|:---------------------:|:---------------------------------------------------------------------:|
| Installation     | Chrome extension, Docker Image, Hosted App.      | Elasticsearch plugin, static page | Elasticsearch plugin, static page  | Elasticsearch plugin (doesn't work with v2.0 and above) | Elasticsearch plugin |
| Modern UI        | Built with React v15.6.0, uses a live-reload interface.                                                  | Built with jQuery v1.6.1, slightly stodgy | Built with Angular 1.x | Built with ExtJs, but a bit stodgy | Built with Node.JS, Hapi, Jade                                                                     |
| Browser features | CRUD with support for data filters. | Read data with support for full-text search. | No data view |  Data view support for a single type | Read view with support for visualizations / charting |
| Data Import/Export | Yes, in JSON and CSV formats. | - | - |  - | Only export is supported, no CSV support. |
| Open Source      | MIT license                                                                          |  Apache v2.0               |       MIT license             |   Apache v2.0                    |   Apache v2.0                                                                    |


---

### 4. Roadmap


<s>Here's a rough roadmap of things to come in the version ``1.0.0`` release.</s>

:fireworks: We just hit the 1.0.0 roadmap.

- [x] Battle-testing with different datasets
- [x] Feature support for advanced filtering   
<s>Offline detection and reconnection for realtime updates</s>
- [x] Performance improvements while scrolling
- [x] Support for importing and exporting data
- [x] Support for a continuous query view
- [x] Available as a docker image

Roadmap for version `2.0.0` release, subject to change:

- [ ] An intuitive data editing experience in tabular mode (v/s JSON edit mode)
- [ ] View data types from within the data browser view
- [ ] A more streamlined import process
- [ ] Create a test coverage suite

---

### 5. Build Locally

`dev` branch is the bleeding edge version of dejavu, all new changes go here.

`chrome-extension` branch is where we make chrome extension related changes.

`gh-pages` branch is for the hosted app, as well as for the version that runs on https://dashboard.appbase.io.


#### Local Installation

1. git clone https://github.com/appbaseio/dejavu
2. git checkout master
3. npm install
4. bower install
5. npm start (open dejavu in the browser on http://localhost:1358)

#### Local Build

#### `chrome-extension` branch: Chrome extension

```sh
$ npm run build_chrome_extension
```

#### `gh-pages` branch: Github hosted pages

```sh
$ npm run build_gh_pages
```

#### How to Contribute

The source code is under the ``live/src`` directory. You can make pull requests against the ``dev`` branch.

---

### 6. Get Dejavu

#### Docker Installation


```
docker run -p 1358:1358 -d appbaseio/dejavu
open http://localhost:1358/live
```   

You can also run a specific version of **dejavu** by specifying a tag. For example, version `1.0.0` can be used by specifying the ``docker run -p 1358:1358 appbaseio/dejavu:1.0.0`` command.

##### CORS

To make sure you enable CORS settings for your ElasticSearch instance, add the following lines in the ES configuration file.

```sh
http.port: 9200
http.cors.allow-origin: "http://localhost:1358"
http.cors.enabled: true
http.cors.allow-headers : X-Requested-With,X-Auth-Token,Content-Type,Content-Length,Authorization
http.cors.allow-credentials: true
```

If you are running your Elasticsearch with docker, you can use flags to pass the custom CORS configuration.

###### Elasticsearch v2.x

`docker run --name es -d -p 9200:9200 elasticsearch:2 -Des.http.port=9200 -Des.http.cors.allow-origin="http://localhost:1358" -Des.http.cors.enabled=true -Des.http.cors.allow-headers=X-Requested-With,X-Auth-Token,Content-Type,Content-Length,Authorization -Des.http.cors.allow-credentials=true`

###### Elasticsearch v5.x

`docker run --name es -d -p 9200:9200 -d elasticsearch:5 -E http.port=9200 -E http.cors.allow-origin="http://localhost:1358" -E http.cors.enabled=true -E http.cors.allow-headers=X-Requested-With,X-Auth-Token,Content-Type,Content-Length,Authorization -E http.cors.allow-credentials=true`


#### Hosted Alternatives

can also be run via hosted app at https://opensource.appbase.io/dejavu/live or [installed as a chrome extension](https://chrome.google.com/webstore/detail/dejavu/jopjeaiilkcibeohjdmejhoifenbnmlh).


For example: If you are using the chrome-extension instead of docker image, the `http.cors.allow-origin` in Elasticsearch.yml file would change accordingly:

```sh
http.port: 9200
http.cors.allow-origin: "chrome-extension://jopjeaiilkcibeohjdmejhoifenbnmlh"
http.cors.enabled: true
http.cors.allow-headers : X-Requested-With,X-Auth-Token,Content-Type,Content-Length,Authorization
http.cors.allow-credentials: true
```

---

