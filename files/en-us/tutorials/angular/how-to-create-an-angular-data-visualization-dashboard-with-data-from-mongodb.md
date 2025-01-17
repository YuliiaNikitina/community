---
title: How to create an Angular data visualization dashboard with data from MongoDB - Complete Guide - Angular Tutorials | indepth.dev
author_name: Nikitina Yuliia
author_link: https://twitter.com/JuliiaNikitina
discussion_link:  https://github.com/indepth-dev/community/discussions/267
display_name: How to create an Angular data visualization dashboard with data from MongoDB - Complete Guide
---

# How to create an Angular data visualization dashboard with data from MongoDB

Choosing a stack for the project, especially a web project, is always quite a hard process for every developer. Stick with the one you are good and confident at? Or try a new promising framework but there is a chance it will just fail your expectations?

I decided to share with you part of my experiments with stacks in this tutorial. Usually, I work with data science projects that focus on data analytics and data visualizations. I mainly use the same frameworks and tools but sometimes I wonder if there is something more effective? 

So here is what I usually use for my web dashboards:
* Vue.js framework
* Node.js
* Flexmonster Pivot Table & Charts for calculations with data
* One of the following charting library depending on the the type of chart that I need: Highcharts or amCharts.

Last two libraries are not free but have a 30 days free trial.

