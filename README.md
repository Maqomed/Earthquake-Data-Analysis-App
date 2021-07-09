# Project Name
> EDA-EARTHQUAKE DATA ANALYSIS

# Project Is Deployed on Netlify
https://earthquakedataanalysis.netlify.app/

## Table of Contents
* [General Info](#general-information)
* [Technologies Used](#technologies-used)
* [Features](#features)
* [Setup](#setup)

<!-- * [License](#license) -->


## General Information
The EDA web app gets information about earthquakes around the world. We gather information about the earthquakes from the United States Geological Survey website. With this app, users can see all the earthquakes that happened during the day on the map provided by the app.


## Technologies Used
- VueJS - version 2.6.13
- Mapbox - version 2.3.0
- Bootstrap - version 5.0.1
- Axios - 0.21.1


## Features
- When the page is loaded, users can see the places where the earthquakes happened. These places are marked on the map. By clicking the markers, users can get more additional information about the earthquake, such as the name of the region, time, magnitude, and depth of the earthquake.
- On the left side of the screen, users can see the earthquakes with the highest and the lowest magnitudes, along with the information about the region, time, and magnitude of the earthquake
- Users can show the places with the highest and lowest magnitudes on the map by clicking the show on map button on the respective card
- The filter button is also available from where users can filter the earthquakes based on the magnitudes. Users are provided with three choices. Users can show on the map earthquakes independent of the magnitude, the earthquakes with a magnitude more than 2.5, and earthquakes with a magnitude less than 2.5.
- Places where earthquakes are happened marked with markers of different colors depending on the earthquake's magnitude.

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
