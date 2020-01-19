<template>
    <div class="profile-page">
        <section class="section-profile-cover section-shaped my-0">
            <div class="shape shape-style-1 shape-primary shape-skew alpha-4">
            </div>
        </section>
        <section class="section section-skew">
            <div class="container" v-on:click="onMapClick">
                
                <card shadow class="card-profile mt--300" no-body>
                    <vl-map :load-tiles-while-animating="true" :load-tiles-while-interacting="true"
                            data-projection="EPSG:4326" style="height: 400px">
                    <vl-view :zoom.sync="zoom" :center.sync="center" :rotation.sync="rotation"></vl-view>

                    <vl-geoloc @update:position="center = $event">
                        <template slot-scope="geoloc">
                        <vl-feature v-if="geoloc.position" id="position-feature">
                            <vl-geom-point :coordinates="geoloc.position"></vl-geom-point>
                            <vl-style-box>
                            <vl-style-icon src="https://static.thenounproject.com/png/1598658-200.png" :scale="0.2" :anchor="[0.5, 1]"></vl-style-icon>
                            </vl-style-box>
                        </vl-feature>
                        </template>
                    </vl-geoloc>

                    <div v-for="fountain in fountainsToShow" v-bind:key="fountain.id">
                        <vl-feature>
                            <vl-geom-point :coordinates="fountain.coordinates"></vl-geom-point>
                            <vl-style-box>
                                <vl-style-icon src="https://images.vexels.com/media/users/3/145358/isolated/preview/caae118ed1bd555d7a47dcadf5af0bf8-water-drop-falling-illustration-by-vexels.png" :scale="0.05" :anchor="[0.5, 1]"></vl-style-icon>
                            </vl-style-box>
                        </vl-feature>

                        <vl-overlay :position="fountain.coordinates">
                            <template>
                                <div class="overlay-content">
                                    {{ fountain.ID }}
                                </div>
                            </template>
                        </vl-overlay>

                        <vl-overlay :position="fountain.coordinates" v-if="fountain.isSelected">
                            <template>
                                <div class="overlay-content">
                                    <card shadow class="card-profile mt--300" no-body>
                                        <span v-if="fountain.Nom_parc_lieu">{{ fountain.Nom_parc_lieu }}</span><br>
                                    </card>
                                </div>
                            </template>
                        </vl-overlay>
                    </div>

                    <vl-layer-tile id="osm">
                        <vl-source-osm></vl-source-osm>
                    </vl-layer-tile>
                    </vl-map>
                </card>
                
                <div v-for="fountain in fountainsToShow.slice(0, 15)" v-bind:key="fountain.id">
                    <card shadow>
                        <div class="row">
                            <p class="h5 col-md-10 col-sm-8">
                                <img src="https://images.vexels.com/media/users/3/145358/isolated/preview/caae118ed1bd555d7a47dcadf5af0bf8-water-drop-falling-illustration-by-vexels.png" width="32px">
                                #{{ fountain.ID }}
                            </p>
                            <base-button class="btn-3 mr--0 col-md-2 col-sm-4" type="primary" size="lg" icon="fa fa-map">Directions</base-button>
                        </div>
                        <div>
                            <p v-if="fountain.Nom_parc_lieu" class="description"><icon name="fa fa-leaf"></icon> Parc : {{ fountain.Nom_parc_lieu }}</p>
                            <p v-if="fountain.Arrondissement" class="description"><icon name="fa fa-building"></icon> Arrondissement : {{ fountain.Arrondissement }}</p>
                            <p v-if="fountain.Intersection" class="description"><icon name="fa fa-road"></icon> Intersection : {{ fountain.Intersection }}</p>
                            <p v-if="fountain.Remarque" class="description text-warning"><icon name="fa fa-info-circle"></icon> {{ fountain.Remarque }}</p>
                        </div>
                    </card>
                </div>
            </div>
        </section>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    data() {
        return {
            zoom: 15,
            center: [-73.71368412394081, 45.56278194861761],
            rotation: 0,
            fountains: null,
            displayDistance: 0.03
        }
    },
    methods: {
        onMapClick(event) {
            let coordinates = {
                longitude: event.offsetX - 500,
                latitude: event.offsetY - 200
            };
            console.log(coordinates);
        }
    },
    computed: {
        fountainsToShow() {
            let fountainsToShow = [];
            if(this.fountains != null) {
                this.fountains.forEach(fountain => {
                    if(Math.sqrt(Math.pow(fountain.Longitude - this.center[0], 2) + Math.pow(fountain.Latitude - this.center[1], 2)) <= this.displayDistance)
                        fountainsToShow.push(fountain);
                });
            }
            return fountainsToShow.sort((a, b) => Math.sqrt(Math.pow(a.Longitude - this.center[0], 2) + Math.pow(a.Latitude - this.center[1], 2))
                                                 -Math.sqrt(Math.pow(b.Longitude - this.center[0], 2) + Math.pow(b.Latitude - this.center[1], 2)));
        }
    },
    mounted() {
        axios.get("http://donnees.ville.montreal.qc.ca/dataset/3ff400f3-63cd-446d-8405-842383377fb8/resource/26659739-540d-4fe2-8107-5f35ab7e807c/download/fontaine_eau_potable_v2018.csv")
             .then(response => {
                 let data = response.data;
                 let lines = data.split("\n");
                 let header = lines[0].split(",");
                 lines.shift();

                 let fountains = [];
                 lines.forEach(line => {
                     let columns = line.split(",");
                     let fountain = {};
                     for(let i = 0; i < header.length; ++i)
                        fountain[header[i].replace('\r', '').replace(' ', '')] = columns[i] != null ? columns[i].replace("\r", '').replace(' ', '') : "";
                    fountain.coordinates = [
                        parseFloat(fountain.Longitude),
                        parseFloat(fountain.Latitude)
                    ];
                    fountain.isSelected = false;
                    if(fountain.ID != "")
                        fountains.push(fountain);
                 });

                 this.fountains = fountains;
        });
        axios.get("@/cooled-places.xml")
             .then(response => {
                 parseString(response.data, function(err, result) {
                     console.log(result);
                 });
        });
    }
};
</script>
<style>
</style>
