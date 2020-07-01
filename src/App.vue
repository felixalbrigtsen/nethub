<template>
<div id="app">
  <div class="App-grid" id="App-grid">
    <div v-for="tile in this.tiles" v-bind:key="tile.key" :class="'module grid-'+tile.width+'-'+tile.height+' '+tile.type">
      <EditBar :removeTile="removeTile" :tileKey="tile.key" :class="editMode ? '' : 'hidden'" />
      <div class="moduleContainer">
        <ModuleAdder v-if="tile.type==''" :key="tile.key" :allTypes="modules" :removeSelf="()=>{removeTile(tile.key)}" @setWidth="tile.width = $event" @setHeight="tile.height = $event" @setType="tile.type = $event" />
        <ModuleNotepad v-if="tile.type=='notepad'" :key="tile.key" v-model="tile.settings.text" />
        <ModuleClock v-if="tile.type=='clock'" :key="tile.key" />
        <ModuleCalculator v-if="tile.type=='calculator'" :key="tile.key" />
        <ModuleWeather v-if="tile.type=='weather'" :key="tile.key" v-model="tile.settings.location" :width="tile.width" />
      </div>
    </div>

    <div v-if="this.tiles.length == 0" class="module emptyscreen">
      <h2>It seems like you don't have any tiles! </h2>
      <h3>Enable edit mode and click one of the preset buttons at the bottom to load a preset</h3>
      <button @click="addTile" class="emptyAddTile">Or click here to add a tile!</button>
    </div>
  </div>

  <div class="App-footer" id="App-footer">
    <button @click="editMode = !editMode">{{editMode ? "Lock" : "Edit"}} Layout</button>
    <button v-if="editMode" @click="addTile">Add new tile</button>
    <button v-if="editMode" class="presetButton" @click="()=>{this.tiles = JSON.parse(this.presets[0])}">Simple preset</button>
    <button v-if="editMode" class="presetButton" @click="()=>{this.tiles = JSON.parse(this.presets[1])}">Full preset</button>
  </div>

</div>
</template>

<script>
import ModuleAdder from "./components/moduleadder"
import EditBar from "./components/editbar"

import ModuleNotepad from "./components/modules/notepad"
import ModuleClock from "./components/modules/clock"
import ModuleCalculator from "./components/modules/calculator"
import ModuleWeather from "./components/modules/weather"
import { uuid } from "vue-uuid"

export default {
  name: 'App',
  components: {
    ModuleAdder,
    EditBar,
    ModuleNotepad,
    ModuleClock,
    ModuleCalculator,
    ModuleWeather
  },
  data() {
    return {
      tiles: Array(0),
      modules: ["notepad","clock","calculator","weather"],
      editMode: false,
      presets: [
        '[{"type":"clock","width":1,"height":1,"key":"a0e51840-bbd8-11ea-8735-9756325b1007","settings":{}},{"type":"notepad","width":1,"height":1,"key":"a5f6b3c0-bbd8-11ea-8735-9756325b1007","settings":{}},{"type":"calculator","width":1,"height":1,"key":"bc6cabf0-bbd8-11ea-8735-9756325b1007","settings":{}}]',
        '[{"type":"clock","width":1,"height":1,"key":"123b4900-8fb1-11ea-8f93-4d0694a51454","settings":{}},{"type":"calculator","width":1,"height":1,"key":"fa7dbe30-8fc7-11ea-9b3e-c3cb5896e76c","settings":{}},{"type":"weather","width":"2","height":1,"key":"fdea5c40-8fc7-11ea-9b3e-c3cb5896e76c","settings":{"location":"Oslo"}},{"type":"notepad","width":1,"height":"2","key":"126d5e10-8fc8-11ea-9b3e-c3cb5896e76c","settings":{"text":"Tall notepad"}},{"type":"notepad","width":"2","height":1,"key":"3b1a4530-8fc8-11ea-9b3e-c3cb5896e76c","settings":{"text":"Wide notepad"}}]'
      ]
    }
  },
  methods: {
    addTile() {
      this.tiles = [...this.tiles, {
        type:'',
        width:1,
        height:1,
        key:uuid.v1(),
        settings:{}}]
    },
    removeTile(key) {
      this.tiles = this.tiles.filter(tile=>tile.key!=key)
    },
    loadPreset(i) {
      this.tiles = JSON.parse(this.presets[i]);
    }
  },
  watch: {
    //Store layout to localstorage
    tiles: {
      handler() {
        localStorage.setItem("tiles", JSON.stringify(this.tiles))
      },
      deep: true
    }
  },
  mounted() {
    //Load layout from localstorage
    if (localStorage.getItem("tiles")) {
      this.tiles = JSON.parse(localStorage.getItem("tiles"))
    }
  }
}
</script>

