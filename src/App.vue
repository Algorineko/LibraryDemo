<template>
  <div class="App row">
      <div class="col-md-10 offset-md-1" style="background-color: rgba(233, 230, 227, 0.812);">
        <h1 class="text-center display-1">图书管理</h1>
        <div class="setBtn">
          <button class="btn btn-light" data-bs-toggle="modal" data-bs-target="#addBookModal">添加图书</button>
          <button class="btn btn-dark" data-bs-toggle="modal" data-bs-target="#deleteBookModal">删除图书</button>
        </div>
        <br><br>
        <div>
          <table class="table text-center table-striped table-bordered container table-hover">
            <thead>
              <tr>
              <th>ID</th>
              <th>书名</th>
              <th>作者</th>
              <th>出版社</th>
              <th>功能</th>
            </tr>
            </thead>
            <tbody>
              <template v-if="showBookList">
                <tr v-for="(item, index) in bookList" :key="index">
                  <td>{{ item.id }}</td>
                  <td>{{ item.bookname }}</td>
                  <td>{{ item.author }}</td>
                  <td>{{ item.publisher }}</td>
                  <td><button @click="getBook(item.id)" data-bs-toggle="modal" data-bs-target="#changeInfoModal" class="btn btn-secondary">设置</button></td>
                </tr>
            </template>
            <template v-else>
              <tr>
                <th colspan="5">无图书信息</th>
              </tr>
            </template>
            </tbody>
          </table>
        </div>
      </div>
    <!-- addBookModal -->
    <div class="modal" id="addBookModal" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">添加图书</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <form>
              <div id="search" class="mb-3">
                  书名：
                  <input type="text" v-model="newBook.bookname" class="form-control text-center form-control-md">
                  作者：
                  <input type="text" v-model="newBook.author" class="form-control text-center form-control-md">
                  出版社：
                  <input type="text" v-model="newBook.publisher" class="form-control text-center form-control-md">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" id="closeModalA">关闭</button>
            <button type="button" class="btn btn-primary" @click="putOnBook">确定</button>
          </div>
        </div>
      </div>
    </div>
    <!-- deleteBookModal -->
    <div class="modal" id="deleteBookModal" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">删除图书</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <form>
              <div id="search" class="mb-3">
                输入删除图书ID:
                <input type="text" v-model="idToDelete" class="form-control text-center form-control-md">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" id="closeModalB">关闭</button>
            <button type="button" class="btn btn-primary" @click="cutOutBook">确定</button>
          </div>
        </div>
      </div>
    </div>
    <!-- changeInformation -->
    <div class="modal" id="changeInfoModal" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">修改信息</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <form>
              <div id="search" class="mb-3">
                  书名：
                  <input type="text" v-model="selectedBookname" class="form-control text-center form-control-md">
                  作者：
                  <input type="text" v-model="selectedAuthor" class="form-control text-center form-control-md">
                  出版社：
                  <input type="text" v-model="selectedPublisher" class="form-control text-center form-control-md">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" id="closeModalC">关闭</button>
            <button type="button" class="btn btn-primary" @click="saveChanges">确定</button>
          </div>
        </div>
      </div>
    </div>
  </div> 
</template>

<script>
import axios from 'axios';
export default {
  components:{

  },
  data(){
    return{
      bookList:[],
      newBook:{
        id: null,
        bookname:"",
        author:"",
        publisher:"",
      },
      idToDelete:null,
      idOfselectedBook: null,
      selectedBookname:null,
      selectedAuthor:null,
      selectedPublisher:null,
    }
  },
  computed: {
    showBookList() {
      return this.bookList.length > 0;
    },
  },
  methods:{ 
    putOnBook(){
      if(this.newBook.bookname === "" || this.newBook.author === "" || this.newBook.publisher === ""){
        alert("请填写完整信息");
        return;
      }
      this.newBook.id = new Date().getTime().toString().slice(-6);
      this.bookList.push(this.newBook);
      this.newBook = {
          id: null,
          bookname: "",
          author: "",
          publisher: "",
        };
      document.getElementById('closeModalA').click();
    },
    cutOutBook() {
      if(this.idToDelete == null){
        alert("请填写完整信息");
        return;
      }
      this.bookList = this.bookList.filter( book => book.id != this.idToDelete);
      this.idToDelete = null;
      document.getElementById('closeModalB').click();
    },
    getBook(id) {
      this.idOfselectedBook = id;
    },
    saveChanges(){
      const index = this.bookList.findIndex(item => item.id == this.idOfselectedBook);
      if (index != -1) {
        this.bookList[index].bookname = this.selectedBookname;
        this.bookList[index].author = this.selectedAuthor;
        this.bookList[index].publisher = this.selectedPublisher;
      }
      this.selectedBookname=null;
      this.selectedAuthor=null;
      this.selectedPublisher=null;
      this.idOfselectedBook=null;
      document.getElementById('closeModalC').click();
    }
  },
  mounted(){
    axios({
      url:'http://hmajax.itheima.net/api/books',
      params: {
        creator:"AirLingPing",
      }
    }).then(result=>{
      this.bookList=result.data.data;
    })
  }
}
</script>

<style>
.App {
  margin: 0 auto;
  padding: 20px;
}
.setBtn{
  float: right;
}
</style>
<!-- 8.14--8.16 -->