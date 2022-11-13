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

                <button type="button" style="margin-left: 10px;" class="btn btn-danger" @click="delete_room">Delete room</button>
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
            axios.delete('http://mahditrabolsi.cleverapps.io/api/rooms/' + this.room.id, {
                auth: {
                    username: 'mahdi',
                    password: 'user'
                }
            }).then(response => {
                this.$el.remove();
            })
        }
    },
}
</script>
