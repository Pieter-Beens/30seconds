<template v-if="enabled">
  <select v-model="this.selectedSheet" @change="changedYear()">
    <option v-for="sheetTitle in sheetTitles" v-bind:key="sheetTitle" v-bind:value="sheetTitle">
      {{ sheetTitle }}
    </option>
  </select>

  <div v-for="(chapterTitle, index) in chapterTitles" v-bind:key="chapterTitle">
    <label>{{chapterTitle}}</label>
    <input type="checkbox" v-model="this.selectedColumns" v-bind:value="index" @change="changedChapters()" />
  </div>

  <button id=button1 @click="handleClick()">
    <span>Genereer kaartje</span>
  </button>
  <p>test var: {{ this.glossary }}</p>
  <Kaartje :concepts="this.randomConcepts" />
</template>

<script>
// https://medium.com/@hiddendreamz7/deploy-vue-js-site-an-easy-approach-dcfe3c2e166d

import Kaartje from './components/Kaartje.vue'
const { GoogleSpreadsheet } = require('google-spreadsheet');
const doc = new GoogleSpreadsheet('1zoyOkAtnoJKZ3SZgqLUBpq0Ci56NmK9KiimEisHsbAU');

export default {
  name: 'App',
  components: {
    Kaartje
  },
  data(){
    return {
      docTitle: "",
      enabled: false,
      sheet: null,
      sheetTitles: [],
      chapterTitles: [],
      selectedSheet: "",
      selectedColumns: [],
      glossary: [],
      randomConcepts: [],
      chaptersChangedSinceLastQuery: false,
      roundCounter: 0
    }
  },
  methods: {
    async changedYear() {
      this.chapterTitles = [];
      this.glossary = [];
      this.sheet = await doc.sheetsByTitle[this.selectedSheet];
      await this.sheet.loadCells();

      for (let i = 0;i < this.sheet.columnCount; i++)
      {
        let title = this.sheet.getCell(0,i).value;
        if (title != null) this.chapterTitles.push(title);
      }
    },
    async handleClick() {
      if (this.chaptersChangedSinceLastQuery) this.loadGlossary();

      this.randomizeNextCard();
    },
    changedChapters() {
      this.chaptersChangedSinceLastQuery = true;
      this.roundCounter = 0;
    },
    loadGlossary() {
      let itemPool = [];
      for (let rowIndex = 1; rowIndex < this.sheet.rowCount; rowIndex++)
      {
        this.selectedColumns.forEach(columnIndex => 
        {
          let value = this.sheet.getCell(rowIndex,columnIndex).value;
          if (value != null) itemPool.push(value);
        })
      }
      this.glossary = itemPool;
      this.chaptersChangedSinceLastQuery = false;
    },
    randomizeNextCard() {
      if (this.glossary.length == 0) {
        this.randomConcepts.push("ERROR: SELECTEER EEN HOOFDSTUK!");
      } else {
        if (this.roundCounter == 0 || this.roundCounter*5+5 > this.glossary.length) {
          console.log("GLOSSARY WAS SHUFFLED!");
          this.glossary.sort(() => .5 - Math.random());
          this.roundCounter = 0;
        }
        this.randomConcepts = this.glossary.slice(this.roundCounter*5, this.roundCounter*5+5)
        this.roundCounter++;
      }
    }
  },
  async mounted() {
    doc.useApiKey('AIzaSyCnxnz6XDkbbOCmxNvLsKBx9Jrrk2SgGCs');
      await doc.loadInfo();
      doc.sheetsByIndex.forEach(sheet => this.sheetTitles.push(sheet.title));
      this.enabled = true;
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
