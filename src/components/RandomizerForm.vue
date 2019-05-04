<template>
  <form id="randomizerForm" @submit.prevent="onSubmit" :class="isLoading ? 'loading' : null">
    <div class="row">
      <div class="form-group col">
        <div class="row">
          <div class="col-form-label col-md-9 text-md-right">
              <label for="wordsNumber">Number of words:</label>
          </div>
          <div class="col-md-3 text-md-left">
            <input id="wordsNumber" type="number" class="form-control text-center" placeholder="Limit" v-model="wordsNumber">
          </div>
        </div>
      </div>
      <div class="form-group col">
        <div class="row">
          <div class="col-form-label col-md-9 text-md-right">
            Randomize words:
          </div>
          <div class="col-md-3 text-md-left">
            <button type="submit" class="btn btn-primary form-submit w-100" title="Get a randomized words">RUN</button>
          </div>
        </div>
      </div>
    </div>
    <div class="form-group text-left">
      <label for="wordsInput">Words input:</label>
      <textarea id="wordsInput" rows="8" class="form-control" placeholder="Words input" v-model="wordsInput"></textarea>
    </div>
    <div class="form-group text-left">
      <label for="wordsOutput">Words output:</label>
      <textarea id="wordsOutput" rows="4" class="form-control" placeholder="Words output" v-model="wordsOutput"></textarea>
    </div>
    <div class="form-group">
      <button type="button" class="btn btn-link form-save-output" title="Save words output" @click="saveOutput">SAVE</button>
      <button type="button" class="btn btn-link form-clear-output" title="Clear words output" @click="clearOutput">CLEAR</button>
      <button type="button" class="btn btn-link form-clear-storage" title="Reset all fields to default state" @click="clearStorage">RESET</button>
    </div>
  </form>
</template>

<script>
export default {
  data () {
    return {
      isLoading: false,
      wordsNumber: 20,
      wordsInput: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      wordsOutput: ''
    }
  },
  mounted: function () {
    this.loadFromStorage('wordsNumber');
    this.loadFromStorage('wordsInput');
    this.loadFromStorage('wordsOutput');
  },
  watch: {
    wordsNumber: function (newValue, oldValue) {
      this.saveToStorage('wordsNumber');
    },
    wordsInput: function (newValue, oldValue) {
      this.saveToStorage('wordsInput');
    },
    wordsOutput: function (newValue, oldValue) {
      this.saveToStorage('wordsOutput');
    }
  },
  methods: {
    getRandomInt: function (min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    onSubmit: function () {
      this.isLoading = true;
      let words = this.wordsInput;
      // replace new lines with spaces
      words = words.replace(/(?:\r\n|\r|\n)/g, ' ');
      // remove dots and comas
      words = words.replace(/(?:\.|,)/g, '');
      // convert string to array
      const wordsArray = words.split(' ');
      let output = '';
      // randomize words
      for (let i = 0, len = parseInt(this.wordsNumber); i < len; i++) {
        const randomIndex = this.getRandomInt(0, wordsArray.length - 1);
        if (wordsArray[randomIndex]) {
          output += wordsArray[randomIndex] + ' ';
        }
      }
      this.wordsOutput = output.trim();
      this.isLoading = false;
      this.$emit('alert', 'The words have been randomized.', 'success');
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
    clearOutput: function () {
      this.wordsOutput = '';
      this.$emit('alert', 'The output has been cleared.', 'success');
    },
    saveOutput: function () {
      if (this.wordsOutput) {
        this.saveFile(this.wordsOutput, 'words-output.txt');
        this.$emit('alert', 'The output has been saved.', 'success');
      } else {
        this.$emit('alert', 'The output is empty and cannot be saved.', 'danger');
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

<style lang="scss" scoped>
.col-form-label {
  white-space: nowrap;

  label {
    margin: 0;
  }
}
.loading {
  cursor: wait;
}
</style>
