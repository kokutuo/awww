<template>
  <div>
    <div class="wrap">
      <Nav/>

      <div class="container row">
        <div class="col-lg-3">
          <AdminNav/>
        </div>

        <div class="col-lg-9 content">
          <div class="header row">
            <div class="col-lg-8 left">
              <div class="title">用户管理</div>
            </div>
            <div class="col-lg-4 right">
              <button @click="edit_pattern=true" class="btn-primary round">添加</button>
            </div>
          </div>

          <div v-if="!edit_pattern" class="tool-bar">
            <form @submit.prevent="search" class="search">
              <label for="search">快速查找</label>
              <input id="search" type="search" v-model="keyword">
              <button class="btn-primary">搜</button>
            </form>
          </div>

          <form v-if="edit_pattern" @submit.prevent="cou">
            <div class="input-control">
              <label>用户名</label>
              <input 
                v-model="current.username"
                class="round"
                type="text">
            </div>
            <div class="input-control">
              <label>密码</label>
              <input 
                v-model="current.password"
                class="round"
                type="text">
            </div>
            <div class="input-control">
              <label>手机</label>
              <input 
                v-model="current.phone"
                class="round"
                type="text">
            </div>
            <div class="input-control">
              <label>邮箱</label>
              <input 
                v-model="current.email"
                class="round"
                type="text">
            </div>
            <div class="input-control">
              <label>地址</label>
              <input 
                v-model="current.location"
                class="round"
                type="text">
            </div>
            <div class="input-control">
              <button type="submit" class="btn-primary left-round">提交</button>
              <button @click="cancel" type="button" class="right-round">取消</button>
            </div>
          </form>

          <div v-else class="table">
            <table>
              <thead>
                <th>用户名</th>
                <th>密码</th>
                <th>手机</th>
                <th>邮箱</th>
                <th>操作</th>
              </thead>
              <tbody>
                <tr v-for="row in list" :key="row.id">
                  <td>{{row.username || '-'}}</td>
                  <td>{{row.password}}</td>
                  <td>{{row.phone || '-'}}</td>
                  <td>{{row.email || '-'}}</td>
                  <td>
                    <button @click="set_current(row)" class="btn-primary left-round">编辑</button>
                    <button @click="remove(row.id)" class="right-round">删除</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <Pagination v-if="!edit_pattern" :totalCount="total" :pageLimit="page_limit" :btnLimit="btn_limit" :on-page-change="on_page_change"/>
        </div>
      </div>
    </div>
    
    <Footer/>
  </div>
</template>

<script>
import AdminPage from "../../mixins/AdminPage";

import api from "../../lib/api";

export default {
  mixins: [AdminPage],

  data() {
    return {
      model: "user",
      searchable: ["username", "phone", "email", "location"]
    };
  }
};
</script>

<style scoped>
</style>