<template>
  <div class="box">
    <div class="row">
      <div class="col" v-if="isLoading">Loading...</div>
      <div class="col" v-else>
        <div class="row">
          <div class="col">{{ average_value.toFixed(2) }} {{ selected.type_unit }}</div>
        </div>
        <div class="row">
          <div class="col">
            <select v-model="selected" name="select-form" class="form-select">
              <option disabled selected>Please select</option>
              <option v-for="option in types" :value="option" :key="option.id">{{ option.type_name }}</option>
            </select>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <input type="checkbox" id="checkbox" v-model="checked" />
            <label for="checkbox">Show Date Range</label>
          </div>
        </div>
        <div class="row" v-if="checked">
          <div class="col">
            <div>Start: <input type="datetime-local" id="start" v-model="start" placeholder="start" class="form-control"></div>
            <div>End: <input type="datetime-local" id="end" v-model="end" placeholder="end" class="form-control"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup lang="ts">
import axios from 'axios';
import { ValueType } from '../scripts/value_type'
import { ref, watch, onMounted } from 'vue';

const average_value = ref(0);
const selected = ref({} as ValueType);
const types = ref([] as ValueType[]);
const isLoading = ref(false);
const checked = ref(false);

const start_date = new Date()
const formatted_start_date = start_date.toISOString().slice(0, 16)
const end_date = new Date()
const formatted_end_date = end_date.toISOString().slice(0, 16)

const start = ref(formatted_start_date)
const end = ref(formatted_end_date)

const update = async () => {
  try {
    isLoading.value = true;
    const url = `/api/average/${selected.value.id}/`;
    let params = {};
    if (checked.value) {
      params = {
        start: Math.floor(Date.parse(start.value) / 1000),
        end: Math.floor(Date.parse(end.value) / 1000),
      }
    }
    const response = await axios.get(url, { params });
    average_value.value = response.data;
  } catch (error) {
    console.error(error);
  } finally {
    isLoading.value = false;
  }
};

const get_types = async () => {
  try {
    isLoading.value = true;
    const url = '/api/type/';
    const response = await axios.get(url);
    types.value = response.data;
  } catch (error) {
    console.error(error);
  } finally {
    isLoading.value = false;
  }
};

watch(selected, update);
watch(checked, update);
watch(start, update);
watch(end, update);

onMounted(() => {
  if (types.value.length === 0) {
    get_types();
  }
  selected.value = types.value[0] || {};
});
</script>

<script lang="ts">
</script>

<style>
.box {
    position: fixed;
    right: 0;
    padding: 40px;
    margin: 20px;
    background-color: rgb(0,0,0,0.5);
    border-radius: 10px;
}
</style>
