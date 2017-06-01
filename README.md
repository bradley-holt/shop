# SHOP

## Quick Start

### Setup

##### Prerequisites

[Download and install Node.js.](https://nodejs.org/en/download/)

Install [polymer-cli](https://github.com/Polymer/polymer-cli):

    $ npm install -g polymer-cli

Install [bower](https://bower.io/):

    $ npm install -g bower

##### Setup

    $ npm install

### Start the development server

    $ npm start

## Tutorial

This app is a fork of the [Polymer Shop app](https://github.com/Polymer/shop). Shop is a demo Progressive Web App built using [Polymer App Toolbox](https://www.polymer-project.org/2.0/toolbox/). You can find out more in this [case study about the Shop app](https://www.polymer-project.org/2.0/toolbox/case-study). This tutorial will walk you through the steps necessary to transform the original Shop app into an Offline First app that uses [PouchDB](https://pouchdb.com/) (an open source JavaScript database that syncs) and [Hoodie](http://hood.ie/) (an open source backend framework for Offline First applications). If you simply want to try out this version of the Shop app then read the [Quick Start](#quick-start) section of this README.

### Table of Contents

* Key Concepts&nbsp;&nbsp;💡
* Initial Set Up&nbsp;&nbsp;⌨
* Install Hoodie ([diff](https://github.com/bradley-holt/shop/compare/upstream...bradley-holt:01-install-hoodie))&nbsp;&nbsp;🐶
* Configure Hoodie ([diff](https://github.com/bradley-holt/shop/compare/01-install-hoodie...bradley-holt:02-configure-hoodie
))&nbsp;&nbsp;🐶  ⚙
* Hoodie Store API ([diff](https://github.com/bradley-holt/shop/compare/02-configure-hoodie...bradley-holt:03-hoodie-store))&nbsp;&nbsp;🐶 🗃
* Hoodie Account API ([diff](https://github.com/bradley-holt/shop/compare/03-hoodie-store...bradley-holt:04-hoodie-account))&nbsp;&nbsp;🐶 👤
* Offline Sync&nbsp;&nbsp;🐶 🔄
* What's next?&nbsp;&nbsp;🤔

### Key Concepts&nbsp;&nbsp;💡

Some key concepts to understand before we get started:

* **Progressive Web Apps:** A Progressive Web App provides both the discoverability of a web app and the *reliable, fast, and engaging user experience* of a native mobile app. See [Pokedex.org](https://www.pokedex.org/) for a fun example of a Progressive Web App and check out [PWA Stats](https://www.pwastats.com/) for a community-driven list of stats and news related to Progressive Web Apps.
* **[Polymer](https://www.polymer-project.org/):** Libraries, tools, and patterns for *building Progressive Web Apps using web platform features* such as Web Components, Service Workers, and HTTP/2.
* **[Polymer App Toolbox](https://www.polymer-project.org/2.0/toolbox/):** Components, tools, and templates for *building Progressive Web Apps with Polymer*.
* **[The Polymer Shop App](https://github.com/Polymer/shop):** A demo Progressive Web App built using Polymer App Toolbox.
* **Offline First:** Progressive Web Apps must be Offline First in order to provide a reliable, fast, and engaging user experience *regardless of network connectivity*.
* **Service Workers:** Use the Cache API (part of the Service Workers specification) to make URL addressable *resources and content* available while offline.
* **IndexedDB:** Use IndexedDB or localForage (a polyfill that uses WebSQL or localStorage if IndexedDB is not supported) to make *application data* available while offline.
* **[PouchDB](https://pouchdb.com/):** An open source *JavaScript database that syncs* with anything that implements the CouchDB Replication Protocol.
* **[Apache CouchDB](http://couchdb.apache.org/):** An open source document database featuring an HTTP API, JSON documents, clustering capabilities for horizontal scalability, and *peer-to-peer replication*.
* **[IBM Cloudant](https://cloudant.com/):** A fully-managed database-as-a-service (DBaaS) *based on Apache CouchDB* with additional full text and geospatial search capabilities
* **[Hoodie](http://hood.ie/):** An open source *backend framework for Offline First applications*, leveraging Apache CouchDB on the server and PouchDB on the client

### Initial Set Up&nbsp;&nbsp;⌨

Check for Node.js version 4 or higher:

```
$ node -v
v7.10.0
```

Install Node.js if is not already installed:

* [Download Node.js](https://nodejs.org/en/download/) or
* [Install Node.js via a package manager](https://nodejs.org/en/download/package-manager/)

Install Bower:

```
$ npm install -g bower
```

Install Polymer CLI:

```
$ npm install -g polymer-cli
```

Install the Polymer Shop app:

```
$ git clone https://github.com/bradley-holt/shop.git
$ cd shop
$ git checkout upstream
$ bower install
```

Start the Polymer Development server:

```
$ polymer serve
info:    Files in this directory are available under the following URLs
      applications: http://127.0.0.1:8081
      reusable components: http://127.0.0.1:8081/components/shop/
```

You should now be able to browse to `http://127.0.0.1:8081` and see the Polymer Shop app. This is the original version of the Polymer Shop app. It uses Service Workers to store its resources and content for offline access. It uses IndexedDB to make application data available while offline. You can add, update, and remove items from your cart and this data will be stored locally within IndexedDB. In a later step we will replace IndexedDB with the Hoodie store, which is simply a wrapper for PouchDB. This will allow us to sync the shopping cart data with Apache CouchDB or IBM Cloudant when a user is signed in via Hoodie. 

### Install Hoodie&nbsp;&nbsp;🐶



### Configure Hoodie&nbsp;&nbsp;🐶  ⚙



### Hoodie Store API&nbsp;&nbsp;🐶 🗃



### Hoodie Account API&nbsp;&nbsp;🐶 👤



### Offline Sync&nbsp;&nbsp;🐶 🔄



### What's next?&nbsp;&nbsp;🤔


