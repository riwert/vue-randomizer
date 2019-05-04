<template>
  <div :class="['alert alert-dismissible fade show', 'alert-' + (type ? type : 'info')]" role="alert">
    <slot></slot>
    <button type="button" class="close" data-dismiss="alert" aria-label="Close" @click.prevent="$emit('clear')">
      <span aria-hidden="true">&times;</span>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    type: String
  }
}
</script>

<style lang="scss" scoped>
.alert {
  position: fixed;
  z-index: 10;
  top: 1rem;
  right: 1rem;
}
</style>
