<template>
  <div>
    <h1>User Edit</h1>
    <el-input v-model="input" style="width: 240px" placeholder="name" />
    <el-input v-model="inputfam" style="width: 240px" placeholder="familiya" />
    <el-input v-model="inputphone" style="width: 240px" placeholder="phone" />
    <el-input v-model="inputage" style="width: 240px" placeholder="age" />
    <el-popconfirm title="Are you sure to save this?"
    @confirm="updateUser">

      <template #reference>
        <el-button>Save</el-button>
      </template>
    </el-popconfirm>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import axios from "axios";

const route = useRoute();
const userId = route.params.id;

const input = ref("");
const inputfam = ref("");
const inputphone = ref("");
const inputage = ref("");

// Fetch user data on component mount
onMounted(async () => {
  try {
    const response = await axios.get(`https://dummyjson.com/users/${userId}`);
    const userData = response.data;
    input.value = userData.firstName;
    inputfam.value = userData.lastName;
    inputphone.value = userData.phone;
    inputage.value = userData.age;
  } catch (error) {
    console.error("Error fetching user data:", error);
  }
});

const updateUser = async () => {
  try {
    const response = await axios.put(`https://dummyjson.com/users/${userId}`, {
      firstName: input.value,
      lastName: inputfam.value,
      phone: inputphone.value,
      age: inputage.value,
    });
    console.log("Data updated successfully:", response.data);
  } catch (error) {
    console.error("Error updating data:", error);
  }
};
</script>

<style lang="scss" scoped></style>
