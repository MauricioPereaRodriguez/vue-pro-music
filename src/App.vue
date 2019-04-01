<template lang="pug">
  #app
    section.section
      nav.nav.has-shadow
        .container
          .field.has-addons
            .control
              input.input.is-large(
                type="text",
                placeholder="¿Qué canción estás buscando?",
                v-model="searchQuery"
              )
            .control
              a.button.is-info.is-large(v-on:click="search") Buscar
            .control
              a.button.is-danger.is-large &times;
          p.help.is-info.has-text-right
            small {{ searchMessage }}
      .container.results
        .columns
          .column(v-for="t in tracks") {{ t.name }} - {{ t.artists[0].name }}"
</template>

<script>
import trackService from './services/track'

export default {
  name: 'app',
  data () {
    return {
      searchQuery: '',
      tracks: []
    }
  },
  computed: {
    searchMessage () {
      return `Encontrados: ${this.tracks.length}`
    }
  },
  methods: {
    search () {
      if (this.searchQuery) { return }

      trackService.search(this.searchQuery)
        .then(res => {
          this.tracks = res.tracks.items
        })
    }
  }
}
</script>

<style lang="scss">
  @import './scss/main.scss';
  .results {
    margin-top: 50px;
  }
</style>
