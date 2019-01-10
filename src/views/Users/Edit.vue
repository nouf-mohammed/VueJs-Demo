<template>
  <div class="users">
    <div class="container">


      <form @submit.prevent="save">

        <div class="column" >
        <div class="form-group">
          <label for="input-profile_picture">: الصورة الشخصية</label><br>
          <div v-if="form.profile_picture !=null">
            <img :src="form.profile_picture" alt="" class="center">
            <a :href="form.profile_picture" download class="fa fa-download"></a><br><br>
          </div>
          <input type="file"  id="input-profile_picture" ref="profilePicture" accept="image/*" class="form-control" required><br>
        </div>
        </div>

       <div class="columnn" >
         <div class="actionbar">
           <h3 align="right">تعديل مستخدم</h3>
         </div>
        <div class="form-group">
          <label for="input-id">: رقم الهوية</label>
          <input v-model="form.empid" type="text" class="form-control" id="input-id" aria-describedby="idHelp" placeholder="أدخل رقم الهوية الخاص بالموظف" disabled>
        </div>

        <div class="form-group">
          <label for="input-name">: اسم الموظف</label>
          <input v-model="form.empname" type="text" class="form-control" id="input-name" aria-describedby="nameHelp" placeholder="اسم الموظف" required>
        </div>

        <div class="form-group">
          <label for="input-deptname">: الإدارة</label>
            <select id="input-deptname" v-model="form.deptname" class="form-control">
                <option value="" disabled selected>الإدارة التي يتبعها الموظف</option>
                <option>إدارة ضمان الجودة </option><option>مكتب نائب الرئيس للأعمال الإحصائية</option><option>إدارة التوثيق والأرشفة </option>
                <option>إدارة إحصاءات البيئة</option><option>إدارة المشتريات  </option><option>الإدارة العامة للإعلام والوعي الإحصائي </option>
                <option>إدارة إحصاءات الخدمات الحكومية و الجغرافية</option><option>وكيل الخدمات الإحصائية </option><option>إدارة إحصاءات الزراعة</option>
                <option>الإدارة العامة للتقارير العامة</option><option>إدارة المرافق والخدمات الإدارية</option><option>الإدارة العامة للفروع</option>
                <option>إدارة تنسيق العمل الميداني</option><option>وكيل الشؤون الإدارية والمالية</option><option>الإدارة  العامة للعلاقات ودعم العملاء</option>
                <option>إدارة إستراتيجية الموارد البشرية</option><option>الإدارة العامة للإبتكار والتطوير الإحصائي</option><option>إدارة إحصاءات الأسعار</option>
                <option>إدارة الاتصالات الإدارية </option><option>إدارة إحصاءات التجارة الخارجية</option><option>مكتب نائب الرئيس للأعمال الإحصائية</option>
                <option>الإدارة العامة للمشاريع والتخطيط الإستراتيجي </option><option>إدارة الشبكات والإتصالات</option><option>إدارة الدعم الفني</option>
                <option>إدارة دعم قواعد البيانات</option><option>إدارة تطوير التطبيفات</option><option>إدارة الخدمات الالكترونية وإدارة البيانات</option>
                <option>الإدارة العامة للإعلام والوعي الإحصائي</option><option>إدارة دعم المنهجيات الإحصائية</option><option>إدارة إحصاءات المال و الاستثمار</option>
                <option>الإدارة العامة للبنية التحتية</option><option>الإدارة العامة للتحليل الاحصائي</option><option>القسم النسوي</option>
            </select>
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
          <input type="file" id="input-resume" class="form-control" ref="resume" accept="application/pdf" required>
          <div v-if="form.resume !=null">
            <a :href="form.resume" download class="fa fa-download"></a>
          </div>
        </div>

        <button type="submit" class="btn btn-primary">حـفظ</button>
        <button type="button" class="btn btn-link" @click="goBack">إلـغاء</button>
       </div>
      </form>
    </div>
  </div>
</template>

<script>
  import {get, update} from "../../services/EmployeeService";
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
                  profile_picture: null,
                  resume: null
              }
          }
      },
      mounted() {
          const pk = this.$route.params.pk;
          get(pk).then(res => this.form = res.data);
      },
      methods: {
          save() {
            var photo = this.$refs.profilePicture.files[0];
            var resume = this.$refs.resume.files[0];

            var fd = new FormData();
            fd.append('empid', this.form.empid);
            fd.append('empname', this.form.empname);
            fd.append('deptname', this.form.deptname);
            fd.append('email', this.form.email);
            fd.append('ext', this.form.ext);
            fd.append('profile_picture', photo);
            fd.append('resume', resume);


            update(this.form.pk, fd, {
              headers: {
                'Content-Type': 'multipart/form-data'
              }
            }).then(res => console.log(res));
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
  input[type=file] {
    text-align: left;
  }
  select{
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
  .center {
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 200px;
  }
  .column {
    float: right;
    width: 20%;
    padding: 5px;
    height: 300px; /* Should be removed. Only for demonstration */
  }
  .columnn {
    float: left;
    width: 80%;
    padding: 5px;
    height: 300px; /* Should be removed. Only for demonstration */
  }
</style>

