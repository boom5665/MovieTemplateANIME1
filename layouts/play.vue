<template>
    <div>
        <b-container>
            <Header />
            <!-- <div class="text-center">
                <h2 class="web-title">devmovie</h2>
                <h3 class="web-slogan">{{ slogan }}</h3>
            </div> -->
        </b-container>
        <!-- <div class="full-category-line"></div> -->
        <b-container>
            <b-row>
                <!-- <b-col cols="12" md="3" lg="2">
                    <div class="d-none d-sm-block">
                        <CategoryList :_isAV="true" />
                    </div>
                </b-col> -->
                <b-col cols="12" md="12" lg="12">
                    <Nuxt />
                </b-col>
            </b-row>
            <Banner />
            <Footer />
            <div class="footer-text">
                <div class="footer-title">{{ slogan }}</div>
                <div class="footer-content" v-html="descriptionFooter"></div>
            </div>
        </b-container>

        <!-- <div class="footer-ads-container" v-show="showAds"> -->
        <div class="footer-ads-container" v-show="showAds" v-if="adsBottom.length > 0">
            <div class="inner-container">
                <a class="ads-image" :href="value.url" target="_blank" v-for="(value, index) in adsBottom" :key="index" @click="updateAdsImageClick(value)">
                    <nuxt-img :src="value.picture" :alt="value.name" />
                </a>
                <!-- <nuxt-img format="webp" src="/center.png" alt="loader" /> -->
                <div class="close-ads" @click="showAds = false">X</div>
            </div>
        </div>
    </div>
</template>

<script>
import { mapState } from "vuex";
export default {
    head() {
        return {
            title: this.SEOTitle,
            meta: [
                {
                    hid: "description",
                    name: "description",
                    content: this.SEODescription,
                },
                {
                    name: "keywords",
                    content: ["ดูอนิเมะ", "ดูอนิเมะออนไลน์", "ดูซีรี่ย์"],
                },
                {
                    property: "og:title",
                    name: "og:title",
                    content: this.SEOTitle,
                },
                {
                    property: "og:description",
                    name: "og:description",
                    content: this.SEODescription || "mugquwas open graph meta description",
                },
                {
                    property: "og:url",
                    name: "og:url",
                    content: "https://www.devmovie.com",
                },
                {
                    property: "og:site_name",
                    name: "og:site_name",
                    content: "devmovie",
                },
                {
                    property: "og:image",
                    name: "og:image",
                    content: this.logo,
                },
            ],
            link: [{ rel: "icon", type: "image/x-icon", href: this.icon }],
        };
    },
    data() {
        return {
            menuType: [],
            menuCategory: [],
            menuYear: [],
            showAds: true,
        };
    },
    created() {
        this.getMenuMovies();
    },
    computed: {
        ...mapState({
            descriptionFooter: (state) => state.descriptionFooter,
            logo: (state) => state.logo,
            icon: (state) => state.icon,
            slogan: (state) => state.slogan,
            SEOTitle: (state) => state.SEOTitle,
            SEODescription: (state) => state.SEODescription,
            adsBottom: (state) => state.adsBottom,
        }),
    },
    mounted() {},
    methods: {
        async getMenuMovies() {
            const data = await this.$axios.$get("anime/listtypeandcate");
            if (data.code == 200) {
                this.menuType = data.result.Listdata_type;
                this.menuCategory = data.result.Listdata_cate;
            }
        },
    },
};
</script>
<style lang="scss" scoped>
.container,
.container-sm,
.container-md,
.container-lg,
.container-xl {
    @media (min-width: 1200px) {
        max-width: 80%;
    }
    @media (min-width: 576px) {
        max-width: 80%;
    }
}
</style>
