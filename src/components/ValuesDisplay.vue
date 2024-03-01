<script lang="ts">
import { Value } from '@/scripts/value'

export default {
  props: ['values', 'value_types', 'order', 'asc'],
  setup(props) {
    console.log(props.values)
  },
  emits: ['set-order'],
  methods: {
    getTypeName(value: Value) {
      for (var i = 0; i < this.value_types.length; i++) {
        if (this.value_types[i].id == value.value_type_id) {
          return this.value_types[i].type_name
        }
      }
      return 'XXX'
    },
    getUnit(value: Value) {
      for (var i = 0; i < this.value_types.length; i++) {
        if (this.value_types[i].id == value.value_type_id) {
          return this.value_types[i].type_unit
        }
      }
      return 'XXX'
    },
    formatTime(timestamp: any) {
      const date = new Date(timestamp * 1000);
      const options = { timeZone: 'UTC', hour12: false };
      return date.toLocaleString('en-US', options);
    }
  }
}
</script>

<template>
  <div class="row bg-primary mt-2 mb-1">
    <div
      class="col-2"
      data-bs-toggle="tooltip"
      data-bs-placement="top"
      data-bs-title="Tooltip on top"
      @click="$emit('set-order', 'time')"
    >
      time
      <template v-if="order === 'time'">
        <i class="bi bi-caret-down-fill" title="Show TypeEditor" v-if="asc"></i>
        <i class="bi bi-caret-up-fill" title="Hide TypeEditor" v-else></i>
      </template>
    </div>
    <div class="col-1" @click="$emit('set-order', 'value_type_id')">type
      <template v-if="order === 'value_type_id'">
        <i class="bi bi-caret-down-fill" title="Show TypeEditor" v-if="asc"></i>
        <i class="bi bi-caret-up-fill" title="Hide TypeEditor" v-else></i>
      </template>
    </div>
    <div class="col-2" @click="$emit('set-order', 'value')">value
      <template v-if="order === 'value'">
        <i class="bi bi-caret-down-fill" title="Show TypeEditor" v-if="asc"></i>
        <i class="bi bi-caret-up-fill" title="Hide TypeEditor" v-else></i>
      </template>
    </div>
    <div class="col"  @click="$emit('set-order', 'device_id')">device
      <template v-if="order === 'device_id'">
        <i class="bi bi-caret-down-fill" title="Show TypeEditor" v-if="asc"></i>
        <i class="bi bi-caret-up-fill" title="Hide TypeEditor" v-else></i>
      </template>
    </div>
  </div>
  <div class="row bg-secondary rounded mt-1" v-for="value in values" :key="value">
    <div class="col-2">
      {{ formatTime(value.time) }}
    </div>
    <div class="col-1">
      {{ getTypeName(value) }} 
    </div>
    <div class="col-2">{{ value.value.toFixed(2) }} {{ getUnit(value) }}</div>
    <div class="col">{{ value.device_id }}</div>
  </div>
</template>
