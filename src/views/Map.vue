<template>
  <div class="map">
    <h1>Map</h1>
    <div id="map"></div>
    
    
    
    
  </div>
</template>

<style>
    body { margin:0; padding:0; }
    #map { position:absolute; height: 800px; top:10; bottom:10; left: 10; width:90%; }

    #marker {
    background-image: url('https://docs.mapbox.com/mapbox-gl-js/assets/washington-monument.jpg');
    background-size: cover;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    cursor: pointer;
    }
     
    .mapboxgl-popup {
    max-width: 200px;
    }

</style>

<script>
export default {
  data: function() {
    return {
      
    };
  },
  mounted: function() {
    mapboxgl.accessToken = 'pk.eyJ1IjoiYmprYW1wIiwiYSI6ImNqeGFseHA1cDE4MzQzdHJ2bnBjZTdsMTEifQ.BMXm7D46lgju3qldWBeDeQ';
    var monument = [-77.0353, 38.8895];
    var work = [-87.65, 41.99];
    var waterTower = [-87.62, 41.88];
    var places = [work, waterTower];
    var map = new mapboxgl.Map({
        container: 'map', // container id
        style: 'mapbox://styles/mapbox/streets-v11', // stylesheet location
        center: [-87.70, 41.94], // starting position [lng, lat]
        zoom: 10 // starting zoom
    });
    var mapboxClient = mapboxSdk({ accessToken: mapboxgl.accessToken });
    

   
    // create the popup



    var popupWork = new mapboxgl.Popup({ offset: 25 })
    .setText('Work');
    var popupWaterTower = new mapboxgl.Popup({ offset: 25 })
    .setText('Chicago Water Tower'); 

    var popup = [popupWork, popupWaterTower];

     
    // create DOM element for the marker
    var el1 = document.createElement('div');
    var el2 = document.createElement('div');
    el1.id = 'marker';
    el2.id = 'marker';
    var el = [el1, el2]

     
    // create the marker
    for (var i = 0; i < places.length; i++) {
      new mapboxgl.Marker(el[i])
      .setLngLat(places[i])
      .setPopup(popup[i]) // sets a popup on this marker
      .addTo(map);
    }
    
  },


  methods: {
   
  }
};


</script>
