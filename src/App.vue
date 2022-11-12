<template>
  <div id="app">
    <header>
      <div class="title">Smart Building Managment</div>
    </header>

    <section class="main-content w-50 mx-auto">
      <ul class="nav nav-tabs">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Windows</a>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Rooms</a>
        </li>
      </ul>

      <div class="windows-list pt-3">
        <div v-for="window in this.windows" :key="window.id">
          <Window :window="window" />
        </div>


      </div>
    </section>
    <div>
      <!-- call update values function after response -->
      <WindowForm @response="update_values()" />
    </div>

  </div>
</template>

<script>
import axios from 'axios'
import WindowForm from './components/WindowForm.vue'
import Window from './components/Window.vue'
export default {
  name: 'App',
  components: {
    WindowForm,
    Window
  },

  //predfine data
  data() {
    return {
      windows: []
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
