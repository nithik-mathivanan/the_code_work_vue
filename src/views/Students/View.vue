<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h4>
          Students
          <RouterLink to="/student/create" class="btn btn-primary float-end">
            Add Student
          </RouterLink>
        </h4>
        <div class="class-body">
          <table class="table table-bordered">
            <thead>
              <tr>
                <th>ID</th>
                <th>Name</th>
               
                <th>Email</th>
                <th>Phone</th>
                <th>Created At</th>
                <th>Action</th>
              </tr>
            </thead>
            <tbody v-if="this.students.length > 0">
              <tr v-for="(student, index) in this.students" :key="index">
                <td>{{ student.id }}</td>
                <td>{{ student.name }}</td>
                <td>{{ student.email }}</td>
                <td>{{ student.phone }}</td>
                <td>{{ student.created_at }}</td>
                <td> <RouterLink :to="{ path: '/student/'+student.id+'/edit' }" class="btn btn-primary ">
                  edit
                 </RouterLink>
                 <button type="button" class="btn btn-danger" >delete</button></td>
              </tr>
            </tbody>
            <tbody v-else>
              <tr v-for="(student, index) in this.students" :key="index">
                <td colspan="6">Loading...</td>
               
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'students',
  data() {
    return {
      students: []
    };
  },
  mounted() {
  this.getStudents();
   // console.log('I am here');
  },
  methods: {
  getStudents(){
  axios.get('http://localhost/the_code_works/api/student').then(res => {
  this.students = res.data.students
    console.log(this.students)
  });
  }
  },
};
</script>
