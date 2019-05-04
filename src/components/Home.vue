<template>
  <div id="home">
    <Alert v-if="alert.message" :type="alert.type" @clear="clearAlert">{{ alert.message }}</Alert>
    <RandomizerForm @alert="showAlert"/>
  </div>
</template>

<script>
import Alert from '@/components/Alert'
import RandomizerForm from '@/components/RandomizerForm'

export default {
  name: 'Home',
  components: {
    Alert,
    RandomizerForm
  },
  data () {
    return {
      alert: {
        message: null,
        type: null,
        delay: 3000,
        timeout: 0
      }
    }
  },
  methods: {
    showAlert: function (message, type) {
      if (this.alert.timeout) {
        clearTimeout(this.alert.timeout);
      }

      this.alert.message = message;
      this.alert.type = type;

      this.alert.timeout = setTimeout(() => {
        this.clearAlert();
      }, this.alert.delay);
    },
    clearAlert: function () {
      this.alert.message = null;
      this.alert.type = null;
    }
  }
}
</script>

<style>

</style>
