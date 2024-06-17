<template>
  <div>
    <form @submit.prevent="handleSubmit">
      <input type="file" ref="fileInput" @change="handleFileChange">
      <button class="btn btn-success" type="submit"><i class="bi bi-sliders"></i>&nbsp;&nbsp;Upload</button>
    </form>
    
    <!-- Error div to display API response or error -->
    <div v-if="apiResponse" class="alert alert-success">
      {{ apiResponse }}
    </div>
    <div v-if="apiError" class="alert alert-danger">
      {{ apiError }}
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      apiResponse: null,
      selectedFile: null,
      apiError: null // Initialize apiError here
    };
  },
  methods: {
    
    handleFileChange(event) {
      this.selectedFile = event.target.files[0];
    },
          
    async handleSubmit() {

      if (!this.selectedFile) {
        this.apiError = 'Please select a file.';
        return;
      }

      const formData = new FormData();
      formData.append('productCsv', this.selectedFile);        
      
      const response = await axios.post('http://localhost:8002/api/v1/product/upload', formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    })
    .then(response => {
      console.log('File uploaded successfully:', response.data);
      this.uploading = false;
      this.uploadSuccess = true;
      // Optionally reset the file input
      this.$refs.fileInput.value = null;
      this.apiError = '';
      this.apiResponse = response.data.message;
    })
    .catch(error => {
      console.error('Error uploading file:', error);
      this.uploading = false;
      his.apiError = 'Error uploading file. Please try again.';
    });
    
    }
  }
}
</script>

<style>
.alert {
  margin-top: 20px;
  padding: 10px;
  border-radius: 4px;
}
.alert-success {
  background-color: #d4edda;
  border-color: #c3e6cb;
  color: #155724;
}
.alert-danger {
  background-color: #f8d7da;
  border-color: #f5c6cb;
  color: #721c24;
}
</style>
