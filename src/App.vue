<template lang="pug">
  #app
    img(src='https://osmandi.github.io/platzimusic/dist/logo.png')
    h1 PlatziMusic
    p Selecciona un país de la lista para mostrar el top de artistas de dicho país. Usa la api de #[a(href="https://www.last.fm/api/show/geo.getTopArtists")= "https://www.last.fm/api/show/geo.getTopArtists"]
    select(v-model="selectedCountry")
        option(v-for="country in countries" v-bind:value="country.value") {{ country.name  }}
    spinner(v-show="loading")
    ul        
        artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'
export default {
  name: 'app',
  data () {
    return {
        artists: [],
        countries: [
            { name: 'Argentina', value: 'argentina' },
            { name: 'Colombia', value: 'colombia'},
            { name: 'España', value: 'spain' },
            { name: 'Venezuela', value: 'venezuela' },
        ],
        selectedCountry: 'argentina',
        loading: true
    }
  },
  components: {
  Artist,
  Spinner
  },
  methods:{
    refreshArtists(){
        const self = this
        this.loading = true
        this.artists = []
        getArtists(this.selectedCountry)
            .then(function (artists) {
            self.loading = false
            self.artists = artists        
        })
        
    }
  },
  mounted() {
    this.refreshArtists()
  },
  watch: {
    selectedCountry() {
        this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
#app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color red !important
    margin-top 60px

h1, h2
    font-weight normal

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px
p
    color black

a
    color #42b983
</style>
