<template>
  <div id="app">
    <el-container>
      <el-header>
        <el-row>
          <el-col :span="4">
            <router-link :to="{ name: 'home'}">
              <div class="logo">
                <img src="@/assets/images/krill_logo_white.svg">
              </div>
            </router-link>
          </el-col>
          <el-col :span="20">
            <div class="toolbar">
              <el-select v-model="$i18n.locale" placeholder="Language" size="small">
                <el-option v-for="lang in langs" :key="lang.iso" :value="lang.iso" :label="lang.label"></el-option>
              </el-select>
              <font-awesome-icon icon="sign-out-alt" v-if="user" class="logout" @click="logout"/>
            </div>
          </el-col>
        </el-row>
      </el-header>

      <el-main>
        <router-view v-on:authEvent="loadUser"/>
      </el-main>
    </el-container>
  </div>
</template>



<style>
html,
body {
  padding: 0;
  margin: 0;
  font-family: "Lato", sans-serif;
  background-color: #fff;
}
.el-header {
  background: linear-gradient(45deg, #f63107, #f63107);
  line-height: 60px;
  color: #ffffff;
  z-index: 3;
}
.logo img {
  width: 146px;
  margin-left: -14px;
}
.logout {
  margin-left: 2rem;
  cursor: pointer;
}

.toolbar {
  text-align: right;
}
</style>

<script>
import router from "@/router";
import APIService from "@/services/APIService.js";

export default {
  data() {
    return {
      user: null,
      langs: [
        {iso: "it", label: "Italiano"},
        {iso: "en", label: "English"}
      ]
    };
  },
  created() {
    this.loadUser();
  },
  methods: {
    loadUser() {
      this.user = JSON.parse(localStorage.getItem("user"));
    },
    logout() {
      return APIService.logout().then(() => {
        this.user = null;
        router.push("/login");
      });
    }
  }
};
</script>