<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <h4>Update Student Details</h4>
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
          <label>Name</label>
          <input type="text" v-model="model.student.name" class="form-control" />
        </div>
        <div class="mb-3">
          <label>Email</label>
          <input type="text" v-model="model.student.email" class="form-control" />
        </div>
        <div class="mb-3">
          <label>Phone Number</label>
          <input type="text" v-model="model.student.phone" class="form-control" />
        </div>

        <!-- Submit button to save student data -->
        <div class="mb-3">
          <button type="button" @click="updateStudent" class="btn btn-primary">Update</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'StudentEdit',
  data() {
    return {
      errorList: {}, // Initialize as an empty object
      studentId: '', // Initialize studentId
      model: {
        student: {
          name: '',
          email: '',
          phone: ''
        }
      }
    }
  },
  mounted() {
    // Get student ID from route parameters
    this.studentId = this.$route.params.id;
    this.getStudentData(this.studentId);
  },
  methods: {
    getStudentData(studentId) {
      axios.get(`http://localhost/the_code_works/api/student/${studentId}/edit`)
        .then(res => {
          console.log(res.data.message);
          // Assuming res.data.message contains the student data
          this.model.student = res.data.message; 
        })
        .catch(error => {
          console.error('Error fetching student data:', error);
        });
    },

    updateStudent() {
      // Make the API call to update student data
      axios
        .put(`http://localhost/the_code_works/api/student/${this.studentId}/edit`, this.model.student) // Use backticks for template strings
        .then((res) => {
          console.log(res.data);
          this.errorList = {}; // Clear errors on successful update
          alert('Student details updated successfully!');
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
