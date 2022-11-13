<template>

    <div class="card w-50 mx-auto my-4 py-3 ">
        <form @submit.prevent="createRoom">
            <div class="form-group">
                <label for="name">Name</label>
                <input type="text" class="form-control" id="name" v-model="room.name" required>
            </div>
            <div class="form-group">
                <label for="floor">Floor</label>
                <input type="number" class="form-control" id="floor" v-model="room.floor" required>

            </div>
            <div class="form-group">
                <label for="buildingId">Building</label>
                <select class="form-control" id="buildingId" v-model="room.buildingId" required>
                    <option v-for="building in buildings" :value="building.id">{{ building.name }}</option>
                </select>
            </div>
            <div class="form-group">
                <label for="targetTemperature">Target Temperature</label>
                <input type="number" class="form-control" id="targetTemperature" v-model="room.targetTemperature" required>

            </div>

            <div class="form-group">
                <label for="currentTemperature">Current Temperature</label>
                <input type="number" class="form-control" id="currentTemperature" v-model="room.currentTemperature" required>
                </div>



            <!-- <div class="form-group">
                <label for="room">Room</label>
                <select class="form-control" id="room" v-model="room.roomId" required>
                    <option v-for="room in rooms" :value="room.id">{{ room.name }}</option>
                </select>
            </div> -->

            <button type="submit" class="btn btn-primary">Create</button>
        </form>
    </div>
</template>
  
<script>
import axios from 'axios'
export default {
    name: 'RoomForm',
    emits: ['response'],
    data() {
        return {
            room: {
                name: '',
                roomId: '',
                roomName: '',
            },
            buildings : []
        }
    },
    methods: {
        createRoom() {
            
            axios({
                method: 'post',
                url: 'https://mahditrabolsi.cleverapps.io/api/rooms',
                auth: {
                    username: "mahdi",
                    password: "user",
                },
                data: this.room
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
        axios.get('http://mahditrabolsi.cleverapps.io/api/buildings', {
            auth: {
                username: 'mahdi',
                password: 'user'
            }
        })
            .then(response => {
                this.buildings = response.data
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