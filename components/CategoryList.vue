<template>
    <div class="">
        <!-- <div class="category-header">หมวดหมู่</div> -->
        <!-- <div class="category-list" style="display: grid; grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr"> -->
        <!-- <div class="category-single" v-for="(value, index) in types" :key="'type' + index">
                <nuxt-link :to="'/' + _type + '/type/' + value.name_slug">{{ value.name_th }}</nuxt-link>
            </div> -->
        <!-- <nuxt-link v-for="(value, index) in categories" :key="'cate' + index" :to="'/' + _type + '/category/' + value.name_slug ">
                <div class="category-single-style">
                    <nuxt-link :to="'/' + _type + '/category/' + value.name_slug" class="category-single-font">{{ value.name_th }}</nuxt-link>
                </div>
            </nuxt-link> -->
        <!-- </div> -->

        <div class="" @mouseover="onOver" @mouseleave="onLeave">
            <nuxt-img format="webp" style="margin-left: 5px" src="/navbar.png" alt="loader" />
            <b-dropdown id="dropdown-1" class="m-md-2 dropdown all">
                <b-dropdown-item :to="'/' + _type + '/category/' + value.name_slug" class="category-single drop-con" v-for="(value, index) in categories" :key="'cate' + index">
                    <span>{{ value.name_th }}</span>
                </b-dropdown-item>
            </b-dropdown>
        </div>
        <!-- <b-dropdown id="dropdown-1" text="หมวดหมู่" class="m-md-2 dropdown">
            <li class="category-single drop-con" v-for="(value, index) in categories" :key="'cate' + index">
                <nuxt-link :to="'/' + _type + '/category/' + value.name_slug" class="drop-a">{{ value.name_th }}</nuxt-link>
            </li>
        </b-dropdown> -->
    </div>
</template>

<script>
export default {
    props: {
        _type: {
            type: String,
            required: false,
            default: "anime",
        },
    },
    data() {
        return {
            categories: [],
            types: [],
        };
    },
    created() {
        this.getType();
        this.getCategory();
    },
    methods: {
        async getType() {
            const data = await this.$axios.$get(this._type + "/listtype");
            if (data.code == 200) this.types = data.result;
        },
        async getCategory() {
            const data = await this.$axios.$get(this._type + "/listcate");
            if (data.code == 200) this.categories = data.result;
        },
        onOver() {
            this.$refs.dropdown = true;
        },
        onLeave() {
            this.$refs.dropdown = false;
        },
    },
};
</script>

<style lang="">
</style>
