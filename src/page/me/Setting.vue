<template>
  <div>
    <div class="wrap">
      <Nav/>

      <div class="container row">
        <div class="col-lg-3">
          <setting-nav/>
        </div>

        <div class="col-lg-9 content">
            <div class="header">
              <div class="title">更改用户名</div>
            </div>
            <div class="cute-form">
              <div v-if="!show.username">
                <span class="key">用户名：</span>
                <span class="val">{{current.username || '-'}}</span>
                <span @click="show.username=true" class="edit btn-small round">更改</span>
              </div>
              <form v-else @submit.prevent="submit('username')">
                <div class="input-control">
                  <label for="username">请输入新的用户名</label>
                  <input 
                    id="username"
                    v-model="current.username"
                    class="round"
                    type="text">
                </div>
                <div class="btn-group">
                  <button type="submit" class="btn-primary left-round">提交</button>
                  <button @click="show.username=false" type="button" class="btn right-round">取消</button>
                </div>
              </form>
            </div>
        </div>          
      </div>
    </div>

    <Footer/>
  </div>
</template>

<script>
import "../../css/cuteForm.css";

import session from "../../lib/session.js";
import api from "../../lib/api.js";

import Nav from "../../components/Nav";
import Footer from "../../components/Footer";
import SettingNav from "../../components/SettingNav";

export default {
  components: { Nav, Footer, SettingNav },

  data() {
    return {
      error: {
        invalid_old_password: false
      },
      show: {
        username: false
      },
      password: {
        old: "",
        new: ""
      },
      current: session.uinfo()
    };
  },

  methods: {
    submit(property) {
      api("user/update", this.current).then(r => {
        session.replace_uinfo(r.data);
        this.show[property] = false;
      });
    },

    reset() {
      let form = document.querySelector("form");
      form.reset();
    },

    change_password() {
      let u = session.uinfo();
      let unique = u.username || u.phone || u.email;

      session.exist(unique, this.password.old).then(r => {
        r ? this.update_password() : this.error.invalid_old_password;
      });
    },

    update_password() {
      return api("user/update", {
        id: this.current.id,
        password: this.password.new
      }).then(() => {
        alert("密码修改成功");
        session.logout("#/login");
      });
    }
  }
};
</script>

<style scoped>
</style>