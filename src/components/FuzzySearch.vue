<template>
  <div class="columns is-desktop">
    <div class="column">
      <h1 class="title">Original Title</h1>
      <p>{{ count === originalTitles.length - 1 ? 'END' : originalTitle }}</p>
      <button class="button field is-link" @click="submit" style="margin-top: 1rem;">
            <span>Next</span>
        </button>
    </div>
    <div class="column">
      <h1 class="title">Suggested Title</h1>
      <form v-on:submit.prevent="submit">
        <b-table
            :data="filteredDataObj"
            :columns="columns"
            :selected.sync="selected"
            focusable>
        </b-table>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Fuse from 'fuse.js'

export default {
  data () {
    return {
      count: 0,
      originalTitles: [],
      correctTitles: [],
      updatedTitles: [],
      selected: null,
      columns: [
        {
          label: 'title',
          field: 'item.titleFr',
        },
        {
          label: 'score',
          field: 'score',
        }
      ],
      fuseOptions: {
        tokenize: true,
        matchAllTokens: false,
        shouldSort: true,
        includeScore: true,
        threshold: 0.3,
        location: 0,
        distance: 100,
        maxPatternLength: 200,
        minMatchCharLength: 1,
        keys: [
          "titleFr"
        ]
      },
      fuse: null
    }
  },
  computed: {
    originalTitle () {
      if(this.originalTitles.length) {
        return this.originalTitles[this.count].title.toString()
      }
      return 'loading...'
    },
    filteredDataObj () {
      if(this.fuse && this.originalTitles.length && this.correctTitles.length) {
        let originalTitle = this.originalTitles[this.count].title.toString()
        if(this.count === this.originalTitles.length - 1)
          return []
        let top3 = this.fuse.search(originalTitle).slice(0,3)
        return top3.map(r => {
          r.score = Math.round((1 - r.score) * 100) + '%'
          return r
        })
      }
      return this.correctTitles
    }
  },
  methods: {
    submit () {
      this.count++;
      this.updatedTitles.push(this.selected.item)
    }
  },
  mounted () {
    // original titles and correct titles
    axios.get('http://localhost:8000/api/original-titles').then(response => {
      this.originalTitles = response.data.map( t => {return {'title': t}})
    })
    axios.get('http://localhost:8000/api/correct-titles').then(response => {
      this.correctTitles = response.data
      this.fuse = new Fuse(this.correctTitles, this.fuseOptions)
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
