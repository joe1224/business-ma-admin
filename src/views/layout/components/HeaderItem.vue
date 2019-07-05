<template>
  <el-header height="70px">
    <img
      src="../../../assets/enLogo.svg"
      alt="element-logo"
      class="header-logo">
      <el-dropdown class="avatar-container" trigger="click">
        <div class="avatar-wrapper">
          <img class="user-avatar" :src="avatar">
          <i class="el-icon-caret-bottom"></i>
        </div>
        <el-dropdown-menu class="user-dropdown" slot="dropdown">
          <router-link class="inlineBlock" to="/">
            <el-dropdown-item>
              首页
            </el-dropdown-item>
          </router-link>
          <el-dropdown-item divided>
            <span @click="logout" style="display:block;">退出</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
  </el-header>
</template>

<script type="text/ecmascript-6">
  import {mapGetters} from 'vuex'

  export default {
    name: 'HeaderItem',
    computed: {
      ...mapGetters([
        'avatar'
      ])
    },
    methods: {
      logout() {
        this.$store.dispatch('LogOut').then(() => {
          location.reload() // 为了重新实例化vue-router对象 避免bug
        })
      }
    }
  };
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .header-logo {
    display: inline-block;
    vertical-align: middle;
    margin-top: 5px;
  }

  .el-header {
    position: fixed;
    width: 100%;
    z-index: 2;
    background-color: #ffffff;
    border-bottom: 1px solid #c5c5c5;
  }

  .avatar-container {
    height: 50px;
    display: inline-block;
    position: absolute;
    right: 35px;
    .avatar-wrapper {
      cursor: pointer;
      margin-top: 15px;
      position: relative;
      .user-avatar {
        width: 40px;
        height: 40px;
        border-radius: 10px;
      }
      .el-icon-caret-bottom {
        position: absolute;
        right: -20px;
        top: 25px;
        font-size: 12px;
      }
    }
  }
</style>
