<template>
  <div class="row">
    <div class="col-md-6 offset-md-3" style="background-color: rgba(233, 230, 227, 0.812);">
      <br><br>
      <h1 class="text-center">登录</h1>
      <br>
      <div class="col-md-6 offset-md-3">
        账号：<input type="text" v-model="curAmount.username">
        <br><br>
        密码：<input type="password" v-model="curAmount.password">
        <br><br>
        <div class="col-md-6 offset-md-3">
          <button type="button" class="btn btn-outline-secondary" @click="handleRegister">注册</button>
          &nbsp;&nbsp;
          <button type="button" class="btn btn-outline-secondary" @click="handleLogin">登录</button>
        </div>
      </div>
      <br><br>
    </div>
  </div>
</template>

<script>
export default {
  name: "LoginLibrary",
  data(){
    return{
      amountList:JSON.parse(localStorage.getItem("amountList"))||[
        { username:"default",password:"default"},
      ],
      curAmount:{
        username:"",
        password:"",
      }
    }
  },
  methods: {
    handleRegister(){
      this.amountList.push({
        username:this.curAmount.username,
        password:this.curAmount.password
      });
      alert("注册成功");
      this.curAmount.username="";
      this.curAmount.password="";
    },
    handleLogin(){
      for(let amount of this.amountList){
        if(this.curAmount.username === amount.username && this.curAmount.password === amount.password){
          this.$emit("loggedin");
          //cookie设置
          let d=new Date();
          d.setTime(d.getTime()+1*24*60*60*1000);
          document.cookie=`username=${this.curAmount.username};expires=${d.toGMTString()}`
          window.location.href = "http://localhost:8080/#/booklist";
          alert("登录成功");
          this.curAmount.username = "";
          this.curAmount.password = "";
          return;
        }
      }
      alert("账号或密码错误");
    }
  },
  watch:{
    amountList:{
      deep:true,
      handler(newValue){
        localStorage.setItem('amountList',JSON.stringify(newValue));
      }
    }
  },
}
</script>

<style>

</style>