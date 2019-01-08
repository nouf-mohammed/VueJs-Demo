<template>
    <div>
    <table aria-rowcount="30">
        <thead >
        <tr>
            <th scope="col">حذف</th>
            <th scope="col">تعديل</th>
            <th scope="col">السيرة الذاتية </th>
            <th scope="col">الصورة الشخصية</th>
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
            <td><button type="button" @click="del(user.pk)"><i class="fa fa-trash"></i></button></td>
            <td><router-link :to="'/user/'+user.pk"><i class="fa fa-edit"></i></router-link></td>
            <td><div v-if="user.profile_picture !=null">
                <i class="fa fa-paperclip"></i>
            </div></td>
            <td><div v-if="user.resume !=null">
                <i class="fa fa-paperclip"></i>
            </div></td>
            <td>{{user.ext}}</td>
            <td>{{user.deptname}}</td>
            <td>{{user.email}}</td>
            <td>{{user.empname}}</td>
            <td>{{user.empid}}</td>
            <th scope="row">{{index+1}}</th>
        </tr>
        </tbody>
    </table><br><br>
    <br><br>


        <p>Current page: {{ currentPage }}</p>
        <v-pagination v-model="currentPage"
                      :page-count="totalPages"
                      :classes="bootstrapPaginationClasses"
                      :labels="paginationAnchorTexts" length="40"></v-pagination>
    </div>
</template>

<script>
    import vPagination from 'vue-plain-pagination'
    import {del, loadEmployees} from "../services/EmployeeService";
    export default {
        components: { vPagination },
        data() {
            return {
                users: [],
                searchName : '',
                currentPage: 1,
                totalPages: 40,
                bootstrapPaginationClasses: {
                    ul: 'pagination',
                    li: 'page-item',
                    liActive: 'active',
                    liDisable: 'disabled',
                    button: 'page-link'
                },
                paginationAnchorTexts: {
                    first: 'First',
                    prev: 'Previous',
                    next: 'Next',
                    last: 'Last'
                }
            }
        },
        mounted() {
            loadEmployees();
        },
        methods: {
            searchEmp(name){

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
