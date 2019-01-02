<template>
  <div class="users">
    <div class="container">
      <div class="actionbar">
        <h3 align="right">تعديل مستخدم</h3>
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
          <input v-model="form.email" type="email" class="form-control" id="input-email" aria-describedby="emailHelp" placeholder="أدخل البريد الالكتوني">
        </div>
        <div class="form-group">
          <label for="input-ext">: التحويلة</label>
          <input v-model="form.ext" type="text" class="form-control" id="input-ext" aria-describedby="extHelp" placeholder="أدخل رقم التحويلة">
        </div>
        <div class="form-group">
          <label for="input-image"> : الصورة الشخصية</label>
          <div id="input-image" style="display: flex; justify-content: center;">
            <vue-upload-multiple-image
                    @upload-success="uploadImageSuccess"
                    @before-remove="beforeRemove"
                    @edit-image="editImage"
                    @data-change="dataChange"
                    :data-images="profile_picture"
            ></vue-upload-multiple-image>
          </div>
        </div>
        <button type="submit" class="btn btn-primary">حـفظ</button>
        <button type="button" class="btn btn-link" @click="goBack">إلـغاء</button>
      </form>
    </div>
  </div>
</template>

<script>
  import {get, update} from "../../services/EmployeeService";
  import VueUploadMultipleImage from 'vue-upload-multiple-image'
  import axios from 'axios'
  export default {
      data() {
          return {
              form: {
                  pk: '',
                  empid: '',
                  empname: '',
                  deptname: '',
                  email: '',
                  ext: '',
                  profile_picture: [],
                  resume: null
              }
          }
      },
      mounted() {
          const pk = this.$route.params.pk;
          get(pk).then(res => this.form = res.data);
      },
    components: {
      VueUploadMultipleImage
    },
      methods: {
        uploadImageSuccess(formData, index, fileList) {
          console.log('data', formData, index, fileList)
          // Upload image api
          axios.put(`http://192.168.0.192:818/employee/${this.form.pk}/update/`, { data: formData }).then(response => {
            console.log(response)
          })
        },
        beforeRemove (index, done, fileList) {
          console.log('index', index, fileList)
          const r = confirm("remove image");
          if (r == true) {
            done()
          } else {
          }
        },
        editImage (formData, index, fileList) {
          console.log('edit data', formData, index, fileList)
        },
        dataChange (data) {
          console.log(data)
        },
          save() {
              update(this.form.pk, this.form).then(res => console.log(res));
              this.goBack();
          },
          goBack() {
              this.$router.push({name: 'users'});
          }
      }
  }
</script>

<style>
  input[type=text], input[type=email] {
    text-align: right;
  }
  form {
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

