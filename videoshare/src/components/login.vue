<template>
    <div class="body_div">
        <div class="flex">
            <h2>Login</h2>
            <h4>PLEASE ACTIVATE COOKIES</h4>
            <form v-on:submit.prevent="login">
                <input type="text" placeholder="Username..." v-model="username" required/>
                <input type="password" placeholder="Password..." v-model="password" required/>
                <button type="submit" value="submit">Submit</button>
            </form>
            <p v-if="error" class="error">{{message}}</p>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
import Vue from "vue";
import store from '../store.js';

export default {
    name: "login",
    data() {
        return {
            username: "",
            password: "",
            error: false,
            message: null
        }
    },

    
    methods: {
        //Login to express api
        login: function login() {
            const { username, password } = this;
            console.log("Here")
            axios
            .post("http://localhost:3000/login", {
                username,
                password
            })
            .then((Response) => {
                //console.log(Response);
                //console.log("no error");
                console.log(Response.data)
                if(Response.data == "Something went Wrong, please try again."){
                    this.error = true
                    this.message = "Something went wrong, please try again"
                }else{
                this.error = false
                Vue.$cookies.set(
                    username,
                    "97410df8-c866-11eb-b8bc-0242ac130003",
                    "1d"
                )
                let uid = Response.data[0].user_id
                let uname = Response.data[0].username

                store.commit('login', { username: uname, uid: uid })
                //  console.log(store.state)
                }
            })
            .catch((error) => {
                console.log(error)
            })
        }   
    }
}
</script>

<style>
    .flex > form{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        gap: 25px;
    }
    .body_div{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100vw;
        height: 30vh;
    }
    .flex > form> button{
        background-color: black;
        color: rgb(167, 167, 167);
        border: none;
        border-radius: 4px;
        transition: 0.7s;
        width: 200px;
        height: 35px;
    }
    .flex > h2 {
        font-size: 60px;
    }
    .flex > form >input{
        background-color: rgb(31, 31, 31);
        border-radius: 5px;
        border: none;   
        margin-left: 100px;
        margin-right: 100px;
        width: 500px;
        height: 40px;
        color: rgb(167, 167, 167);
    }
    .flex > form> input:focus{
        color: rgb(167, 167, 167);
        outline: none !important;
        border:1px solid rgb(46, 129, 43);
        box-shadow: 0 0 10px #719ECE;
    }
    .flex > form > button:hover{
        color: rgb(221, 221, 221);
        background-color: rgb(46, 129, 43);
    }
    .error{
        color: rgb(150, 47, 47);
    }
</style>
