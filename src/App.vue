<template>
  <div id="app" class="d-flex flex-column vh-100">
    <Header/>

    <Main/>

    <Footer/>
  </div>
</template>

<script>
import Header from '@/components/Header'
import Main from '@/components/Main'
import Footer from '@/components/Footer'

export default {
  name: 'App',
  components: {
    Header,
    Main,
    Footer
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

a,
.btn-link {
  color: #333;

  &,
  &:hover,
  &:focus,
  &:active {
    text-decoration: none;
  }
}
</style>
