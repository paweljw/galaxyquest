<template>
  <div class="item">
    <a :href="item.url" v-html="nameWithMatches"></a><br/>
    <span class="desc">{{ item.desc }}</span>
  </div>
</template>

<script>
export default {
  name: 'star',
  props: ['fullname'],
  computed: {
    item () {
      if (this.fullname.fullName) {
        return this.fullname
      } else {
        return this.fullname.item
      }
    },
    nameWithMatches() {
      var nameCopy = this.item.fullName

      if (this.fullname.matches) {
        let nameMatches = this.fullname.matches[0].indices
        var offset = 0

        nameMatches.forEach(([start, end]) => {
          let length = end - start
          if (length < 1) {
            return
          }
          let slice = nameCopy.slice(offset + start, offset + end + 1)
          nameCopy = nameCopy.substr(0, offset) + nameCopy.substr(offset).replace(slice, `<strong>${slice}</strong>`)
          offset += (length + 17)
        })
      }

      return nameCopy
    }
  }
}
</script>

<style type="sass">
.item {
  margin-bottom: 14px;
}

.item a {
  color: #000;
}

.item .desc {
  color: #aaa;
}

strong {
  font-weight: 700;
  font-size: 110%;
}
</style>
