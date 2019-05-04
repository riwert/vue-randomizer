<template>
  <header class="mt-5">
    <div class="container text-center">
      <h1>
        <a href="https://riwert.github.io/vue-randomizer/" title="Reload">
          <img class="img-fluid" :src="logo" alt="logo" />
          Words Randomizer
        </a>
      </h1>
    </div>
  </header>
</template>

<script>
import logo from '@/assets/logo.png'

export default {
  data: function () {
    return {
      logo: logo
    }
  }
}
</script>

<style lang="scss" scoped>
h1 {
  display: inline-block;

  a {
    display: block;

    img {
      max-height: 1.5em;
    }
  }
}
</style>
