<template v-if="enabled">
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-3" id="menu">
        <br />
        <i>Kies hier je jaar </i>
        <br />
        <select
          class="custom-select"
          v-model="this.selectedSheet"
          @change="changedYear()"
        >
          <option
            v-for="sheetTitle in sheetTitles"
            v-bind:key="sheetTitle"
            v-bind:value="sheetTitle"
          >
            {{ sheetTitle }}
          </option>
        </select>
        <br />

        <div class="w-100"></div>
        <div class="chapters">
          <br />
          <div
            v-for="(chapterTitle, index) in chapterTitles"
            v-bind:key="chapterTitle"
          >
            <label>{{ chapterTitle }} </label>&nbsp;
            <input
              type="checkbox"
              v-model="this.selectedColumns"
              v-bind:value="index"
              @change="changedChapters()"
            />
          </div>
        </div>

        <div class="w-100"></div>
        <i class="bi bi-alarm" style="font-size:20px;"></i>
        &nbsp;
        <input
          type="number"
          v-model="this.timerSetting"
          step="5"
          min="5"
          max="60"
          style="width: 44px"
        /> &nbsp;
        <label>sec per ronde</label>

        <div class="w-100"></div>
        <br />
        <button class="duolingo-button" role="button" @click="handleClick()">
          <span>Genereer kaartje</span>
        </button>
      </div>

      <div class="p-3 col-sm-9">
        <Kaartje
          ref="kaartje"
          :concepts="this.randomConcepts"
          @revealed="startTimer()"
        />
        <Timer
          ref="timer"
          :timeAlotted="this.timerSetting"
          :running="this.timerIsRunning"
        />
      </div>
    </div>
  </div>
</template>

<script>
// https://medium.com/@hiddendreamz7/deploy-vue-js-site-an-easy-approach-dcfe3c2e166d

import Kaartje from "./components/Kaartje.vue";
import Timer from "./components/Timer.vue";

const { GoogleSpreadsheet } = require("google-spreadsheet");
const doc = new GoogleSpreadsheet(
  "1zoyOkAtnoJKZ3SZgqLUBpq0Ci56NmK9KiimEisHsbAU"
);

export default {
  name: "App",
  components: {
    Kaartje,
    Timer,
  },
  emits: ["resetTimer"],
  data() {
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
      roundCounter: 0,
      timerSetting: 30,
      timerIsRunning: false,
    };
  },
  methods: {
    async changedYear() {
      this.chapterTitles = [];
      this.glossary = [];
      this.sheet = await doc.sheetsByTitle[this.selectedSheet];
      await this.sheet.loadCells();

      for (let i = 0; i < this.sheet.columnCount; i++) {
        let title = this.sheet.getCell(0, i).value;
        if (title != null) this.chapterTitles.push(title);
      }
    },
    async handleClick() {
      if (this.chaptersChangedSinceLastQuery) this.loadGlossary();

      this.randomizeNextCard();
      this.$refs.timer.resetTimer();
      this.$refs.kaartje.revealed = false;
    },
    changedChapters() {
      this.chaptersChangedSinceLastQuery = true;
      this.roundCounter = 0;
    },
    startTimer() {
      this.timerIsRunning = true;
      console.log("App.resetTimer called successfully");
    },
    loadGlossary() {
      let itemPool = [];
      for (let rowIndex = 1; rowIndex < this.sheet.rowCount; rowIndex++) {
        this.selectedColumns.forEach((columnIndex) => {
          let value = this.sheet.getCell(rowIndex, columnIndex).value;
          if (value != null) itemPool.push(value);
        });
      }
      this.glossary = itemPool;
      this.chaptersChangedSinceLastQuery = false;
    },
    randomizeNextCard() {
      if (this.glossary.length == 0) {
        this.randomConcepts.push("ERROR: SELECTEER EEN HOOFDSTUK!");
      } else {
        if (
          this.roundCounter == 0 ||
          this.roundCounter * 5 + 5 > this.glossary.length
        ) {
          console.log("GLOSSARY WAS SHUFFLED!");
          this.glossary.sort(() => 0.5 - Math.random());
          this.roundCounter = 0;
        }
        this.randomConcepts = this.glossary.slice(
          this.roundCounter * 5,
          this.roundCounter * 5 + 5
        );
        this.roundCounter++;
      }
    },
  },
  async mounted() {
    doc.useApiKey("AIzaSyCnxnz6XDkbbOCmxNvLsKBx9Jrrk2SgGCs");
    await doc.loadInfo();
    doc.sheetsByIndex.forEach((sheet) => this.sheetTitles.push(sheet.title));
    this.enabled = true;
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 50px;
}

#menu {
  background-color: lightgray;
  border-style: solid;
  border-width: 2px;
  width: 100%;
  padding-bottom: 80px;
}

.duolingo-button {
  appearance: button;
  background-color: #268600;
  border: solid transparent;
  border-radius: 16px;
  border-width: 0 0 4px;
  box-sizing: border-box;
  color: #ffffff;
  cursor: pointer;
  display: inline-block;
  font-family: din-round, sans-serif;
  font-size: 15px;
  font-weight: 700;
  letter-spacing: 0.8px;
  line-height: 20px;
  margin: 0;
  outline: none;
  overflow: visible;
  padding: 13px 16px;
  text-align: center;
  text-transform: uppercase;
  touch-action: manipulation;
  transform: translateZ(0);
  transition: filter 0.2s;
  user-select: none;
  -webkit-user-select: none;
  vertical-align: middle;
  white-space: nowrap;
  width: 100%;
}

.duolingo-button:after {
  background-clip: padding-box;
  background-color: #00c90a;
  border: solid transparent;
  border-radius: 16px;
  border-width: 0 0 4px;
  bottom: -4px;
  content: "";
  left: 0;
  position: absolute;
  right: 0;
  top: 0;
  z-index: -1;
}

.duolingo-button:main,
.duolingo-button:focus {
  user-select: auto;
}

.duolingo-button:hover:not(:disabled) {
  filter: brightness(1.1);
}

.duolingo-button:disabled {
  cursor: auto;
}

.chapters {
  min-height: 70%;
}
</style>
