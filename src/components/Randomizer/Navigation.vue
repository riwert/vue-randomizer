<template>
  <v-navigation-drawer
    v-model="localDrawer"
    app
    temporary
    dark
    :src="background"
  >
    <v-img :src="logo" alt="Vuetify Logo" aspect-ratio="1" max-width="50%" class="mx-auto mt-2"></v-img>

    <v-list dense>
      <v-list-item link href="https://riwert.github.io/vue-randomizer">
        <v-list-item-action>
          <v-icon>mdi-home</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>Home</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item link href="mailto:revert@revert.pl">
        <v-list-item-action>
          <v-icon>mdi-contact-mail</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>Contact</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item link href="https://github.com/riwert/vue-randomizer" target="_blank" rel="noopener">
        <v-list-item-action>
          <v-icon>mdi-github-circle</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>GitHub</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
import logo from '@/assets/vuetify-logo.png'
import background from '@/assets/v-bg.webp'

export default {
  name: 'Navigation',

  props: {
    drawer: Boolean
  },

  data: () => ({
    localDrawer: false,
    logo,
    background
  }),

  watch: {
    drawer: function () {
      this.localDrawer = this.drawer
    },
    localDrawer: function () {
      this.$emit('updateDrawer', this.localDrawer)
    }
  }
}
</script>
