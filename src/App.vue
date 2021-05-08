<template>
  <div id="app">
    <Header/>
    <Form/>
  </div>
</template>

<script>
import Form from './components/Form.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    Form,
    Header
  }
}
</script>

<style lang="sass">
@import url('https://fonts.googleapis.com/css2?family=Mitr:wght@500&family=Open+Sans:wght@300;400&display=swap')
@import './sass/_reset.sass'
// @import './sass/vars.sass'

#app
  font-family: 'Open Sans', sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale

@import './sass/_media'
</style>
