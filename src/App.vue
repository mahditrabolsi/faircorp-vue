<template>
  <div id="app">
    <header>
      <div class="title">Smart Building Managment</div>
    </header>

    <section class="main-content w-50 mx-auto">
      <navigation @panel-change="updatePanel"></navigation>
      <Spinner size="medium" class=" m-4 " id="spinner" />
      <div v-if="currentPanel.name === 'Windows'">

        <div v-for="window in this.windows" :key="window.id">
          <Window :window="window" />
        </div>
        <WindowForm @success="update_windows" />
      </div>
      <div v-if="currentPanel.name === 'Rooms'">
        <div v-for="room in this.rooms" :key="room.id">
          <Room :room="room" />
        </div>
        <RoomForm @success="update_rooms" />

      </div>


    </section>
  </div>
</template>

<script>
import axios from 'axios'
import WindowForm from './components/WindowForm.vue'
import Window from './components/Window.vue'
import Navigation from './components/Navigation.vue'
import RoomForm from './components/RoomForm.vue'
import Room from './components/Room.vue'
import Toasted from 'vue-toasted';
import Vue from 'vue';
import Spinner from 'vue-simple-spinner'
Vue.use(Toasted)

export default {
  name: 'App',
  components: {
    WindowForm,
    Window,
    Navigation,
    Room,
    RoomForm,
    Spinner
},

  data: function () {

    return {
      windows: [],
      currentPanel: {},
      rooms: []
    }
  },
  methods: {
    update_windows() {
        var toast = this.$toasted.show('Updating windows...');
      var spinner = document.getElementById('spinner');
        this.windows = [];
      spinner.style.display = 'block';
      axios.get('http:localhost:4000/api/windows', {
        auth: {
          username: 'mahdi',
          password: 'user'
        }
      })
        .then(response => {
          spinner = document.getElementById("spinner");
          spinner.style.display = 'none';
          this.windows = response.data
            toast.goAway(0);
            this.$toasted.success('Windows updated', {
              duration: 2000,
            });

        })
        .catch(error => {
          spinner = document.getElementById("spinner");
          spinner.style.display = 'none';
            toast.goAway(0);
            this.$toasted.error('Error updating windows', {
              duration: 2000
            });
        })
    },
    updatePanel(newPanel) {
      this.currentPanel = newPanel;
    },
    update_rooms(from_create = false) {
        var toast = this.$toasted.show('Updating rooms...');
      axios.get('http:localhost:4000/api/rooms', {
        auth: {
          username: 'mahdi',
          password: 'user'
        }
      })
        .then(response => {
          this.rooms = response.data
            toast.goAway(0);
            this.$toasted.success('Rooms updated', {
              duration: 2000
            });
        })
        .catch(error => {
          console.log(error)
            toast.goAway(0);
            this.$toasted.error('Error updating rooms', {
              duration: 2000
            });
        })
    },
  },
  created() {
    var spinner = document.getElementById("spinner");
    axios.get('http:localhost:4000/api/windows', {
        auth: {
          username: 'mahdi',
          password: 'user'
        }
      })
        .then(response => {
          spinner = document.getElementById("spinner");
          spinner.style.display = "none";
          this.windows = response.data

        })
        .catch(error => {
          spinner = document.getElementById("spinner");
          spinner.style.display = "none";
          console.log(error)
        })
        axios.get('http:localhost:4000/api/rooms', {
        auth: {
          username: 'mahdi',
          password: 'user'
        }
      })
        .then(response => {
          this.rooms = response.data
        })
        .catch(error => {
          console.log(error)
        })
  },

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

<style>
@import 'assets/style.css';
</style>

