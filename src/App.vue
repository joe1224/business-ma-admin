<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
  export default {
    name: 'App'
  }
</script>

<style>
  #app .sidebar-container .el-submenu .el-menu-item.is-active {
    background-color: #1890FF !important;
    font-weight: bold;
    border-right: 3px solid #FFC234;
  }
</style>
