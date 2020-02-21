<template>
  <v-snackbar
    v-model="localShow"
    :color="localType"
    :timeout="localShowtime"
    top
  >
    <slot></slot>
    <v-btn
      icon
      @click.prevent="$emit('clear')"
    >
      <v-icon color="white">mdi-close-circle-outline</v-icon>
    </v-btn>
  </v-snackbar>
</template>

<script>
export default {
  props: {
    show: Boolean,
    type: String,
    showtime: Number
  },

  data: () => ({
    localShow: false,
    localType: null,
    localShowtime: 0
  }),

  watch: {
    show: function () {
      this.localShow = this.show
    },
    localShow: function () {
      this.$emit('updateShow', this.localShow)
    },
    type: function () {
      this.localType = this.type
    },
    showtime: function () {
      this.localShowtime = this.showtime
    }
  }
}
</script>
