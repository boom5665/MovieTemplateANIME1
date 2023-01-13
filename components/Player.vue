<template>
    <div class="player-page">
        <b-container>
            <div class="dis-between" style="display: flex; justify-content: center; margin-top: 30px">
                <h2 class="list-title color-blue-sea">{{ _movieObj.full_name }} ตอนที่ {{ _ep }}</h2>
            </div>

            <div class="nopadding">
                <div class="">
                    <!-- <b-aspect aspect="16:9" class="mb-2">
                        <div class="iframe-container" style="z-index: 0">
                            <iframe id="playervideo" :src="playerIframeUrl" allowfullscreen="allowfullscreen" style="width: 100%; height: 100%; border: 0px; overflow: hidden"></iframe>
                            <AdsVideo :_adsArray="adsVideoList" v-model="isShowAds" v-if="adsVideoList.length > 0 && isShowAds" />
                        </div>
                    </b-aspect> -->
                    <div
                        class="iframe-container"
                        style="z-index: 0; cursor: pointer; width: 100%; height: 100%; border: 0px; display: flex; position: relative; justify-content: center"
                        @click="Movie()">
                        <nuxt-img format="webp" src="/playtem.png" alt="loader" />
                    </div>
                </div>

                <div class="contract">
                    <nuxt-link style="display: flex; align-items: center" :to="Back()">
                        <div class="pagination-btn">&lsaquo;</div>
                        <div class="">ตอนก่อนหน้า</div>
                    </nuxt-link>
                    <nuxt-link style="display: flex; align-items: center" :to="Next()">
                        <div class="">ตอนถัดไป</div>
                        <div class="pagination-btn prev">&rsaquo;</div>
                    </nuxt-link>
                </div>
                <div class="d-flex align-items-center justify-content-between">
                    <div class="sound-container">
                        <div class="btn-bot" v-if="_movieObj.player_link_main != '' && currentSound != 'Main'" @click="currentSound = 'Main'">ลิ้งค์หลัก ({{ _movieObj.sound_main }})</div>
                        <div class="btn-bot" v-if="_movieObj.player_link_backup1 != '' && currentSound != 'Backup1'" @click="currentSound = 'Backup1'">
                            ลิ้งค์สำรอง 1 ({{ _movieObj.sound_backup1 }})
                        </div>
                        <div class="btn-bot" v-if="_movieObj.player_link_backup2 != '' && currentSound != 'Backup2'" @click="currentSound = 'Backup2'">
                            ลิ้งค์สำรอง 2 ({{ _movieObj.sound_backup2 }})
                        </div>
                    </div>

                    <div class="btn-bot-alert" @click="showReportPopup = true">แจ้งอนิเมะเสีย</div>
                </div>

                <!-- <div class="custom-card-container mt-3" v-if="_type == 'series'"> -->
                <div class="custom-card-container mt-3">
                    <div class="custom-card-header">เลือกตอน</div>
                    <div class="custom-card-content">
                        <div class="ep-list">
                            <nuxt-link
                                :to="getEpLink(index + 1)"
                                class="ep-row"
                                :class="{ active: index + 1 == _ep }"
                                v-for="(value, key, index) in epArray"
                                :key="index"
                                v-show="index + 1 != epArray.length">
                                {{ key }}
                            </nuxt-link>
                            <!-- {{ _movieObj.player_link_main }} -->
                        </div>
                    </div>
                </div>
                <div class="popup-container" id="request-popup" v-show="showReportPopup" @click.self="showReportPopup = false">
                    <div class="popup-content">
                        <!-- <div class="text-right"><b-icon-x-circle-fill class="popup-close-btn" @click="showReportPopup = false" /></div> -->
                        <div class="tab-btn-list">
                            <div class="tab-btn active">แจ้งอนิเมะเสีย</div>
                        </div>

                        <div class="tab-content-list">
                            <div class="tab-content">
                                <textarea id="request-detail" style="height: 150px" class="popup-input" v-model="reportDetail" @keyup="isLetter"></textarea>
                                <div class="text-center">
                                    <div class="submit-btn" @click="reportMovie()">ส่ง</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <b-row class="bg-img">
                <b-col cols="12" lg="4" xl="4" class="nopadding">
                    <b-container>
                        <!-- <div class="dis-between">
                            <h2 class="list-title">อนิเมะแนะนำ</h2>
                        </div>
                        <b-row style="display: grid; grid-template-columns: 1fr 1fr">
                            <b-col style="margin-bottom: 10px" cols="12" lg="12" xl="12" v-for="(value, index) in movieList" :key="index">
                                <SingleMovielist :_obj="value" />
                            </b-col>
                        </b-row> -->

                        <div class="preview-poster">
                            <nuxt-img loading="lazy" style="width: 90%" format="webp" :src="_movieObj.picture" :alt="_movieObj.full_name" />
                        </div>
                        <!-- <div class="preview-url">
                            <div class="">{{ _isAV ? "ตัวอย่างอนิเมะ:" : "" }} {{ _movieObj.full_name }}</div>
                            <iframe class="preview-movie" :src="previewUrl" frameborder="0" allowfullscreen></iframe>
                        </div> -->
                    </b-container>
                </b-col>
                <b-col cols="12" lg="8" xl="8" class="nopadding text-end">
                    <div class="dis-between">
                        <h2 class="list-title">{{ _movieObj.full_name }}</h2>
                    </div>
                    <div class="movie-description">
                        <h3 class="color-blue-sea">เรื่องย่อ</h3>
                        <div class="desword">
                            {{ _movieObj.description }}
                        </div>
                    </div>
                </b-col>
            </b-row>
        </b-container>
        <div class="mb-2">
            <b-container>
                <b-row>
                    <b-col cols="12" lg="12" xl="12" class="">
                        <div class="" style="margin: 5px">
                            <!-- <div class="player-top-content">
                                <div class="player-top-content-font">
                                    คะแนน : <span class="point-style">{{ _movieObj.ratescore }} / {{ _isAV ? 100 : 10 }}</span>
                                </div>
                            </div> -->
                            <!-- <div class="player-top-content">
                                <div class="player-top-content-font">
                                    ปีที่ฉาย : <span class="point-style">{{ _movieObj.year }}</span>
                                </div>
                            </div> -->
                            <div class="player-top-content">
                                <div class="player-top-content-font" style="margin: 5px 0px">
                                    <span class="color-blue-sea">เสียง :</span> <span class="point-style">{{ _movieObj.sound_main ? _movieObj.sound_main : "ไม่มี" }}</span>
                                </div>
                            </div>
                            <div class="player-top-content">
                                <div class="d-flex">
                                    <div class="player-top-content-font" style="margin: 5px 0px">
                                        <span class="color-blue-sea">หมวดหมู่ :</span>
                                        <span class="point-style" v-for="(value, index) in _movieObj.type" :key="index"> {{ value.name_th ? value.name_th : "ไม่มี" }}</span>
                                    </div>
                                </div>
                            </div>
                            <div class="player-top-content">
                                <div class="d-flex">
                                    <div class="player-top-content-font" style="margin: 5px 0px">
                                        <span class="color-blue-sea">ประเภท :</span> <span class="point-style" v-for="(value, index) in _movieObj.Category" :key="index"> {{ value.name_th }}</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <!-- <div class="menu-btn"><CategoryListnobar :_isAV="true" /></div> -->
                    </b-col>
                </b-row>
            </b-container>
        </div>
        <div class="" v-if="movieList.length">
            <div class="dis-between grop-head">
                <h2 class="list-title color-blue-sea">อนิเมะแนะนำ</h2>
            </div>
            <div class="group-content">
                <div class="" v-if="movieList">
                    <swiperAnime />
                </div>
                <div class="" v-if="movieList">
                    <swiperAnime />
                </div>
            </div>

            <div class="loader-container" v-show="loadingList">
                <nuxt-img format="webp" src="/loader.png" alt="loader" />
            </div>
        </div>

        <div class="loader-container fixed" v-show="showLoader">
            <nuxt-img format="webp" src="/loader.png" alt="loader" />
        </div>
    </div>
