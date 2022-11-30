<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref } from 'vue'
const lights = ref({
  light1: "#ff0000",
  light2: "#ff0000",
  light3: "#ff0000",
  light4: "#ff0000",
})

const setupFinished = ref(false)
const hubs = ref([])
const selectedHub = ref(null)

function updateColors() {
  console.log(lights.value)
}

function authHue() {
  fetch(`http://${internalipaddress}/api/newdeveloper`)
  .then(response => {
    return response.json()
  })
  .then(data => {
    console.log(data)
  })
}

function selectHub(hub) {
  selectedHub.value = hub
  authHue()
  // fetch(`http://${selectedHub.value.internalipaddress}/api/0/config`)
  // .then(response => {
  //   return response.json()
  // })
  // .then(data => {
  //   console.log(data)
  // })
}

function setupHue() {
  fetch("https://discovery.meethue.com/")
  .then(response => {
    return response.json()
  })
  .then(data => {
    hubs.value = data
  })
}
if (!selectedHub.value) {
  setupHue()
} else {
  authHue()
}
</script>

<template>
  <nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <a class="navbar-item" href="/">
        A tool to set the hue palette
      </a>

      <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false">
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
      </a>
    </div>
  </nav>
  <div class="container">
    <section class="section" v-if="!setupFinished">
      <div class="card">
        <div class="card-header">
          <p class="title">
            Setting up Hue
          </p>
        </div>
        <div class="card-content">
          <div>
            Select your hub
          </div>
          <button class="button" v-for="hub in hubs" :key="hub.id" @click="selectHub(hub)">
            Hub {{hub.id}}
          </button>
        </div>
      </div>
    </section>
    <section class="section" v-if="setupFinished">
      <div class="card">
        <div class="card-header">
          <p class="title">
            Set Palette
          </p>
        </div>
        <div class="card-content">
          <div class="is-flex">
            <div>
              <label for="favcolor">Light 1</label>
              <input type="color" id="favcolor" name="favcolor" v-model="lights.light1"/>
            </div>
            <div>
              <label for="favcolor">Light 2</label>
              <input type="color" id="favcolor" name="favcolor" v-model="lights.light2"/>
            </div>
            <div>
              <label for="favcolor">Light 3</label>
              <input type="color" id="favcolor" name="favcolor" v-model="lights.light3"/>
            </div>
            <div>
              <label for="favcolor">Light 4</label>
              <input type="color" id="favcolor" name="favcolor" v-model="lights.light4"/>
            </div>
          </div>
          <div>
            <button class="button" @click="updateColors">Set Color</button>
          </div>
        </div>
      </div>
    </section>
    <section class="section">
      <div class="card">
        <div class="card-header">
          
        </div>
      </div>
    </section>
  </div>
</template>

<style lang="scss" scoped>

</style>
