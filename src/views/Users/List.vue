<template>
  <div class="users">
    <div class="actionbar">

  </div>
    <router-link to="/users/add" class="btn btn-dark">إضافة مستخدم</router-link><br><br>
    <div class="container">

      <table>
        <thead >
        <tr>
          <th scope="col">تعديل</th>
          <th scope="col">حذف</th>
          <th scope="col">الصورة الشخصية </th>
          <th scope="col">السيرة الذاتية </th>
          <th scope="col">التحويلة </th>
          <th scope="col">الإدارة</th>
          <th scope="col">البريد الالكتروني</th>
          <th scope="col">الأسم</th>
          <th scope="col">رقم الهوية</th>
          <th scope="col">#</th>
        </tr>
        </thead>
        <tbody>
          <tr v-for="(user, index) in this.$store.state.users">
            <td><router-link :to="'/user/'+user.pk"><i class="fa fa-pencil"></i></router-link></td>
            <td><button type="button" @click="del(user.pk)"><i class="fa fa-trash"></i></button></td>
            <td>{{user.profile_picture}}</td>
            <td>{{user.resume}}</td>
            <td>{{user.ext}}</td>
            <td>{{user.deptname}}</td>
            <td>{{user.email}}</td>
            <td>{{user.empname}}</td>
            <td>{{user.empid}}</td>
            <th scope="row">{{index+1}}</th>
          </tr>
        </tbody>
      </table><br><br><br>
    </div>
  </div>
</template>

<script>
  import {del, loadEmployees} from "../../services/EmployeeService";

  export default {
    data(){
      return{
        sortKey: 'empname',
        reverse: false,
        search: '',
      }
    },
    mounted() {
      loadEmployees();
    },
    methods: {
      del(pk) {
        del(pk).then(res => {
          loadEmployees();
        });
      }
    }
  }
</script>

<style>
  .container .btn:hover {
    background-color: black;
    text-align: center;
  }
  .actionbar {
    margin-bottom: 20px;
    text-align: left;
  }
  table {
    width:100%;
    table-layout: auto;
  }
  table, th, td {
    border: 1px solid black;
    border-collapse: collapse;
  }
  th, td {
    padding: 10px;
  }
  table#t01 tr:nth-child(even) {
    background-color: #919191;
  }
  table#t01 tr:nth-child(odd) {
    background-color: #000000;
  }
  table#t01 th {
    background-color: #000000;
    color: #000000;
  }
</style>
