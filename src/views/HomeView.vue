<template>
    <div class="flex h-screen">
      <div class="m-auto container">
        <div class="mockup-window border border-base-300" data-theme="aqua">
          <div class="flex justify-center px-4 py-16 border-t border-base-300">
            <table class="table text-lg">
              <thead class="text-xl">
                <tr>
                  <th>#</th>
                  <th>Name</th>
                  <th>Price</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(product, index) in products" :key="product._id">
                  <th>{{ index + 1 }}</th>
                  <td>{{ product.name }}</td>
                  <td>{{ product.price }}</td>
                  <td>
                    <RouterLink :to="{name: 'edit',params:{id: product._id}}" class="btn btn-outline"> Edit</RouterLink>
                    <!-- <RouterLink :to="{name: 'delete',params:{id: product._id}}" @click="destroy(product._id)" class="btn btn-error"> Delete</RouterLink> -->
                    <button @click="destroy(product._id)" class="btn btn-error">Delete</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import axios from "axios";
  import { ref, onMounted } from "vue";
  const products = ref([]);
  onMounted(async () => {
    products.value = await getAllProduct();
  });
  const API_PATH = import.meta.env.VITE_API_PATH;
  const getAllProduct = async () => {
    let data = [];
    data = await axios
      .get(`${API_PATH}/getallproduct`)
      .then((response) => {
        return response.data;
      });
    return data;
  };
  const destroy = async (pro_id) => {
  await axios.delete(`${API_PATH}/destory/${pro_id}`).then((response) => {
    window.location.reload();
  });
}
  </script>
  
  <style scoped>
  </style>