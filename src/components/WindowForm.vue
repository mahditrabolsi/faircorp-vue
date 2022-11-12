<template>

  <div class="card w-50 mx-auto my-4 py-3 ">
    <form @submit.prevent="createWindow">
      <div class="form-group">
        <label for="name">Name</label>
        <input type="text" class="form-control" id="name" v-model="window.name" required>
      </div>
      <div class="form-group">
        <label for="room">Room</label>
        <select class="form-control" id="room" v-model="window.roomId" required>
          <option v-for="room in rooms" :value="room.id">{{ room.name }}</option>
        </select>
      </div>

      <div class="form-group">
        <label for="windowStatus">Window Status</label>
        <select class="form-control" id="windowStatus" v-model="window.windowStatus" required>
          <option value="OPEN">Open</option>
          <option value="CLOSED">Closed</option>
        </select>
      </div>
      <button type="submit" class="btn btn-primary">Create</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'WindowForm',
  emits: ['response'],
  data() {
    return {
      window: {
        name: '',
        roomId: '',
        roomName: '',
        windowStatus: ''
      },
      rooms: []
    }
  },
  methods: {
    createWindow() {
      this.window.roomName = this.rooms.find(room => room.id === this.window.roomId).name
      axios({
        method: 'post',
        url: 'https://mahditrabolsi.cleverapps.io/api/windows',
        auth: {
          username: "mahdi",
          password: "user",
        },
        data: this.window
      })
        .then(response => {
          console.log(response.data)
          this.$emit('success', "true");
        })
        .catch(error => {
          console.log(error)
          this.$emit('success', "false");
          this.$emit('response', error.response.data);
        });
        
    }
  },
  mounted() {
    axios.get('http://mahditrabolsi.cleverapps.io/api/rooms', {
      auth: {
        username: 'mahdi',
        password: 'user'
      }
    })
      .then(response => {
        this.rooms = response.data
      })
  },
}



</script>

<style lang="scss" scoped>
.form-title {
  text-align: center;
  font-weight: bold;
  color: #000;

}
</style>