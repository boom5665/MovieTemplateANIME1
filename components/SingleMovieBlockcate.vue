<template>
    <div :to="getPlayUrl(_obj)" class="single-movie-block-cate">
        <b-row class="bg-img" style="justify-content: flex-end">
            <b-col cols="12" md="12" lg="4">
                <b-row>
                    <b-col cols="12" md="12" lg="12">
                        <div class="poster-container preview-poster">
                            <nuxt-img class="poster" :loading="_fetchMode" :src="_obj.picture" height="287" :alt="_obj.full_name" />
                            <!-- <div class="poster-overlay">
                                <div class="rating" v-show="_obj.ratescore">{{ score ? score : _obj.ratescore }}</div>
                                <b-icon-play-circle class="poster-play" />
                                <nuxt-img format="webp" src="/play.png" alt="loader" />
                                <div class="title">{{ _obj.full_name }}</div>
                            </div> -->
                        </div>
                    </b-col>
                    <!-- <b-col cols="12" md="12" lg="6">
                        <div class="poster-container" style="opacity: 0.7">
                            <nuxt-img disabled="disabled" style="background: #dddddd" class="poster" :loading="_fetchMode" :src="_obj.picture" height="287" :alt="_obj.full_name" />
                        </div>
                    </b-col> -->
                </b-row>
            </b-col>
            <b-col cols="12" md="12" lg="8" class="text-end">
                <div class="">
                    <div class="dis-between play-content">
                        <div class="play-font">อะนิเมะที่กำลังฮิตและมาแรง</div>
                        <nuxt-link :to="getPlayUrl(_obj)">
                            <div class="play-button"><nuxt-img format="webp" style="margin-left: 5px; width: 36px" src="/play.png" alt="loader" /></div>
                        </nuxt-link>
                    </div>
                    <div class="title">{{ _obj.full_name }}</div>
                    <!-- <div class="d-flex">
                        <div class="dis-between">
                            <div class="head-des-small" style="line-height: 1.8;">ประเภท :</div>
                            <div class="cate" v-for="(value, index) in _obj.Category" :key="index" style="margin-left: 5px">
                                {{ value.name_th }}
                            </div>
                        </div>
                    </div> -->
                    <!-- <div class="dis-between">
                        <div class="head-des-small">ความละเอียด :</div>
                        <div class="resolution">{{ _obj.quality }}</div>
                    </div> -->
                    <!-- <div class="dis-between">
                        <div class="head-des-small">ซับไตเติ้ล :</div>
                        <div class="sound">{{ _obj.sound_main ? _obj.sound_main : "NOSUB" }}</div>
                    </div> -->

                    <!-- <div class="dis-between">
                        <div class="head-des-small">คะแนน :</div>
                        <div class="resolution resolution-2">{{ score ? score : _obj.ratescore }}</div>
                    </div> -->

                    <div class="dis-between">
                        <div class="head-des color-blue-sea">เรื่องย่อ</div>
                    </div>
                    <div class="resolution desword">{{ _obj.description ? _obj.description : "ไม่มีคำบรรยาย" }}</div>
                </div>
            </b-col>
        </b-row>
    </div>
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
            this.score = Number.parseFloat(this._obj.ratescore).toFixed(1);
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
