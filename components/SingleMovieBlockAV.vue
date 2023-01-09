<template>
    <nuxt-link :to="getPlayUrl(_obj)" class="single-movie-block">
        <div class="poster-container">
            <nuxt-img class="poster" :loading="_fetchMode" :src="_obj.picture" height="287" :alt="_obj.full_name" />

            <div class="poster-overlay">
                <!-- <div class="rating" v-show="_obj.ratescore">{{ score ? score : _obj.ratescore }}</div> -->
                <!-- <b-icon-play-circle class="poster-play" /> -->
                <nuxt-img format="webp" src="/play.png" alt="loader" />
                <!-- <div style="position: absolute; bottom: 0; width: 100%">
                    <div class="title">{{ _obj.full_name }}</div>
                    <div class="view"><b-icon-eye class="view-icon" /> {{ _obj.view }}</div>
                </div> -->
            </div>
        </div>

          <div class="d-flex align-items-center dis-group" style=" flex-direction: column;text-align: center;">
            <!-- <div class="resolution">{{ _obj.quality }}</div>
            <div class="sound flex-grow-1">{{ _obj.sound_main ? _obj.sound_main : "NOSUB" }}</div>
            <div class="resolution resolution-2">{{ score ? score : _obj.ratescore }}</div> -->
            <div class="title">{{ _obj.full_name }}</div>
            <div class="view"><b-icon-eye class="view-icon" /> {{ _obj.view }}</div>
        </div>
    </nuxt-link>
</template>

<script>
export default {
    props: {
        _obj: {
            type: Object,
            required: true,
        },
        _fetchMode: {
            type: String,
            required: false,
            default: "lazy",
        },
    },
    data() {
        return {
            score: "",
        };
    },
    mounted() {
        this.getfixscore();
    },

    methods: {
        getfixscore() {
            this.score = Number.parseFloat(this._obj.ratescore).toFixed(0);
        },
    },
};
</script>

<style>
.dis-group {
    background: transparent;
    border-radius: 0px 0px 10px 10px;
}
</style>
