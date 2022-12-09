<template>
    <div class="room border border-secondary rounded p-2 mb-2">
        <div class="top-row d-flex">
            <div class="room-name fw-bold pe-3">{{ room.name }}</div>
            <div class="text-muted">Floor : {{ room.floor }}</div>
            <div class="text-muted" style="margin-left: 20px;">Building Id :  {{ room.buildingId }}</div>
    

            <div @click="showhide" class="expand-button ms-auto">&#9658;</div>
        </div>

        <div class="details" style="display: None;">
            <hr />
            <div class="details d-flex">

                <div>
                    <div>
                        <span class="icon">Current temperature &#x1F321; :</span>
                       <span v-if="room.currentTemperature !== null">  {{ room.currentTemperature }}°C
                        </span>
                        <span v-else>
                            <span class="icon">No data</span>
                        </span>
                    </div>
                    <div>
                        <span class="icon">Target temperature &#x1F321; :</span>
                        <span v-if="room.targetTemperature !== null"> 
                        {{ room.targetTemperature }}°C
                        </span>
                        <span v-else>
                            <span class="icon">No data</span>
                        </span>
                    </div>

                </div>
                <button type="button" style="margin-left: 40px;" class="btn btn-secondary me-2" @click="rename_room">Rename room</button>
                <button type="button" id="delete_room" style="margin-left: 10px;" class="btn btn-danger" @click="delete_room">Delete room</button>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'room',
    props: {
        room: Object
    },
    methods: {
        showhide() {
            if (this.$el.querySelector('.details').style.display === 'none') {
                this.$el.querySelector('.details').style.display = 'block';
                this.$el.querySelector('.expand-button').innerHTML = '&#9660;';
            } else {
                this.$el.querySelector('.details').style.display = 'none';
                this.$el.querySelector('.expand-button').innerHTML = '&#9658;';
            }
        },

        delete_room() {
            var deleteButton = this.$el.querySelector('#delete_room');
            deleteButton.disabled = true
            axios.delete('localhost:4000/api/rooms/' + this.room.id, {
                auth: {
                    username: 'mahdi',
                    password: 'user'
                }
            }).then(response => {
                deleteButton.disabled = false
                this.$el.remove();

            } ).catch(error => {
                deleteButton.disabled = false
                console.log(error);
            })
            

        },
        rename_room() {
            var new_name = prompt("Enter new name");
            this.room.name = new_name;
            axios({
        method: 'post',
        url: 'https://mahditrabolsi.cleverapps.io/api/windows',
        auth: {
          username: "mahdi",
          password: "user",
        },
        data: this.window,
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
}
</script>