This time I went through an interesting article about MEAN stack. **MEAN** stack is an acronym for **MongoDB**, **ExpressJS**, **AngularJS**, and **NodeJS**. More information you can check in this [web development with MEAN stack article](https://www.webdatarocks.com/blog/web-development-with-mean-stack/).

I had used Angular before, so I decided to give a try to this collection of JavaScript technologies. And it went pretty well!

My final stack looked like this:
* MongoDB
* Express.js
* Angular
* Node.js
* and I ended up leaving [Flexmonster](https://www.flexmonster.com/demos/angular/pivot-table/) and [Highcharts](https://www.highcharts.com/demo) in my list because these tools integrate perfectly with each other and all of the technologies mentioned above.

Another reason why I chose Flexmonster is that I can use Flexmonster CLI to conveniently create a project with just a few commands. CLIs will help us reduce the amount of t steps we need to complete for project creation. 

In this tutorial, we will create both, client and server. Client using Angular and reporting libraries and server using Express.js and Node.js.

![InDepthDev3](https://user-images.githubusercontent.com/66951194/179533387-c4703859-85fd-4153-bc3a-0f22e08c0174.gif)


## Phase 1. Create Angular project with embedded data visualization tools
Create an Angular app by running the following command in the console:

`ng new flexmonster-project && cd flexmonster-project`

Next, we will add Flexmonster. For that, first install Flexmonster CLI and then run following command:

`flexmonster add ng-flexmonster`
Now you have Flexmonster Angular wrapper in your project. Let's add the pivot grid to the main page.
To do it, you shall import several things:
1. `FlexmonsterPivotModule` into `src/app/app.module.ts`: 
```html
import { FlexmonsterPivotModule } from 'ng-flexmonster';

@NgModule({
    ...
    imports: [
        FlexmonsterPivotModule
        // other imports
    ],
    ...
})
```
2. Flexmonster styles (e.g., in `styles.css`):
```html
@import "flexmonster/flexmonster.min.css"
```
3.  and `FlexmonsterPivot` from `ng-flexmonster` (e.g., in `app.component.ts`):
```html

import { FlexmonsterPivot } from 'ng-flexmonster';
```
Now you can insert the `fm-pivot` directive (e.g., in `app.component.html`):
```html
<fm-pivot
    [report]="'https://cdn.flexmonster.com/reports/report.json'">
</fm-pivot>
```
The string in the report property links to a sample Flexmonster report Object in JSON file. It contains all the information about the future pivot table: how it will look, where the data will come from, which fields to show and how to display them. And it looks something like this:
```html
{
    "dataSource": {
        "type": "json",
        "filename": "https://cdn.flexmonster.com/data/retail-data.json",
        "mapping": {
            "Quantity": {
                "type": "number"
            },
            "Price": {
                "type": "number"
            },
            "Retail Category": {
                "type": "string"
            },
            "Sales": {
                "type": "number"
            },
            "Order Date": {
                "type": "year/quarter/month/day"
            },
            "Date": {
                "type": "date"
            },
            "Status": {
                "type": "string"
            },
            "Product Code": {
                "type": "string"
            },
            "Phone": {
                "type": "string"
            },
            "Country": {
                "type": "string",
                "folder": "Location"
            },
            "City": {
                "type": "string",
                "folder": "Location"
            },
            "CurrencyID": {
                "type": "property",
                "hierarchy": "Country"
            },
            "Contact Last Name": {
                "type": "string"
            },
            "Contact First Name": {
                "type": "string"
            },
            "Deal Size": {
                "type": "string"
            }
        }
    },
    "slice": {
        "rows": [
            {
                "uniqueName": "Country",
                "filter": {
                    "members": [
                        "country.[australia]",
                        "country.[usa]",
                        "country.[japan]"
                    ]
                }
            },
            {
                "uniqueName": "Status"
            }
        ],
        "columns": [
            {
                "uniqueName": "Order Date"
            },
            {
                "uniqueName": "[Measures]"
            }
        ],
        "measures": [
            {
                "uniqueName": "Price",
                "aggregation": "sum",
                "format": "-13w0a1w1c23j00"
            }
        ],
        "sorting": {
            "column": {
                "type": "desc",
                "tuple": [],
                "measure": {
                    "uniqueName": "Price",
                    "aggregation": "sum"
                }
            }
        },
        "expands": {
            "rows": [
                {
                    "tuple": [
                        "country.[japan]"
                    ]
                }
            ]
        },
        "drills": {
            "columns": [
                {
                    "tuple": [
                        "order date.[2019]"
                    ]
                }
            ]
        },
        "flatSort": [
            {
                "uniqueName": "Price",
                "sort": "desc"
            }
        ]
    },
    "conditions": [
        {
            "formula": "#value > 35000",
            "isTotal": true,
            "measure": "Price",
            "format": {
                "backgroundColor": "#00A45A",
                "color": "#FFFFFF",
                "fontFamily": "Arial",
                "fontSize": "12px"
            }
        },
        {
            "formula": "#value < 2000",
            "isTotal": false,
            "measure": "Price",
            "format": {
                "backgroundColor": "#df3800",
                "color": "#FFFFFF",
                "fontFamily": "Arial",
                "fontSize": "12px"
            }
        }
    ],
    "formats": [
        {
            "name": "-13w0a1w1c23j00",
            "thousandsSeparator": " ",
            "decimalSeparator": ".",
            "decimalPlaces": 0,
            "currencySymbol": "$",
            "positiveCurrencyFormat": "$1",
            "nullValue": "-",
            "textAlign": "right",
            "isPercent": false
        }
    ]
}
```
For our ease we can add a public pivotReport object in `app.component.ts` and change the link in the report property to this object. In this way you will be able to change the configuration of your gris on the flow. 

To learn more about all the properties and configuration options visit the [documentation with available tutorials](https://www.flexmonster.com/doc/available-tutorials-report/).

And we are done with the first phase. We have an Angular app with a pivot grid. You can run the project to check how everything works with their sample report.

## Phase 2. Setting up MongoDB Atlas and creating cluster
To communicate with Mongo, I will use Flexmonster MongoDB Connector - another feature that can help with binding the MEAN stack project together. One more advantage of using it is that all calculations are performed server-side, so we receive a reduced load on the browser’s memory.

Although, if you are not familiar with MongoDB and have troubles using it then there is a [tutorial on exploring the data in MongoDB Compass](https://www.freecodecamp.org/news/how-to-build-a-web-based-dashboard-with-django-mongodb-and-pivot-table/). Also you can visit the [MongoDB documentation page](https://www.mongodb.com/docs/manual/tutorial/getting-started/) as they have a convenient online shell where you can train working with the tool. If you have your data already ready and you have your connection string to the file, you can skip this phase and follow the next steps.

We will use MongoDb Atlas -  a cloud database service for applications. It will be easier for us and more convenient to work in the cloud and to connect to the database. All you need to start is to sign in your MongoDB account, create a project and a new cluster. It will take some time so don't worry.

After that you will need to whitelist your IP address and create a MongoDB user. Here you will be able to choose the connection option. Firstly go with MongoDB Shell and import the data you want to use in your database and visualize in the app. MongoDB Atlas will guide you through the full process. And it's completely free to explore and learn the tool and use it in test projects. 

After completing our database, we will go with a connection string for an application as we will use it further to connect to Flexmonster.

## Phase 3. Creating server using Express.js and connecting to MongoDB data source


Firstly, we will create a simple server on Express.js. 

Create a new folder for the server and generate the `package.json` file by running the `npm init` command in it.

Install express, cors, and body-parser packages via npm, as well as Flexmonster MongoDB Connector and the MongoDB driver that we will use in the next steps:
```
npm install express
npm install cors
npm install body-parser
npm install mongodb
npm install flexmonster-mongo-connector
```
The last thing you still have to do is to create a simple server in a `.ts` file (e.g., `server.ts`):
```ts
const express = require("express");
var cors = require("cors");
var bodyParser = require("body-parser");
const app = express();
app.use(cors());
app.use(bodyParser.json());
var port = 9204;
app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`)
});
```
For the request routing we need to create a separate module (e.g. `mongo.js` , we will do it the next step) and add it to the server:
```ts
app.use('/mongo', require('./mongo.js'));
```
That's all the configurations we need to set a server for this project. I will not dive deeper into the additional details of setting up a Node.js and Express environment because there are a lot of different options and they all depend on how you personally want your backend to work. Here is a good piece with [detailed instructions to Express the development environment](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/development_environment) if you want to deepen your knowledge.

So now we have the server, but it doesn’t work as we didn’t define a mongoController.js properly.

You will need to create this separate .ts controller file for the request routing. Firstly, install Flexmonster MongoDB Connector and the MongoDB driver in it and then configure the module itself:

```ts
//flexmonster-project/src/mongoController.ts
const mongo = require('express').Router();
const mongodb = require("mongodb");
const fm_mongo_connector = require("flexmonster-mongo-connector");
let dbo = null;
let _apiReference = null; // it'll be the Connector instance
 
// Define the config for the Connector
let config = {
    cacheEnabled: true,
    cacheMemoryLimit: 100,
    cacheTimeToLive: 120,
    logsEnabled: true
};

mongodb.MongoClient.connect(
    "your connection string", // given by MongoDb Atlas as a connection option
    {
        useNewUrlParser: true,
        useUnifiedTopology: true 
    }, 
    (err, db) => {
 	   if (err)
     	   throw err;
 	   dbo = db.db("your database name");
        // Pass the config to the Connector 
 	   _apiReference = new fm_mongo_connector.MongoDataAPI(config);
    }
);
// requests handling functions will appear here
module.exports = mongo;
```
Also don't forget to change the datasource in your report so it looks like this:
```html
public pivotReport = {
    dataSource: {
 	   type: "api",
 	   url: "http://localhost:9204/mongo", //the path to your API endpoints
 	  index: "your-collection-name"
    }
... 
}
```
Great! Now we can connect to MongoDB. But we will not understand what Flexmonster Pivot wants from us because we don’t have proper handlers for the requests. Let's fix that.

There are 4 request types we need to know how to handle while working with a pivot table: fields request, members request, select request, and optional handshake request.

The field request will help build a Field List in a pivot grid. With the members' request response Flexmonster will be able to select a string field for rows or columns and retrieve its members. When Flexmonster successfully receives the response to select request, a pivot table with the received data is shown. The “handshake” will help a client to understand if the custom data source API is the same for it and the server.

The implementation of this handlers shall look like this:

```html
mongo.post("/fields", async (req, res) => {
    try {
        const result = await _apiReference.getSchema(dbo, req.body.index);
        res.json(result);
    } catch (err) {
        //your error handler
    }
});

mongo.post("/members", async (req, res) => {
    try {
        const result = await _apiReference.getMembers(
            dbo,
            req.body.index, 
            { field: req.body.field }, 
            { page: req.body.page, pageToken: req.body.pageToken }
        );
        res.json(result);
    } catch (err) {
        //your error handler
    }
});

mongo.post("/select", async (req, res) => {
    try {
        const result = await _apiReference.getSelectResult(
            dbo, req.body.index, req.body.query,
            { page: req.body.page, pageToken: req.body.pageToken });
        res.json(result);
    } catch (err) {
      //your error handler
    }
});

mongo.post("/handshake", async (req, res) => {
    try {
    	res.json({ version: _apiReference.API_VERSION });
    } catch (err) {
     	handleError(err, res);
    }
});
```
For a better understanding you can check a [GitHub sample on how to connect to MongoDB with Flexmonster Pivot](https://github.com/flexmonster/pivot-mongo).

Now we can add the final touch on our dashboard - charts and graphs. 

## Phase 4. Adding charts to the reporting app

To add Highcharts to your analytical dashboard you will need to complete just a few steps because Flexmonster will work as a Connector between charts and data source so there is no need for their second connection. Basically the pivot will receive the data from the server, aggregate it and display it on the frontend.

So the pivot table will aggregate the data and pass it to the charting library that will draw the needed chart type. We need to create an extra container for it right after the pivot container in your .html file. Like this:
```html
<div>
    <fm-pivot #pivot [toolbar]="true" [shareReportConnection]="{url: 'https://olap.flexmonster.com:9500'}"
        [report]="'https://cdn.flexmonster.com/github/demo-report.json'" [height]="600"
        [licenseFilePath]="'https://cdn.flexmonster.com/jsfiddle.charts.key'" (reportcomplete)="onReportComplete()"
        (beforetoolbarcreated)="customizeToolbar($event)">
    </fm-pivot>
</div>

<div class="chart-container">
    <div id="highcharts-container"></div>
</div>

```
Then import Highcharts and the connector for tools communication:
```html
import * as Highcharts from 'highcharts';
// Importing Flexmonster's connector for Highcharts
import "flexmonster/lib/flexmonster.highcharts.js";
```
Then you will need to add the `reportcomplete` event (you can find it in the pivot parameters in the `.html` file); when it is triggered, the chart-drawing function is invoked. And of course the `drawChart()` function that will do the job.

```html
drawChart() {
        this.pivot.flexmonster.highcharts?.getData(
            {
                type: "area",
            },
            (data: Flexmonster.GetDataValueObject) => {
                Highcharts.chart('highcharts-container', <Highcharts.Options>data);
            },
            (data: Flexmonster.GetDataValueObject) => {
                Highcharts.chart('highcharts-container', <Highcharts.Options>data);
            }
        );
    }

    onReportComplete() {
        this.pivot.flexmonster.off("reportcomplete");
        this.drawChart();
    }
```
Again, there is a [sample project](https://github.com/flexmonster/pivot-angular/tree/master/src/app/examples/with-highcharts) with Highcharts integration so you can check yourself and the [GitHub repo with the whole project](https://github.com/YuliiaNikitina/MEANstackProject) to look through and get the overall picture. 

## Conclusion

That's it, our work results can be seen in the browser. Now you can build and run your MEAN stack data visualization app and see the results of your fruitful work. All these tools actually work pretty well with each other and form a great base to continue the development path further.



