<template>
    <div id="app">
        <sightings-form />
        <sightings-grid :sightings="sightings" />
    </div>
</template>

<script>
import SightingsForm from "./components/SightingsForm";
import SightingsGrid from "./components/SightingsGrid";
import SightingService from "./services/SightingService.js";

import { eventBus } from "@/main.js";

export default {
    name: "app",
    components: {
        "sightings-form": SightingsForm,
        "sightings-grid": SightingsGrid,
    },
    data() {
        return {
            sightings: [],
        };
    },
    mounted() {
        this.fetchSightings();
        eventBus.$on("add-sighting", (sighting) => {
            this.sightings.push(sighting);
        });
        eventBus.$on("delete-sighting", (id) => {
            const index = this.sightings.findIndex(
                (sighting) => sighting._id === id
            );
            this.sightings.splice(index, 1);
        });
    },
    methods: {
        fetchSightings() {
            SightingService.getSightings().then(
                (sightings) => (this.sightings = sightings)
            );
        },
    },
};
</script>

<style>
html {
    height: 100%;
}

body {
    background: url("./assets/birds-background.jpg") no-repeat;
    height: 100%;
    background-position: center;
    background-repeat: repeat;
    background-size: cover;
}
</style>
