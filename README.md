## Inspiration
We looked through the open data portal of the City of Montréal (http://donnees.ville.montreal.qc.ca/) for data related to issues linked with climate change and sustainability.
## What it does
It shows you where you can find resources pertinent to a heatwave situation (i.e. water fountains and conditioned air) on a map. It can also display a compilation of the zones of the city historically affected by heatwaves.
## How we built it
We made a web app. using Vue.js. We made AJAX calls to endpoints provided by the City of Montréal using axios and integrated these data with a map component from VueLayers which uses OpenLayers (an opensource map library). For the UI, we used a template called Argon (https://www.creative-tim.com/product/vue-argon-design-system).
## Challenges we ran into
Data formats were inconsistent across data sets. There is one data set where the endpoint is protected against cross origin requests, rendering us unable to use it. The map from VueLayers does not support click event registration on feature elements (overlay objects), so we had to find a workaround.
## Accomplishments that we're proud of
Making an application that works in time. Our team has different strengths and we managed to work together efficiently enough to produce a coherent product.
## What we learned
Furthered knowledge of Vue.js. Learned to use the APIs from the City of Montréal. Learned to use a map and add elements to it.
## What's next for Heat Wave - Life Saver
+ More data types
+ Heatwave notifications
+ Crowd statistics (to sort places by the least currently populated)
+ Authentication and user reviews
