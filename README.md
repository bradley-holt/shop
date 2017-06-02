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

In a terminal, check for Node.js version 4 or higher:

```
$ node -v
v7.10.0
```

**Note:** The `$` in the above instruction (and in all subsequent examples) indicates the start of a command prompt in a terminal. Do _not_ type the leading `$` into your command prompt. Multiple lines beginning with a `$` in subsequent instructions indicate the start of a new command (i.e. hit "enter" after the previous command and then type the new command). Subsequent lines _not_ beginning with a `$` in examples like the one above indicate output from the previous command. You should _not_ type these lines.

Install Node.js if it is not already installed:

* [Download Node.js](https://nodejs.org/en/download/) or
* [Install Node.js via a package manager](https://nodejs.org/en/download/package-manager/)

Install Bower:

```
$ npm install -g bower
```

**Note:** If the above command results in an `EACCES` error then read the documentation on [fixing npm permissions](https://docs.npmjs.com/getting-started/fixing-npm-permissions).

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

**Note:** As mentioned previously, the four `$` instances above indicate four separate commands for you to type (but do _not_ type the `$` at the beginning of each line), hitting enter after typing each of the four separate commands.

Start the Polymer Development server:

```
$ polymer serve
info:    Files in this directory are available under the following URLs
      applications: http://127.0.0.1:8081
      reusable components: http://127.0.0.1:8081/components/shop/
```

**Note:** As mentioned previously, subsequent lines _not_ beginning with a `$` in examples like the one above indicate output from the previous command. You should _not_ type these lines.

You should now be able to browse to `http://127.0.0.1:8081` in your web browser and see the Polymer Shop app. This is the original version of the Polymer Shop app. It uses Service Workers to store its resources and content for offline access. It uses IndexedDB to make application data available while offline. You can add, update, and remove items from your cart and this data will be stored locally within IndexedDB. In a later step we will replace IndexedDB with the Hoodie store, which is simply a wrapper for PouchDB. This will allow us to sync the shopping cart data with Apache CouchDB or IBM Cloudant when a user is signed in via Hoodie.

Close the browser tab containing the Shop app. Back in your terminal, use `Ctrl-C` to cancel the `polymer serve` command and return you to the command prompt.

### Install Hoodie&nbsp;&nbsp;🐶

Create a `package.json` file by running:

```
$ npm init
```

This will prompt you for the answers to a number of questions and then write a `package.json` for you. Here is what I provided for answers:

```
package name: (shop) 
version: (1.0.0) 1.1.2
description: 
entry point: (service-worker.js) index.js
test command: 
git repository: (https://github.com/bradley-holt/shop.git) 
keywords: 
license: (ISC) BSD-3-Clause
```

The parenthesis indicate the default options that were provided to me when I ran `npm init`. Most (if not all) of these options will have no impact on the successful completion of this tutorial. The most important consideration is ensuring that the `entry point` option is _not_ set to the default `service-worker.js` value but rather to `index.js`.

Take a look at my [`package.json`](https://github.com/bradley-holt/shop/commit/bef280159280e35e53d8c4ccb6c309755f2da8d2#diff-b9cfc7f2cdf78a7f4b91a753d10865a2) file that was generated after running `npm init` for reference and compare it against your newly-generated `package.json` file.

**Note:** When viewing and editing files, you will want to use a text editor such as [Sublime Text](https://www.sublimetext.com/) or [Atom](https://atom.io/).

Next you may want to add `node_modules` to your `.gitignore` file. See the diff for my [`.gitignore`](https://github.com/bradley-holt/shop/commit/bb6b4a54657953d3250da882e96ad8054ee773bd#diff-a084b794bc0759e7a6b77810e01874f2) file after this step for reference.

Next we will install Hoodie through npm (the `--save` option will add Hoodie to your `package.json` file as a dependency):

```
$ npm install hoodie --save
```

See the diff for my [`package.json`](https://github.com/bradley-holt/shop/commit/06e12744e90a8fcecec410f268ffbc7aad2af66b#diff-b9cfc7f2cdf78a7f4b91a753d10865a2) file after running this command for reference. You should see a new `start` script with a value of `hoodie` and you should see `hoodie` listed under `dependencies`.

Now let's test that Hoodie has been installed correctly:

```
$ npm start
> shop@1.1.2 start /Users/bradleydholt/shop
> hoodie
🐶  Your Hoodie app has started on: http://localhost:8080
Stop server with control + c
```

Select and copy the `http://localhost:8080` text from the above output to your clipboard (it's possible, but very unlikely, that your output will include a slightly different URL so you should copy from _your_ terminal output rather than from this tutorial). Open up your web browser, paste this URL into the address bar, and hit "enter". You should now see the Hoodie welcome screen with the text, "Well done, you made it!" and a cute dog illustration.

![The Low-Profile Dog Hoodie Mascot](https://avatars1.githubusercontent.com/u/1888826?v=3&s=100)

**Note:** Consider testing your work in [Google Chrome](https://www.google.com/chrome/) as Chrome tends to have good support for web platform features used by Progressive Web Apps, plus Chrome has several useful [developer tools](https://developer.chrome.com/devtools).

Close the browser tab containing the Hoodie welcome screen. Back in your terminal, use `Ctrl-C` to cancel the `npm start` command and return you to the command prompt.

Next you may want to add the `.hoodie` directory to your `.gitignore` file. See the diff for my [`.gitignore`](https://github.com/bradley-holt/shop/commit/f9bc49efedabfcf55d02a7d023e8949b2440bb0c#diff-a084b794bc0759e7a6b77810e01874f2) file after this step for reference. The `.hoodie` directory is automatically created by Hoodie and is where Hoodie stores app data if Hoodie is not configured to use an Apache CouchDB or IBM Cloudant backend database.

Previously in this tutorial when the Shop app was served using Polymer App Toolbox (the `polymer serve` command), the Shop app was only a client app. Now that we are using Hoodie, we are going to now have a server component as well. This means that we will need to move all of our frontend code into a `public` directory that we will create.

The first step is to delete the `app.yaml` file ([diff](https://github.com/bradley-holt/shop/commit/b10883314f1fae08dedb75a8fd98528e0ccde269) for reference) since this is a file used for serving apps on Google App Engine, which we will not be using:

```
$ rm app.yaml
```

Then move all of the frontend files into a new `public` directory ([diff](https://github.com/bradley-holt/shop/commit/45dca35b4fc1b6a54d1ed8c53f5f51bcd163ff31) for reference):

```
$ mkdir public
$ mv bower.json bower_components data images index.html manifest.json polymer.json service-worker.js src sw-precache-config.js test public
```

Here is a directory tree for reference (there is nothing for you to type here, this is for reference purposes only):

```
.
├── README.md
├── node_modules
├── package.json
└── public
    ├── bower.json
    ├── bower_components
    ├── data
    ├── images
    ├── index.html
    ├── manifest.json
    ├── polymer.json
    ├── service-worker.js
    ├── src
    ├── sw-precache-config.js
    └── test
```

Start Hoodie again:

```
$ npm start
> shop@1.1.2 start /Users/bradleydholt/shop
> hoodie
🐶  Your Hoodie app has started on: http://localhost:8080
Stop server with control + c
```

Open the Hoodie URL in your web browser. You should now see the Shop app, rather than the Hoodie welcome screen. At this point the frontend Shop app is identical to the Shop app that you saw when you ran `polymer serve`. The only difference is that the Shop app is now being served by Hoodie rather than by Polymer App Toolbox. In subsequent steps we will replace the use of IndexedDB within the Shop app with the Hoodie store, which is simply a wrapper for PouchDB. This will allow us to sync the shopping cart data with Apache CouchDB or IBM Cloudant when a user is signed in via Hoodie.

Close the browser tab containing the Hoodie welcome screen. Back in your terminal, use `Ctrl-C` to cancel the `npm start` command and return you to the command prompt.

Optionally, update the `README.md` file to remove outdated sections. See the diff for my [`README.md`](https://github.com/bradley-holt/shop/commit/5ccfd76db827947b59dd5034a87cabff33746569#diff-04c6e90faac2675aa89e2176d2eec7d8) file for reference on what to remove.

Optionally, you may want to remove `bower_components` and `build` from your `.gitignore` file, create a new `public/.gitignore` file, and add `bower_components` and `build` to this new `.gitignore` file. See the diff for my [`.gitignore` and `public/.gitignore`](https://github.com/bradley-holt/shop/commit/bcf1c8bb06fc14fd06b74e8e1ba4dfef78375d59) files after this step for reference.

**Note:** A forward slash (`/`) in a file reference indicates that the file or directory following the forward slash is within the preceding directory. For example, `public/.gitignore` means that the `.gitignore` file is within the `public` directory.

For convenience, set up an automated Bower install step within the npm install process. This can be done by opening `package.json` in your text editor and adding a `postinstall` script. Here is the relevant section after adding this line:

```
   "scripts": {
     "postinstall": "cd public && bower install",
     "test": "echo \"Error: no test specified\" && exit 1",
     "start": "hoodie"
   },
```

For reference, take a look at the diff for my  [`package.json`](https://github.com/bradley-holt/shop/commit/68a5598933be1c1fe19017c88632fcc5ca080e32#diff-b9cfc7f2cdf78a7f4b91a753d10865a2) file after this step.

Finally, let's test that `npm install` works correctly: 

```
$ npm install
> shop@1.1.2 postinstall /Users/bradleydholt/shop
> cd public && bower install
```

You should see only the output above and no error messages (though your output will look slightly different).

Optionally, provided updated instructions in the `README.md` file. See the diff for my [`README.md`](https://github.com/bradley-holt/shop/commit/936c11bc706be425a878cf3445ed1386deea6d35#diff-04c6e90faac2675aa89e2176d2eec7d8) file for reference on what to change.

### Configure Hoodie&nbsp;&nbsp;🐶  ⚙

Hoodie can be configured through command line arguments, environment variables, a `.hoodierc` file, and/or a `hoodie` key in `package.json`. See the [configuration section of the Hoodie documentation](http://docs.hood.ie/en/latest/guides/configuration.html) for a full list of Hoodie configuration options.

Setting the `adminPassword` Hoodie configuration option will allow you to login to the Hoodie admin dashboard where you can create, view, and edit user accounts. Create a `.hoodierc` file and add the following contents using your text editor (changing `password` to any value you would like to use for a password):

```
{
  "adminPassword": "password"
}
```

Optionally, you may want to add the `.hoodierc` file to your `.gitignore` file as `.hoodierc` is a file that you want to ensure is _not_ committed to your repository since it contains sensitive configuration values. See the diff for my [`.gitignore`](https://github.com/bradley-holt/shop/commit/a623b5d7cd6caf8749fbafd0218a5be7c2405e97#diff-a084b794bc0759e7a6b77810e01874f2) file after this step for reference.

By default Hoodie will store its data in PouchDB on the server side. This is very convenient for experimenting with Hoodie and for development. However, this is not recommended for production. Instead, use either Apache CouchDB or IBM Cloudant for production deployments. You can also Apache CouchDB, IBM Cloudant, or the [Cloudant Developer Edition](https://hub.docker.com/r/ibmcom/cloudant-developer/) for local development.

#### Optional: Configure a Database for Local Development

##### Apache CouchDB

[Install CouchDB 2.0](http://docs.couchdb.org/en/2.0.0/install/index.html). Instructions are available for installing CouchDB 2.0 on Unix-like systems, on Windows, on Mac OS X, and on FreeBSD.

Configure CouchDB for a [single-node setup](http://docs.couchdb.org/en/2.0.0/install/index.html#single-node-setup), as opposed to a cluster setup. Once you have finished setting up CouchDB, you should be able to access CouchDB at `http://127.0.0.1:5984/`. Ensure that CouchDB is running and take note of your admin username and password.

Configure Hoodie to use your CouchDB instance. The easiest way to do this is by adding a `dbUrl` configuration option to your `.hoodierc` file, leaving the `.hoodierc` file looking something like (replacing `admin` and `password` with your CouchDB admin username and password, respectively):

```
{
  "adminPassword": "password",
  "dbUrl": "http://admin:password@127.0.0.1:5984/"
}
```

##### IBM Cloudant

Sign up for an [IBM Bluemix](https://console.ng.bluemix.net/) account, if you do not already have one.

Once you are logged in to Bluemix, create a new Cloudant instance on the [Cloudant NoSQL DB Bluemix Catalog](https://console.ng.bluemix.net/catalog/services/cloudant-nosql-db) page. This should take you to a page representing the newly-created service instance. Click the "service credentials" link. You should have one set of service credentials listed. Click "view credentials" which should show you a JSON object containing your service credentials. Copy the value for the `url` key to your clipboard (the value will be in the form of `https://username:password@uniqueid-bluemix.cloudant.com`).

Configure Hoodie to use your Cloudant instance. The easiest way to do this is by adding a `dbUrl` configuration option to your `.hoodierc` file, leaving the `.hoodierc` file looking something like (replacing `https://username:password@uniqueid-bluemix.cloudant.com` with the value you copied to your clipboard in the previous step):

```
{
  "adminPassword": "password",
  "dbUrl": "https://username:password@uniqueid-bluemix.cloudant.com"
}
```

### Hoodie Store API&nbsp;&nbsp;🐶 🗃



### Hoodie Account API&nbsp;&nbsp;🐶 👤



### Offline Sync&nbsp;&nbsp;🐶 🔄



### What's next?&nbsp;&nbsp;🤔


