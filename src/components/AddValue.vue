<script setup lang="ts">
import axios from 'axios';
</script>

<template>
    <div class="row">
        <div>
            <label name="value-label">Value:</label>
            <input type="number" v-model="value" placeholder="0">
        </div>
        <div>
            <label name="type-label">Type:</label>
            <input type="number" v-model="type" placeholder="0">
        </div>
        <div>
            <label name="device-label">Device:</label>
            <input type="number" v-model="device" placeholder="0">
        </div>
        <button name="submit" class="col-1" @click="add_value">Submit</button>
    </div>
</template>

<script lang="ts">
export default {
    data() {
        return {
            value: 0.0,
            type: 0,
            device: 0
        }
    },
    methods: {
        add_value() {
            const params = {
                value_value: this.value,
                value_type: this.type,
                value_device: this.device
            };
            console.log(params);
            axios.post("/api/value/", params )
                .then((result) => {
                    console.log(result);
                })
                .catch(error => {
                    console.error(error);
                });  
        }
    }
}
</script>