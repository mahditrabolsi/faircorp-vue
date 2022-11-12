<template>
    <div class="window border border-secondary rounded p-2 mb-2">
        <div class="top-row d-flex">
            <div class="window-name fw-bold pe-3">{{ window.name }}</div>
            <div class="room-name text-muted">{{ window.roomName }}</div>
            <div class="open-status open ms-4">
                <div v-if="window.windowStatus === 'OPEN'">
                    <span class="icon open">&#x2714;</span>
                    Open
                </div>
                <div v-else>
                    <span class="icon closed">&#x2716;</span>
                    Closed
                </div>
            </div>
            <div @click="showhide" class="expand-button ms-auto">&#9658;</div>
        </div>

        <div class="details" style="display: None;">
            <hr />
            <div class="details d-flex">
                <button type="button" class="btn btn-secondary me-2" @click="switch_window">
                    <span v-if="window.windowStatus === 'CLOSED'"> Open window</span>
                    <span v-else> Close window</span>


                </button>
                <button type="button" class="btn btn-danger disabled">Delete window</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Window',
    props: {
        window: Object
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
        switch_window() {
            var btn = this.$el.querySelector('.btn-secondary');
            if (btn.innerHTML === "Open window") {
                axios.post('http://mahditrabolsi.cleverapps.io/api/windows/switch/' + this.window.id, {}, {
                    auth: {
                        username: 'mahdi',
                        password: 'user'
                    }
                }).then(response => {
                    btn.innerHTML = "Close window";
                    this.window.windowStatus = "OPEN";
                })

            } else {

                axios.post('http://mahditrabolsi.cleverapps.io/api/windows/switch/' + this.window.id, {}, {
                    auth: {
                        username: 'mahdi',
                        password: 'user'
                    }
                }).then(response => {
                    btn.innerHTML = "Open window";
                    this.window.windowStatus = "CLOSED";
                })
            }



        }
    },
}
</script>

<style lang="scss" scoped>
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
</style>