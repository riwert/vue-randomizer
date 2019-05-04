<template>
  <div id="app" class="d-flex flex-column vh-100">
    <Header/>

    <div id="content">
      <div class="container text-center">
        <router-view/>
      </div>
    </div>

    <Footer/>
  </div>
</template>

<script>
import Header from '@/components/Header'
import Footer from '@/components/Footer'

export default {
  name: 'App',
  components: {
    Header,
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
