<template>
  <div class="mb-20">
    <div class="flex items-center justify-between mb-10">
      <h1>User list</h1>
      <el-button type="primary" @click="createUser">Create User</el-button>
    </div>

    <el-table v-loading="loading" :data="users" style="width: 100%">
      <el-table-column type="index" :index="indexMethod" width="50" />
      <el-table-column prop="firstName" label="First Name" />
      <el-table-column prop="lastName" label="Last Name" />
      <el-table-column prop="age" label="Age" />
      <el-table-column prop="phone" label="Phone" />
      <el-table-column fixed="right" label="Operations">
        <template #default="{ row }">
          <el-button
            link
            type="primary"
            size="small"
            @click="sendToUpdate(row.id)"
            >Edit</el-button
          >
          <el-popconfirm
            title="Are you sure to delete this?"
            @confirm="deleteUser(row.id)"
          >
            <template #reference>
              <el-button class="!text-red-500 !border-none bg-"
                >Delete</el-button
              >
            </template>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>

    <div class="flex justify-end mt-10">
      <el-pagination
        v-model="currentPage"
        @current-change="handleCurrentChange"
        background
        layout="total, sizes, prev, pager, next"
        :total="totalUsers"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import api from "../../api";

const users = ref([]);
const loading = ref(false);
const totalUsers = ref(0);
const pageSize = ref(10);
const currentPage = ref(1);

const router = useRouter();

const handleCurrentChange = (page) => {
  currentPage.value = page;
  fetchUsers();
};

const indexMethod = (index) => {
  return (currentPage.value - 1) * pageSize.value + index + 1;
};

const sendToUpdate = (id) => {
  router.push({ name: "edit-user", params: { id } });
};
const createUser = () => {
  router.push({ name: "create-user" });
};

const fetchUsers = () => {
  loading.value = true;
  api
    .get("/users", {
      params: {
        limit: pageSize.value,
        skip: (currentPage.value - 1) * pageSize.value,
      },
    })
    .then((res) => {
      users.value = res.data.users;
      totalUsers.value = res.data.total;
    })
    .catch((err) => {
      console.log("err", err);
    })
    .finally(() => {
      loading.value = false;
    });
};

const deleteUser = async (id) => {
  try {
    await api.delete(`/users/${id}`);
    fetchUsers(); // Refresh the user list after deletion
    console.log(`User with ${id} deleted successfully`);
  } catch (error) {
    console.error("Error deleting user:", error);
  }
};

fetchUsers();

const handleClick = () => {
  console.log("click");
};
</script>

<style lang="scss" scoped></style>
