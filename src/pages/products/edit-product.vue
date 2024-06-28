<template>
    <div>
        <h1>Product Create</h1>
        <el-form label-position = "top"
            ref="ruleFormRef"
            :model="newForm"
            :rules="rules"
            label-width="auto"
            class="demo-ruleForm">
            <el-form-item label="Product title" prop="title">
                <el-input v-model="newForm.title" />
            </el-form-item>
                    <el-form-item label="Product description" prop="description">
                <el-input v-model="newForm.description" />
            </el-form-item>
                    <el-form-item label="Product price" prop="price">
                <el-input v-model="newForm.price" />
            </el-form-item>
                    <el-form-item label="Product image url" prop="image">
                <el-input v-model="image" />
            </el-form-item>
            <el-button type="primary" @click="submitForm(ruleFormRef)">
        Create
      </el-button>
      <el-button @click="resetForm(ruleFormRef)">Reset</el-button>
        </el-form>
    </div>
</template>

<script setup>

import { reactive, ref } from 'vue'
import axios from 'axios'
import { useRouter, useRoute } from "vue-router";


const router = useRouter();
const route = useRoute();

const newForm = reactive({
    title: '',
    description: '',
    price: '',
    image: '',
})

const ruleFormRef = ref(null)

const rules = reactive({
    title: [
        { required: true, message: 'Please input Activity title', trigger: 'blur' },
    ],
    description: [
        { required: true, message: 'Please input Activity descrition', trigger: 'blur' },
    ], 
    price: [
        { required: true, message: 'Please input Activity price', trigger: 'blur' },
    ],  
    image: [
        { required: true, message: 'Please input Activity image url', trigger: 'blur' },
    ]
})

const submitForm = async (formEI) => {
    if (!formEI) return
    await formEI.validate((valid, fields) => {
        if (valid) {
             axios.put(`https://65af3f432f26c3f2139a4d9f.mockapi.io/api/v1/products/{router.id}`,newForm.value )
            router.push('/products')
        } else {
            console.log('error submit!', fields)
        }
    })
}

const resetFrom = (formEI) => {
    if (!formEI) return
    formEI.resetFields()
}


</script>

<style lang="scss" scoped>

</style>