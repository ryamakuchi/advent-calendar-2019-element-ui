<template>
  <div>
    <el-menu default-active="1" mode="horizontal">
      <el-menu-item index="1">
        <logo />

        <el-link
          type="primary"
          :underline="false"
          href="https://qiita.com/advent-calendar/2019/nuxt-js"
          target="_blank"
          rel="noopener noreferrer"
          class="brand"
        >
          Nuxt.js Advent Calendar 2019 for Qiita
        </el-link>
      </el-menu-item>

      <el-menu-item index="2" class="float-right">
        <el-link
          href="https://element.eleme.io/"
          target="_blank"
          rel="noopener noreferrer"
        >
          Made with Element
        </el-link>
      </el-menu-item>
    </el-menu>

    <nuxt />
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  }
}
</script>

<style>
html {
  color: #5e6d82;
}

.el-menu .brand {
  font-size: 18px;
  padding-left: 10px;
  color: #409EFF !important;
}
.el-menu .float-right {
  float: right;
}
</style>
