<template>
  <div>
    <div class="mb-10">
      <div class="fr">
        Showing {{ stars.length }} of {{ allStars.length }}
      </div>
      <input placeholder="Start typing here..." v-model="search"></input>
    </div>
    <template v-for="currentStar in stars">
      <star :fullname="currentStar"></star>
    </template>
  </div>
</template>

<script>
import Star from './Star.vue'
import Fuse from 'fuse.js'

export default {
  name: 'stars',
  props: ['username'],
  data () {
    return {
      allStars: [],
      fuse: null,
      search: ''
    }
  },
  computed: {
    stars () {
      if (this.search.length === 0) {
        return this.allStars
      }

      return this.fuse.search(this.search);
    }
  },
  created () {
    let starsUrl = `https://api.github.com/users/${this.username}/starred?per_page=100`
    var vm = this

    fetch(starsUrl).then(response => response.json())
      .then((data) => {
        data.forEach(element => vm.allStars.push({
          fullName: element.full_name,
          url: element.html_url,
          desc: element.description
        }))

        vm.fuse = new Fuse(vm.allStars, {
          shouldSort: true,
          minMatchCharLength: 1,
          keys: ['fullName'],
          includeMatches: true,
          threshold: 0.4,
          tokenize: true,
          location: 0
        })
      })
  },
  components: {
    Star
  }
}
</script>

<style type="scss">
.mb-10 {
  margin-bottom: 10px
}
</style>
