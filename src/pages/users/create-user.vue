<template>
  <div>
    <h1>User create</h1>
    <el-input v-model="input" style="width: 240px" placeholder="name" />
    <el-input v-model="inputfam" style="width: 240px" placeholder="familiya" />
    <el-input v-model="inputphone" style="width: 240px" placeholder="phone" />
    <el-input v-model="inputage" style="width: 240px" placeholder="age" />
    <el-popconfirm title="Are you sure to save this?">
      <template #reference>
        <el-button @click="saveDatas">Save</el-button>
      </template>
    </el-popconfirm>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const input = ref("");
const inputfam = ref("");
const inputphone = ref("");
const inputage = ref("");

const saveDatas = async () => {
  try {
    const response = await axios.post("https://dummyjson.com/users/add", {
      name: input.value,
      familiya: inputfam.value,
      phone: inputphone.value,
      age: inputage.value,
    });
    console.log("Data saved successfully:", response.data);
  } catch (error) {
    console.error("Error saving data:", error);
  }
};
</script>

<style lang="scss" scoped></style>
