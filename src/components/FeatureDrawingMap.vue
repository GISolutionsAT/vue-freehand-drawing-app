<template>
  <div>
    <!-- Map div -->
    <div 
      id="drawing-map-container" 
    />
    
    <form class="drawing-form py-2">
      <div class="drawing-form-label">
        <h5>Select your Geometry type: </h5>
      </div>
      <div class="drawing-form-select">
        <select 
          class="form-control" 
          id="type"
        >
          <option value="LineString">
            LineString
          </option>
          <option value="Polygon">
            Polygon
          </option>
          <option value="Circle">
            Circle
          </option>
          <option value="None">
            None
          </option>
        </select>
      </div>
    </form>
  </div>
</template>

<script>
import 'ol/ol.css';
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import VectorLayer from 'ol/layer/Vector';
import VectorSource from 'ol/source/Vector';
import Stamen from 'ol/source/Stamen';
import Draw from 'ol/interaction/Draw';

export default {
  name: 'OpenlayersDrawingFeatureMap',
  data () {
    return {
      geo_types: [ 'LineString', 'Polygon', 'Circle',  'None' ]
    }
  },
  mounted () {
    const me = this;

    // Create Tile Layer
    const stamenTileLayer = new TileLayer({
        source: new Stamen({
          layer: 'toner'
        })
    });

    // Create Vector Source
    const vectorSource = new VectorSource({wrapX: false});

    // Create Vector Layer
    const vectorLayer = new VectorLayer({
      source: vectorSource
    })

    // Main Map
    me.map = new Map({
      target: 'drawing-map-container',
      layers: [stamenTileLayer, vectorLayer], 
      view: new View({
        center: [1822497, 6141544],
        zoom: 12
      })
    });

    // Select Type
    var typeSelect = document.getElementById('type');

    // Drawing Feature
    var draw;
    function addInteraction() {
    var value = typeSelect.value;
  
    if (value !== 'None') {
      draw = new Draw({
        source: vectorSource,
        type: value,
        freehand: true
      });
      me.map.addInteraction(draw);
    }
  }

    // Handle Change Event
    typeSelect.onchange = function() {
      me.map.removeInteraction(draw);
      addInteraction();
    };

    addInteraction();
  }
}
</script>

<style scoped>
  #drawing-map-container {
    height: 600px;
    width: 100%;
  }

  .drawing-form {
    background-color: #929292;
    color: #fff;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-items: center;
    padding: 2rem;
  }

  .drawing-form-label {
    flex: 1;
  }
  
  .drawing-form-select {
    flex: 2;
  }

  h5 {
    margin: 0;
  }

</style>