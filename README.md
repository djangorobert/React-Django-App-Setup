Django and React
#In this Read Me I will help you figure out How to Start Using Pythons Web Framework Django with Javascripts Popular React Library.

#To Get Started 
#First we will set up the BACKEND with Django

#Lets Create a Virtualenv
virtualenv djangoreactenv

#Now cd into the new virtualenv
cd djangoreactenv

#Activate your Virtual Enviorment
.\scripts\activate

#Now lets use pythons pip to download the packages that will be needed for the setup.
pip install django
pip install django-webpack-loader==0.6.0

#Now lets create a Django Project
django-admin startproject djangoreact

#cd into the new Django Project

cd djangoproject

#Now lets run the python migrations command
python manage.py makemigrations

#Next we run the python migrate to setup the database # REMEMBER before you Run this command to go to the settings.py file to add the 
#Database of your choice this example I used the default sqlite 

python manage.py migrate

#Next lets createasuperuser this allows you to interact with the free django admin 
python manage.py createsuperuser

#Test to see if the web server works
python manage.py runserver


#GREAT

Now for the Front END

# Now Lets Setup React
#You can setitup in the djangoreact root area
# Run this npm Command to install the React Tool they use it to setup projects easier
npm install -g create-react-app


#Great Now lets create a react app
 create-react-app frontend
#Ive noticed that its just smart to call it Frontend since React will takeover the Frontend
#Now lets cd into it
cd frontend

#Now we will run eject so that we can customize the config file
npm run eject

#lets test to see if the react server is running
npm run start

# Now we need to install django-webpack-loader this is were the it mostly happens it will let the app know that the django server is now
# running the show and at the same time allowing React to take the lead in the frontend.
pip install django-webpack-loader

#now we need to go to django's setting.py add this in the settings.
WEBPACK_LOADER = {
    'DEFAULT': {
            'BUNDLE_DIR_NAME': 'bundles/',
            'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats.dev.json'),
        }
}

#Add webpack_loader in the Installed apps
webpack_loader

#While your in the settings setup the templates
TEMPLATES = [
    {
        # ... other settings
        'DIRS': [os.path.join(BASE_DIR, "templates"), ],
        # ... other settings
    },
]

#and for template page you can add something like this.
{% load render_bundle from webpack_loader %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <title>django react</title>
  </head>
  <body>
    <div id="app">
    </div>
      {% render_bundle 'main' %}
  </body>
</html>

#Now back to react install this package
 npm install webpack-bundle-tracker --save-dev
 
 #Then go to the config
 #frontend/config/paths.js add this
 
 module.exports = {
  // ... other values
  statsRoot: resolveApp('../'),
}


#Now add this in frontend/config/webpack.config.dev.js


const publicPath = 'http://localhost:3000/';
const publicUrl = 'http://localhost:3000/';

const BundleTracker = require('webpack-bundle-tracker');

module.exports = {
  entry: [
    // ... KEEP OTHER VALUES
    // this will be found near line 30 of the file
    require.resolve('webpack-dev-server/client') + '?http://localhost:3000',
    require.resolve('webpack/hot/dev-server'),
    // require.resolve('react-dev-utils/webpackHotDevClient'),
  ],
  plugins: [
    // this will be found near line 215-220 of the file.
    // ... other plugins
    new BundleTracker({path: paths.statsRoot, filename: 'webpack-stats.dev.json'}),
  ],
}

#Now we need to edit this file you can find it here  frontend/config/webpackDevServer.config.js now add this

headers: {
  'Access-Control-Allow-Origin': '*'
},


#Now you can add something in your APP.js to see if it works


import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Django React</h1>
        </header>
        <p className="App-intro">
            Django as Backend and React as Frontend Nice 
        </p>
      </div>
    );
  }
}

export default App;


