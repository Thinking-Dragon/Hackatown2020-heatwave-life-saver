<template>
    <div class="profile-page">
        <section class="section-profile-cover section-shaped my-0">
            <div class="shape shape-style-1 shape-primary shape-skew alpha-4">
            </div>
        </section>
        <section class="section section-skew">
            <div class="container">
                
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

                    <vl-layer-tile id="osm">
                        <vl-source-osm></vl-source-osm>
                    </vl-layer-tile>
                    </vl-map>
                </card>
            </div>
        </section>
    </div>
</template>
<script>
export default {
    data() {
        return {
            zoom: 15,
            center: [-73.71368412394081, 45.56278194861761],
            rotation: 0
        }
    }
};
</script>
<style>
</style>
