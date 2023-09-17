<template>
  <div>
    <div class="nav">
      <ul class="nav nav-pills">
        <li class="nav-item">
          <a class="nav-link" href="#/login">登录</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" :class="{'disabled':!isLogin}" href="#/booklist">书单</a>
        </li>
        <li class="nav-item nav-link">
          <b>状态 : </b>{{ isLogin?"已登录":"未登录" }}
        </li>
        <li class="logout">
          <button type="button" class="btn btn-outline-secondary" @click="logout">退出登录</button>
        </li>
      </ul>
    </div>
    <div>
      <!-- 路由出口，所匹配组件展示区 -->
      <router-view @loggedin="setLogin"></router-view>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      isLogin:false,
    }
  },
  methods: {
    setLogin(){
      this.isLogin=true;
    },
    logout(){
      // 过期的cookie即可删除
      document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT";
      this.isLogin=false;
      window.location.href = "http://localhost:8080/#/login";
    }
  },
  created() {
    // 检查Cookie
    const cookies = document.cookie.split(';');
    for (let i = 0; i < cookies.length; i++) {
      const cookie = cookies[i].trim();
      if (cookie.startsWith('username=')) {
        let username = cookie.substring('username='.length);
        this.setLogin();
        alert(`${username}，欢迎回来！`);
        break;
      }
    }
  }
}
</script>

<style>
</style>
<!-- 8.29--9.15 -->