<template>
  <v-content>
    <v-container>
      <v-row
        align="start"
        justify="center"
      >
        <v-col class="text-center">

          <h1>List Input Test</h1>

          <div class="position">
            <v-btn color="warning" class="ma-1" @click="clearInput">
              Clear
            </v-btn>           
            <v-btn color="primary" class="ma-1" @click="onSubmit">
              Enter
            </v-btn> 
            <v-btn color="success" class="ma-1" @click="saveOutput">
              Save
            </v-btn>          
          </div>

          <v-textarea
            counter
            outlined
            name="input"
            label="Input"
            required
            v-model="randomInput"
          ></v-textarea>

          <v-textarea
            counter
            outlined
            name="result"
            label="Result"
            v-model="randomResult"
          ></v-textarea>
        </v-col>
      </v-row>
    </v-container>
  </v-content>
</template>

<script>
export default {
  name: 'Content',

  data: () => ({
    isLoading: false,
    example: false,
    randomAmount: '',
    randomInput: '',
    randomResult: ''
  }),

  mounted: function () {
    this.loadFromStorage('randomAmount');
    this.loadFromStorage('randomInput');
    this.loadFromStorage('randomResult');
  },
  watch: {
    randomAmount: function () {
      this.saveToStorage('randomAmount');
    },
    randomInput: function () {
      this.saveToStorage('randomInput');
    },
    randomResult: function () {
      this.saveToStorage('randomResult');
    }
  },
  methods: {
    getRandomInt: function (min, max) {
      return min + max;
    },
    onSubmit: function () {
      this.isLoading = true;
      let random = this.randomInput;
      if ( ! random.length) {
        return this.$emit('alert', 'The input is empty.', 'error');
      }
      // validate incorrect abmount
      if (this.randomAmount <= 0) {
        return this.$emit('alert', 'The amount is incorrect.', 'error');
      }
      // convert string to array
      const randomArray = random.split(' ');
      let output = '';
      // randomize input and glue it as a result
      for (let i = 0, len = parseInt(this.randomAmount); i < len; i++) {
        const randomIndex = this.getRandomInt(0, randomArray.length - 1);
        if (randomArray[randomIndex]) {
          output += randomArray[randomIndex] + ' ';
        }
      }
      this.randomResult = output;
      this.isLoading = false;
      if (this.randomResult) {
        this.$emit('alert', 'Your input has been pushed.', 'success');
      } else {
        return this.$emit('alert', 'The result is empty.', 'error');
      }
    },
    saveToStorage: function (key) {
      if (this[key] !== undefined) {
        const parsed = JSON.stringify(this[key]);
        localStorage.setItem(key, parsed);
      }
    },
    loadFromStorage: function (key) {
      if (localStorage.getItem(key)) {
        this[key] = JSON.parse(localStorage.getItem(key));
      }
    },
    clearStorage: function () {
      localStorage.clear();
      location.reload();
    },
    clearInput: function () {
      if (this.randomInput) {
        this.randomInput = '';
        this.$emit('alert', 'The input has been cleared.', 'success');
      } else {
        this.$emit('alert', 'The input is already empty.', 'error');
      }
    },
    clearOutput: function () {
      if (this.randomResult) {
        this.randomResult = '';
        this.$emit('alert', 'The output has been cleared.', 'success');
      } else {
        this.$emit('alert', 'The output is already empty.', 'error');
      }
    },
    saveOutput: function () {
      if (this.randomResult) {
        this.saveFile(this.randomResult, 'random-output.txt');
        this.$emit('alert', 'The output has been saved.', 'success');
      } else {
        this.$emit('alert', 'The output is empty and cannot be saved.', 'error');
      }
    },
    saveFile: function (textData, fileName) {
      const blob = new Blob([textData], {type: 'text/plain'});
      const e = document.createEvent('MouseEvents');
      const a = document.createElement('a');
      a.download = fileName;
      a.href = window.URL.createObjectURL(blob);
      a.dataset.downloadurl = ['text/plain', a.download, a.href].join(':');
      e.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
      a.dispatchEvent(e);
    }
  }
}
</script>

<style>
  .position {
    position: relative;
    margin-right: -68%;
  }
</style>