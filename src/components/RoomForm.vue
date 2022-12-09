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
            <button type="submit" id="create" class="btn btn-primary">Create</button>
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
            //disable create button
           var createButton = document.getElementById('create') 
           createButton.disabled = true            
            axios({
                method: 'post',
                url: 'http://localhost:4000/api/rooms',
                auth: {
                    username: "mahdi",
                    password: "user",
                },
                data: this.room
            })
                .then(response => {
                    console.log(response.data)
                    this.$emit('success', "true");
                    createButton.disabled = false
                })
                .catch(error => {
                    console.log(error)
                    this.$emit('success', "false");
                    this.$emit('response', error.response.data);
                    createButton.disabled = false
                });

        }
    },
    mounted() {
        axios.get('http://localhost:4000/api/buildings', {
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