<style>
html, body, #app {
  margin: 0;
  padding: 0;
}

#app {
  background: rgb(227,227,227);
  background: linear-gradient(180deg, rgba(227,227,227,1) 0%, rgba(68,94,139,1) 100%);
}

.App-footer {
  width: 100%;
  background-color: gray;
  height: 30px;

  position: sticky;
  bottom: 0;
}

.App-footer > button {
  height: 100%;
  margin-left: 10px;
}

.presetButton {
  margin-right: 10px;
  margin-left: 0;
  float: right;
}

.App-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  grid-auto-rows: minmax(200px, 45vh);

  gap: 1rem;
  min-height: 95vh;
  overflow-y: visible;

  min-height: 100vh - 35px;
  padding: 10px;
  margin-bottom: 30px;
}
.module {
  border: 1px solid black;
  border-radius: 10px;
  background-color: lightgray;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
}
.module:hover{
  background-color: #c3c3e3;
}
.moduleContainer {
  height: 100%;
}
.grid-1-1 {
  grid-column: span 1 / auto;
  grid-row: span 1 / auto;
}

.grid-1-2 {
  grid-column: span 1 / auto;
  grid-row: span 2 / auto;
}

.grid-1-3 {
  grid-column: span 1 / auto;
  grid-row: span 3 / auto;
}

.grid-2-1 {
  grid-column: span 2 / auto;
  grid-row: span 1 / auto;
}

.grid-2-2 {
  grid-column: span 2 / auto;
  grid-row: span 2 / auto;
}

.grid-2-3 {
  grid-column: span 2 / auto;
  grid-row: span 3 / auto;
}

.grid-3-1 {
  grid-column: span 3 / auto;
  grid-row: span 1 / auto;
}

.grid-3-2 {
  grid-column: span 3 / auto;
  grid-row: span 2 / auto;
}

.grid-3-3 {
  grid-column: span 3 / auto;
  grid-row: span 3 / auto;
}

.emptyscreen {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
@keyframes shake {
  0% {transform: rotate(0deg);}
  10% {transform: rotate(-10deg);}
  20% {transform: rotate(10deg);}
  30% {transform: rotate(0deg);}
  100% {transform: rotate(0deg);}
}
.emptyAddTile {
  animation: 2s 3s infinite forwards shake;
  font-size: 16px;
  border-radius: 4px;
}

@media only screen and (max-width: 600px) {
  /* Mobile */
  .App-grid { 
    grid-template-columns: 1fr;
    grid-auto-rows: minmax(200px, 25vh);
  }
  .grid-1-1 { grid-column: span 1; grid-row: span 1;}
  .grid-1-2 { grid-column: span 1; grid-row: span 2;}
  .grid-1-3 { grid-column: span 1; grid-row: span 2;}
  .grid-2-1 { grid-column: span 1; grid-row: span 1;}
  .grid-2-2 { grid-column: span 1; grid-row: span 2;}
  .grid-2-3 { grid-column: span 1; grid-row: span 2;}
  .grid-3-1 { grid-column: span 1; grid-row: span 1;}
  .grid-3-2 { grid-column: span 1; grid-row: span 2;}
  .grid-3-3 { grid-column: span 1; grid-row: span 2;}
} 

</style>
