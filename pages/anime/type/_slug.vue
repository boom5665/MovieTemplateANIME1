<template>
    <client-only>
        <MovieList :_title="typeObj.name_th" :_type="_slug" />
    </client-only>
</template>

<script>
export default {
    name: "animeTypeSlug",
    async asyncData({ params, $axios }) {
        const res = await $axios.$get("anime/listtype?slug=" + params.slug);
        const typeObj = res.result[0];
        return { typeObj };
    },
    head() {
        return {
            titleTemplate: (titleChunk) => {
                return titleChunk ? `${titleChunk} - ` + this.typeObj.name_th : this.typeObj.name_th;
            },
        };
    },
    data() {
        return {};
    },
    computed: {
        _slug() {
            return this.$route.params.slug;
        },
    },
    mounted() {},
    methods: {},
};
</script>

<style>
</style>
