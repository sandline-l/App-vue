<template>
  <div class="hello">
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  created () {
    axios.get('/good/seller').then(
      res => {
        console.log(res)
        if (res.data.code === 0) {
          console.log(res.data.data)
        }
      }
    )
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import '../../static/style.css';
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
