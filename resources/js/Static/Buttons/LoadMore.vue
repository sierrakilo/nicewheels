<template>
    <li class="flex items-center">
        <button @click="loadMore" class="w-full h-full bg-cyan-600 text-white hover:bg-cyan-400" v-if="show">
            <i class="fas fa-ellipsis-h fa-4x"></i>
        </button>
    </li>
</template>

<script>
export default {
    name: "ButtonLoadMore",
    props: {
        ids: {
            type: Array,
            default: []
        },
        makesAndDescendants: null
    },

    data () {
        return {
            show: true
        }
    },

    computed: {
        collection () {
            return this.$store.getters['static/filterCombined']
        },
    },

    mounted () {
        this.$store.commit('static/setCollection', { collection: 'filterStatic', data: this.ids })
    },

    methods: {
        loadMore () {
            const user = JSON.parse(localStorage.getItem('user', null))
            axios.post('/api/cars/', { ids: this.collection, makes: this.makesAndDescendants })
                .then(resp => {
                    this.$store.commit('static/addToCollection', { collection: 'filterMore', item: resp.data.cars.data.map(d => d.id) })
                    this.$store.commit('static/addToCollection', { collection: 'filterCars', item: resp.data.cars.data })

                    if(user) {
                        this.$store.commit('static/addToCollection', { collection: 'bookmarks', item: resp.data.bookmarks })
                        this.$store.commit('static/addToCollection', { collection: 'redemptions', item: resp.data.redemptions })
                    }
                    
                    if(resp.data.cars.last_page === 1) {
                        this.show = false
                    }
                })
        }
    }
}
</script>

<style scoped>

</style>
