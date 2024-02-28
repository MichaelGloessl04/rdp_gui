<template>
    <div v-if="isLoading">Loading...</div>
    <div v-else class="box">
        <div>
            <select v-model="selected" name="select-form">
                <option disabled value="">Please select</option>
                <option v-for="option in types" :value="option" :key="option.id">
                    {{ option.type_name }}
                </option>
            </select>
        </div>
        <div>Average: {{ average_value.toFixed(2) }} {{ selected.type_unit }}</div>
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

const update = async () => {
  try {
    isLoading.value = true;
    const url = `/api/average/${selected.value.id}/`;
    const response = await axios.get(url);
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
