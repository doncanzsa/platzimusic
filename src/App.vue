<template lang="pug">
#app
  img(src='https://doncanzsa.github.io/platzimusic/assets/logo.png')
  h1 {{ msg }}
  h2 Essential Links
  select(v-model="selectedCountry")
    option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
  spinner(v-show="loader")
  ul
    artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
import Artist from './components/Artist';
import getArtists from './api';
import Spinner from './components/Spinner';

export default {
  name: 'app',
  data () {
    return {
      msg: 'PlatziMusic By Doncans',
      artists: [],
      countries: [
          { name: 'Argentina', value: 'argentina' },
          { name: 'Colombia', value: 'colombia' },
          { name: 'España', value: 'spain' }
      ],
      selectedCountry: 'argentina',
      loader: true
    }
  },
  components: {
      Artist,
      Spinner
  },
  methods: {
      refresArtists() {
        const self = this;
        this.loader = true;
        this.artists = [];
        getArtists(this.selectedCountry).then( function(artists) {
          self.artists = artists;
          self.loader = false;
        });
      }
  },
  mounted() {
      this.refresArtists();
  },
  watch: {
      selectedCountry() {
          this.refresArtists();
      }
  },
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
