<template>
  <div id="app">
    <header>
      <div class="title">Smart Building Managment</div>
    </header>

    <section class="main-content w-50 mx-auto">
      <navigation @panel-change="updatePanel"></navigation>
       <Window v-if="currentPanel.name === 'Window'"></Window>
      <WindowForm v-if="currentPanel.name === 'WindowForm'"></WindowForm> 
    </section>

    <section class="main-content w-50 mx-auto">
      <!-- <ul class="nav nav-tabs" id="myTab" role="tablist">
        <li class="nav-item" role="presentation">

          <button class="nav-link active" id="windows-tab" data-bs-toggle="tab"  href="#windows" type="button" role="tab" aria-controls="windows" aria-selected="true">Windows</button>
        </li>
        <li class="nav-item" role="presentation">

          <button class="nav-link"  id="rooms-tab" data-bs-toggle="tab" href="#rooms" type="button" role="tab" aria-controls="rooms" aria-selected="false">Rooms</button>
        </li>
      </ul> -->

      <div class="windows-list pt-3">
        <div v-for="window in this.windows" :key="window.id">
          <Window :window="window" />
        </div>
        <div>
          <WindowForm @success="update_values" />
        </div>
        </div>

      <div id="rooms" class="rooms-list pt-3  tab-pane fade" role="tabpanel" aria-labelledby="rooms-tab">
      rooms
      <WindowForm/>
      </div>

      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import WindowForm from './components/WindowForm.vue'
import Window from './components/Window.vue'
import Navigation from './components/Navigation.vue'

export default {
  name: 'App',
  components: {
    WindowForm,
    Window,
    Navigation
  },

  //predfine data
  data: function() {
    return {
      windows: [],
      currentPanel: {}
    }
  },
  methods: {
    update_values() {
      this.windows = []
      axios.get('http://mahditrabolsi.cleverapps.io/api/windows', {
        auth: {
          username: 'mahdi',
          password: 'user'
        }
      })
        .then(response => {
          console.log("got response")
          this.windows = response.data
        })
        .catch(error => {
          console.log(error)
        })
    },
    updatePanel(newPanel) {
      this.currentPanel = newPanel;
    },
  },

  created() {
    axios.get('http://mahditrabolsi.cleverapps.io/api/windows', {
      auth: {
        username: 'mahdi',
        password: 'user'
      }
    })
      .then(response => {
        this.windows = response.data
      })
  }


}

</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.icon {
  position: relative;

  &.open {
    font-size: 12px;
    top: -3px;
    color: #198754;
  }

  &.closed {
    color: #dc3545;
  }
}

.window {
  .top-row {
    cursor: pointer;
  }
}
</style>

