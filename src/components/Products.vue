<script setup>
import * as Vue from "vue";
import { createApp } from "vue";
import EasyDataTable from "vue3-easy-data-table";
import "vue3-easy-data-table/dist/style.css";
import { ref, onMounted, watch } from "vue";
import axios from "axios";
import FileUpload from "./FileUpload.vue";

const app = createApp();
const products = ref([]);
const searchField = ref([
  "ProductID",
  "Types",
  "Brand",
  "Model",
  "Capacity",
  "Quantity"
]);
const searchValue = ref("");

onMounted(async () => {
  products.value = await axios({
    method: "get",
    url: "http://localhost:8002/api/v1/product/list"
  })
    .then(function(response) {
      return response.data;
    })
    .catch(error => {
      return error.response.data.error;
    });
});

products.value = watch(products, () => {
  products;
});

const headers = [
  { text: "Product ID", value: "ProductID", sortable: true },
  { text: "Types", value: "Types", sortable: true },
  { text: "Brand", value: "Brand", sortable: true },
  { text: "Model", value: "Model", sortable: true },
  { text: "Capacity", value: "Capacity", sortable: true },
  { text: "Quantity", value: "Quantity", sortable: true }
];
</script>

<template>
  <div class="container col-lg-12">
    <div class="row">
      <FileUpload />
    </div>
    <div class="row mt-3 mb-3">
      <div class="col-md-6">
        <h5>Product Master List</h5>
      </div>
      <div class="col-md-3">
        <label>search value:</label>
      </div>

      <div class="col-md-3">
        <input type="text" class="col-md-12" v-model="searchValue" />
      </div>
    </div>
    <div class="row">
      <EasyDataTable
        theme-color="#1d90ff"
        table-class-name="customize-table"
        header-text-direction="center"
        body-text-direction="center"
        :search-field="searchField"
        :search-value="searchValue"
        v-if="products?.data?.data"
        :headers="headers"
        :items="products?.data?.data"
      />
    </div>
  </div>
</template>
