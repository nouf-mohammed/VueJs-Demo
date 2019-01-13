<template>
  <div class="users">
    <div class="actionbar">

  </div>
    <router-link to="/users/add" class="btn btn-dark">إضافة مستخدم</router-link><br><br>

    <div class="container">

      <form class="form-inline active-cyan-3 active-cyan-4">
         <button @click.prevent="searchEmp" class="unstyled-button" type="submit"><i class="fa fa-search" aria-hidden="true"></i></button>
        <input v-model="query" class="form-control form-control-sm ml-3 w-75" type="text" placeholder="Search">
      </form>
      <br>

      <table>
        <thead >
        <tr>
          <th scope="col">حذف</th>
          <th scope="col">تعديل</th>
          <th scope="col">السيرة الذاتية </th>
          <th scope="col">الصورة الشخصية</th>
          <th scope="col">التحويلة </th>
          <th scope="col">الإدارة</th>
          <th scope="col">البريد الالكتروني</th>
          <th scope="col"> الأسم </th>
          <th scope="col">رقم الهوية</th>
          <th scope="col">#</th>
        </tr>
        </thead>
        <tbody>
          <tr v-for="(user, index) in this.$store.state.users">
            <td><button type="button" @click="del(user.pk)"><i class="fa fa-trash"></i></button></td>
            <td><router-link :to="'/user/'+user.pk"><i class="fa fa-edit"></i></router-link></td>
            <td>
              <span v-if="user.resume != null">
                <i class="fa fa-paperclip"></i>
              </span>
            </td>
            <td>
              <div v-if="user.resume !=null">
                <i class="fa fa-paperclip"></i>
              </div>

            </td>
            <td>{{user.ext}}</td>
            <td>{{user.deptname}}</td>
            <td>{{user.email}}</td>
            <td>{{user.empname}}</td>
            <td>{{user.empid}}</td>
            <th scope="row">{{index+1}}</th>
          </tr>
        </tbody>
      </table><br><br>
      <br>
    </div>
  </div>
</template>

<script>
  import {del, loadEmployees, search} from "../../services/EmployeeService";
  import store from '@/store'
  export default {
    data(){
      return{
        users: [],
        query:''
      }
    },
    mounted() {
       loadEmployees();
    },
    methods: {
      searchEmp() {
        var query = this.query;
        search(query).then(res => {
          this.$store.state.users = res.data.result ;
        })
      },
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
  .unstyled-button {
    border: none;
    padding: 0;
    background: none;
    cursor: pointer;
  }
</style>
