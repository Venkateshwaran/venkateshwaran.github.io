---
layout: post
title:  "Retrieve tracks from SoundCloud using NodeJS/ExpressJS!"
date:   2016-02-05 16:16:01 +0530
categories: APIs
---
A simple node app to retrieve soundcloud tracks using it's API.

Create a directory `mkdir soundcloudapp`

Change directory to the one you just created `cd soundcloudapp`

Create a node app `npm init`

(make sure you have `npm` installed on your computer. If not you can follow the instructions [here to install](https://www.npmjs.com/get-npm))

You will be prompted to generate the `package.json` file.
~~~
 {
  "name": "simplesoundcloudapiusage",
  "version": "1.0.0",
  "description": "Soundcloud API Using NodeJS/ExpressJS",
  "main": "index.js",
  "scripts": {
    "test": "nodemon index.js"
  },
  "keywords": [
    "SoundCloudAPI",
    "NodeJS",
    "ExpressJS"
  ],
  "author": "Venkateshwaran Selvaraj",
  "license": "ISC",
  "dependencies": {
    "express": "^4.15.2"
  }
}
~~~

Once that is done, create index.js file `nano index.js` (I am comfortable with nano editor)


~~~
var bodyParser = require('body-parser');
var express = require('express');
var request =  require('request');
var config = require('./config');
var app = module.exports = express();

//middleware
app.use(bodyParser.urlencoded({extended: true}));

var playlists = require('./routes/playlists');

//Routes
app.use('/playlists', playlists);

app.listen(config.port, function () {
  console.log('Soundcloud app listening on port:'+ config.port)
});

~~~

I have created a separate `config.js` file which includes the port number and the api keys

~~~
var conf = {
  soundcloud: {
    userID :'user_id',
    clientID : 'soundcloud_client_id',
    sortOrder : 'created_at',
  },
  port : 4000
};
module.exports = conf;

~~~

Now create the route file

`mkdir routes`

`nano playlists.js`

~~~
var express = require('express');
var request =  require('request');
var bodyparser = require('body-parser');
var config = require('../config').soundcloud;
var router = express.Router();
var base_url = 'http://api.soundcloud.com' + '/users/'+config.userID+'/playlists';
router.get('/', function(req,res){
  request(base_url+ '?client_id=' + config.clientID + '&order=' + config.sortOrder   , function(error, response, body) {
      if (!error && response.statusCode === 200) {
        res.json(JSON.parse(body));
      } else {
        res.json(error);
      }
    });
  });
module.exports = router;

~~~
