<template>
    <div class="login">
        <form @submit.prevent="login">
            <div class="container">
                <label for="emailInput"><b>اسم المستخدم</b></label><br>
                <input type="email" placeholder="البريد الالكتروني" id="emailInput"
                       aria-describedby="emailHelp" class="form-control" v-model="username" required>
                <br><br>
                <label for="passwordInput"><b>كلمة المرور</b></label><br>
                <input type="password" placeholder="كلمة المرور" id="passwordInput"
                       name="password" class="form-control" v-model="password" required>
                <br><br>
                <button type="submit" class="btn btn-primary" >تسجيل الدخول</button>
                <br><br>
            </div>
        </form>
    </div>
</template>

<script>
    import {login} from "@/services/UserService";

    export default {
        name: "LoginForm",
        data() {
            return {
                username: '',
                password: ''
            }
        },
        methods: {
            login() {
                login(this.username, this.password).then(() => {
                    this.$store.commit('login');
                    this.$router.replace('/users');
                });
            }
        }
    }
</script>

<style scoped>
    form {border: 2px solid #f1f1f1;}
    input[type=email], input[type=password] {
        width: 50%;
        padding: 12px 20px;
        margin: 8px 0;
        display: inline-block;
        border: 1px solid #ccc;
        box-sizing: border-box;
    }

    button {
        background-color: #46af82;
        color: white;
        padding: 14px 20px;
        margin: 8px 0;
        border: none;
        cursor: pointer;
        width: 30%;
    }

    button:hover {
        opacity: 0.8;
    }

    .cancelbtn {
        width: auto;
        padding: 10px 18px;
        background-color: #f44336;
    }


    .container {
        padding: 16px;
    }

    span.psw {
        float: right;
        padding-top: 16px;
    }

    /* Change styles for span and cancel button on extra small screens */
    @media screen and (max-width: 300px) {
        span.psw {
            display: block;
            float: none;
        }
        .cancelbtn {
            width: 100%;
        }
    }

</style>
