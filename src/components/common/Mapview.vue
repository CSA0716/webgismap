<template>
    <div class="mapview-pannel">
    <div id="mapview"></div>
    <div id="basemapToggle"></div>
    <div id="scaleBar"></div>
    <div id="zoom"></div>
</div>
  </template>
  
  <script>
  
 import { loadModules }  from 'esri-loader';  
 const options = {
    url:'https://js.arcgis.com/4.26/init.js',
    css:'https://js.arcgis.com/4.26/esri/themes/light/main.css',
 };
  export default {
    name: 'Mapview',
    components: {},
    mounted:function() {
        //生命周期
        console.log(this.$store.state._defaultView)
        this._createMapView();
        
    },
    methods: {
       async _createMapView() {
            const [Map, MapView,Basemap,TileLayer,BasemapToggle,ScaleBar,Zoom] = await loadModules(
                ['esri/Map',
                 'esri/views/MapView',
                 'esri/Basemap',
                 'esri/layers/TileLayer',
                 'esri/widgets/BasemapToggle',
                 'esri/widgets/ScaleBar',
                 'esri/widgets/Zoom'
                ],
                options
            );
            let basemap = new Basemap({
  baseLayers: [
    new TileLayer({
      url: "http://map.geoq.cn/arcgis/rest/services/ChinaOnlineStreetPurplishBlue/MapServer",
      title: "Basemap"
    })
  ],
  title: "basemap",
  id: "basemap"
});
            const map = new Map({
                basemap,
            });

            const mapView = new MapView({
                container:'mapview',
                map:map,
                zoom:10,
                center:[118.398741,31.391589],
            });
            //实例化地图切换控件
            const basemapToggle = new BasemapToggle({
            view:mapView, 
            nextBasemap: "hybrid",
            container:'basemapToggle'
            });
            mapView.ui.add(basemapToggle);

            //实例化比例尺
            const scaleBar = new ScaleBar({
             view: mapView,
             unit:'metric',
             container:'scaleBar'
            });
            mapView.ui.add(scaleBar);

            //实例化缩放控件
            const zoom = new Zoom({
            view: mapView,
            container:'zoom'
            });
            mapView.ui.add(zoom);
            mapview.ui.components =[];
            this.$store.commit('_setDefaultView',mapview)
            
        },
    },
  };
  </script>
  
  <style>
  .mapview-pannel,
  #mapview {
    position: relative;
    width: 100%;
    height: 100%;
}
#basemapToggle{
    position:absolute;
    right:15px;
    bottom:15px;
}
#scaleBar{
    position:absolute;
    left:15px;
    bottom:15px;
}
#zoom{
    position:absolute;
    right:15px;
    bottom:100px;
}
  </style>
  