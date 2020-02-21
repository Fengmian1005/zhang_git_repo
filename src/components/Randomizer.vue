<template>
  <v-app>

    <Alert :show="alert.show" :type="alert.type" :showtime="alert.showtime" @clear="clearAlert" @updateShow="update => alert.show = update">{{ alert.message }}</Alert>


    <Content @alert="showAlert"/>


  </v-app>
</template>

<script>
import Alert from '@/components/Randomizer/Alert'
import Content from '@/components/Randomizer/Content'

export default {
  name: 'Randomizer',

  components: {
    Alert,
    Content,
  },

  methods: {
    showAlert: function (message, type) {
      this.alert.show = true;
      this.alert.message = message;
      this.alert.type = type;
      this.alert.showtime = 3000;
    },
    clearAlert: function () {
      this.alert.show = false;
      this.alert.message = null;
      this.alert.type = null;
      this.alert.showtime = 0;
    }
  },

  data: () => ({
    drawer: false,
    alert: {
      show: false,
      message: null,
      type: null,
      showtime: 0
    }
  })
}
</script>

<style lang="scss">
h1 {
  line-height: 1.1;
}
a {
  color: #fff !important;

  &,
  &:hover,
  &:focus,
  &:active {
    text-decoration: none;
  }

  &:hover,
  &:focus,
  &:active {
    text-shadow: 0 0 0.5rem rgba(#fff, 0.5);
  }
}
</style>