</template>

<script>
export default {
    props: {
        _id: {
            type: Number,
            required: true,
            default: 0,
        },
        _slug: {
            type: String,
            required: true,
            default: "",
        },
        _ep: {
            type: String,
            required: false,
            default: "",
        },
        _type: {
            type: String,
            required: false,
            default: "movie",
        },
        _movieObj: {
            type: Object,
            required: true,
        },
        _isAV: {
            type: Boolean,
            required: false,
            default: false,
        },
    },
    data() {
        return {
            currentSound: "Main",
            // playerIframeUrl: "",
            serieLinkMain: "",
            relateList: [],
            showReportPopup: false,
            reportDetail: "",
            adsVideoList: [],
            isShowAds: true,
            showLoader: false,
            showmovie: true,
            movieList: [],
            currentPage: 1,
            maxPage: 1,
        };
    },
    computed: {
        epArray() {
            if (this._movieObj.player_link_main) return JSON.parse(this._movieObj.player_link_main);
            return "";
        },
        // previewUrl: function () {
        //     if (this._movieObj.preview) return "https://www.youtube.com/embed/" + this._movieObj.preview + "?controls=0&rel=0";
        //     return "";
        // },
        // playerIframeUrl() {
        //     if (this._movieObj.player_link_main) {
        //         const linkEP = JSON.parse(this._movieObj.player_link_main);
        //         return "https://movie.aegistrex.com/anime/api/player/" + Object.values(linkEP)[parseInt(this._ep) - 1];
        //     }
        //     return "";
        // },
        catIdArray() {
            let cat = [];
            for (const key in this._movieObj.Category) {
                if (Object.hasOwnProperty.call(this._movieObj.Category, key)) {
                    const element = this._movieObj.Category[key];
                    cat.push(element.id);
                }
            }
            return cat;
        },
    },
    created() {
        this.setPlayerUrl();
        this.getAdsVideo();
        this.getRelateMovies();

        if (this._isHot) {
            this.getHotMovies();
        } else if (this._isSearch) {
            this.getSearchMovie();
        } else {
            this.getMovies();
        }
    },
    mounted() {},
    watch: {
        // currentSound() {
        //     const self = this;
        //     this.playerIframeUrl = "";
        //     setTimeout(() => {
        //         self.setPlayerUrl();
        //     }, 100);
        // },
    },
    methods: {
        showMovie() {
            if (this.showmovie) {
                this.showmovie = false;
            }
        },
        setPlayerUrl() {
            const linkEP = JSON.parse(this._movieObj.player_link_main);
            if (this.currentSound == "Main") this.playerIframeUrl = "https://movie.aegistrex.com/anime/api/player/" + Object.values(linkEP)[this._ep];
        },
        getRelateMovies() {
            const self = this;
            this.$axios
                .$post("anime/listmoviesimilar", {
                    category: self.catIdArray,
                    perpage: 12,
                    page: 1,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        self.relateList = response.result;
                    }
                });
        },
        getEpLink(ep) {
            return "/anime/" + this._id + "/" + this._slug + "/" + ep;
        },

        Next() {
            var toplam = parseInt(this._ep) + 1;
            var Last = this._movieObj.last_episode;
            // console.log(toplam);
            // console.log(Last.slice(2));
            if (toplam > Last.slice(2)) {
                return "/anime/" + this._id + "/" + this._slug + "/" + this._ep;
            } else {
                return "/anime/" + this._id + "/" + this._slug + "/" + toplam;
            }
        },

        Back() {
            var toplam = parseInt(this._ep) - 1;
            if (toplam <= 0) {
                return "/anime/" + this._id + "/" + this._slug + "/" + this._ep;
            } else {
                return "/anime/" + this._id + "/" + this._slug + "/" + toplam;
            }
        },

        Movie() {
            this.$swal("", "กรุณาซื้อแพ็กเกจ", "warning");
        },
        reportMovie() {
            const detail = this.reportDetail;
            if (detail.trim() == "") {
                this.$swal("", "กรุณากรอกข้อมูลให้ครบ", "error");
                return;
            }
            // const self = this;
            // this.showLoader = true;
            // let type_id = 0;
            // if (this._isAV) type_id = 1;
            // this.$axios
            //     .$post("moviebroken", {
            //         movie_id: self._id,
            //         movie_type: type_id,
            //         reason: detail,
            //     })
            //     .then(function (response) {
            //         self.showReportPopup = false;
            //         self.showLoader = false;
            //         self.reportDetail = "";
            //         if (response.code == 200) {
            //            self.$swal("", "สำเร็จ", "success");
            //         }
            //     });
            this.$swal("", "กรุณาซื้อแพ็กเกจ", "warning");
        },
        getMovies() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("anime/listmovie", {
                    order: "ID",
                    orderby: "DESC",
                    types: "",
                    category: "",
                    year: 0,
                    perpage: 8,
                    page: this.currentPage,
                    search: this._search,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        // console.log(response.result);
                        self.movieList = response.result;
                        self.maxPage = response.page_total;
                    }
                    self.loadingList = false;
                });
        },
        getAdsVideo() {
            const self = this;
            this.$axios.$get("list/ads-video").then(function (response) {
                if (response.code == 200) {
                    self.adsVideoList = response.result ? response.result : [];
                }
            });
        },
    },
};
</script>


