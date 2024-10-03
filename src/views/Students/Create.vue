<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <h4>Add New Students</h4>
      </div>
      <div class="card-body">
        <!-- Alert box for displaying error messages -->
        <ul class="alert alert-warning" v-if="Object.keys(errorList).length > 0">
          <!-- Iterate through errorList to display each error message -->
          <li class="mb-0 ms-3" v-for="(error, index) in errorList" :key="index">
            {{ error[0] }}
          </li>
        </ul>

        <!-- Form fields for student data input -->
        <div class="mb-3">
          <label for="">Name</label>
          <input type="text" v-model="model.student.name" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Email</label>
          <input type="text" v-model="model.student.email" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Phone Number</label>
          <input type="text" v-model="model.student.phone" class="form-control" />
        </div>

        <!-- Submit button to save student data -->
        <div class="mb-3">
          <button type="button" @click="saveStudent" class="btn btn-primary">Submit</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'StudentCreate',
  data() {
    return {
      errorList: {}, // Initialize as an empty object
      model: {
        student: {
          name: '',
          email: '',
          phone: ''
        }
      }
    };
  },
  methods: {
    saveStudent() {
      // Make the API call to save student data
      axios
        .post('http://localhost/the_code_works/api/student', this.model.student)
        .then((res) => {
          console.log(res.data);

          // Reset the student form fields after successful submission
          this.model.student = {
            name: '',
            email: '',
            phone: ''
          };

          // Clear any previous error messages
          this.errorList = {};
        })
        .catch((error) => {
          // Handle validation errors
          if (error.response && error.response.status === 422) {
            this.errorList = error.response.data.error;
          } else if (error.request) {
            console.log(error.request);
          } else {
            console.log('Error', error.message);
          }
        });
    }
  }
};
</script>
