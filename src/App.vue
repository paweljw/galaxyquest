<template>
  <div id="app">
    <h1>{{ username }}
      <div class="input-form" v-if="!isUsernameValid">
        <p>GitHub Username <input type="text" v-on:keyup.enter="validateUsername" placeholder="Type here then press Enter"></input></p>
        <p>{{ warning }}</p>
      </div>
    </h1>
    <div v-if="isUsernameValid">
      <stars :username="username"></stars>
    </div>
  </div>
</template>

<script>
import { _ } from 'lodash'
import Stars from './Stars.vue'

export default {
  name: 'app',
  data () {
    return {
      username: '',
      warning: ''
    }
  },
  computed: {
    isUsernameValid () {
      return this.username.length > 2
    },
  },
  methods: {
    validateUsername (event) {
      let possibleUsername = event.target.value
      let url = `https://api.github.com/users/${possibleUsername}`

      var vm = this

      fetch(url).then(({ status }) => {
        if(status === 200) {
          vm.username = possibleUsername
          vm.warning = ''
        } else {
          vm.warning = `Username ${possibleUsername} is invalid`
        }
      })
    }
  },
  components: {
    'stars': Stars
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Lato');

body {
  font-family: 'Lato';
}

#container {
  width: 80%;
  margin: 0 auto;
}

h1 {
  width: 100%;
  border-bottom: 1px solid #ccc;
  display: block;
  min-height: 50px;

  p {
    font-size: 14px;
  }
}

.input-form {
  float: right;
}

input { 
  width: 200px;
}
</style>
