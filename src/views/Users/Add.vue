<template>
  <div class="users">
    <div class="container">
      <div class="actionbar">
        <h3 align="right">إضافة مستخدم جديد</h3>
      </div>

      <form @submit.prevent="save">
        <div class="form-group">
          <label for="input-id">: رقم الهوية</label>
          <input v-model="form.empid" type="text" class="form-control" id="input-id" aria-describedby="idHelp" placeholder="أدخل رقم الهوية الخاص بالموظف" required>
        </div>
        <div class="form-group">
          <label for="input-name">: اسم الموظف</label>
          <input v-model="form.empname" type="text" class="form-control" id="input-name" aria-describedby="nameHelp" placeholder="اسم الموظف" required>
        </div>
        <div class="form-group">
          <label for="input-deptname">: الإدارة</label>
          <input v-model="form.deptname" type="text" class="form-control" id="input-deptname" aria-describedby="deptnameHelp" placeholder="أدخل إلإدارة التي يتبعها الموظف">
        </div>
        <div class="form-group">
          <label for="input-email">: البريد الالكتروني</label>
          <input v-model="form.email" type="email" class="form-control" id="input-email" aria-describedby="emailHelp" placeholder="أدخل البريد الالكتوني" required>
        </div>
        <div class="form-group">
          <label for="input-ext">: التحويلة</label>
          <input v-model="form.ext" type="text" class="form-control" id="input-ext" aria-describedby="extHelp" placeholder="أدخل رقم التحويلة">
        </div>
        <div class="form-group">
          <label for="input-resume">: السيرة الذاتية</label><br>
          <input type="file" @change="onFileChanged" id="input-resume"><br>
          <button @click="onUpload">Upload!</button>
        </div>
        <div class="form-group">
          <label for="input-profile_picture">: الصورة الشخصية</label><br>
          <input type="file" @change="onFileChanged" id="input-profile_picture"><br>
          <button @click="onUpload">Upload!</button>
        </div>
        <button type="submit" class="btn btn-primary">حـفظ</button>
        <button type="button" class="btn btn-link" @click="goBack">إلـغاء</button>
      </form>
    </div>
  </div>
</template>

<script>
  import {add} from "../../services/EmployeeService";
  import axios from 'axios';
  export default {
    data() {
      return {
        form: {
          empid: '',
          empname: '',
          deptname: '',
          email: '',
          ext: '',
          profile_picture: null,
          resume: null
        }
      }
    },
    methods: {
      onFileChanged (event) {
        const files = event.target.files || event.dataTransfer.files;
        if (!files.length)
          return;
        this.createImage(files[0]);
      },
      createImage(file) {
        const image = new Image();
        const reader = new FileReader();
        const vm = this;
        reader.onload = (e) => {
          vm.image = e.target.result;
        };
        reader.readAsDataURL(file);
      },
      onUpload(){
        const formData = new FormData()
        formData.append('myFile', this.profile_picture, this.profile_picture.name)
        axios.post('http://192.168.0.192:818/add-employee/', formData)
      },
      save() {
        add(this.form).then(res => console.log(res));
        this.goBack();
      },
      goBack() {
        this.$router.push({name: 'users'});
      }
    }
  }
</script>

<style>
  form {
    text-align: right;
  }
  input[type=text], input[type=email] {
    text-align: right;
  }
  .form-group label {
    margin-bottom: 2px;
  }
  .actionbar {
    margin-bottom: 20px;
    text-align: left;
  }
</style>
