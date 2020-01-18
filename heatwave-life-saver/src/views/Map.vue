<template>
    <div class="profile-page">
        <section class="section-profile-cover section-shaped my-0">
            <div class="shape shape-style-1 shape-primary shape-skew alpha-4">
                <span></span>
                <span></span>
                <span></span>
                <span></span>
                <span></span>
                <span></span>
                <span></span>
            </div>
        </section>
        <section class="section section-skew">
            <div class="container">
                <card shadow class="card-profile mt--300" no-body>
                    <vl-map :load-tiles-while-animating="true" :load-tiles-while-interacting="true"
                            data-projection="EPSG:4326" style="height: 400px">
                    <vl-view :zoom.sync="zoom" :center.sync="center" :rotation.sync="rotation"></vl-view>


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
            zoom: 10,
            center: [-73.71368412394081, 45.56278194861761],
            rotation: 0,
            geolocPosition: undefined
        }
    }
};
</script>
<style>
</style>
