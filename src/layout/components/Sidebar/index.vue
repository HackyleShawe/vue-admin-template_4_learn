<!--
  左侧导航栏
-->
<template>
  <div :class="{'has-logo':showLogo}">

    <!--是否显示LOGO-->
    <logo v-if="showLogo" :collapse="isCollapse" />

    <!--
      el-scrollbar：滚动条组件
      element官网文章中没有介绍滚动条的使用方式，但是官方滚动条源码中有滚动条的样式代码
    -->
    <el-scrollbar wrap-class="scrollbar-wrapper">
      <!--
        菜单 https://element.eleme.io/#/zh-CN/component/menu
        ":background-color"的赋值不能直接给赋常量值，而应该先放在data中
      -->
      <el-menu
        :default-active="activeMenu"
        :collapse="isCollapse"
        :background-color="variables.menuBg"
        :text-color="variables.menuText"
        :unique-opened="false"
        :active-text-color="variables.menuActiveText"
        :collapse-transition="false"
        mode="vertical"
      >
        <!--菜单项、滚动条组件项-->
        <sidebar-item v-for="route in routes" :key="route.path" :item="route" :base-path="route.path" />
      </el-menu>
    </el-scrollbar>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Logo from './Logo'
import SidebarItem from './SidebarItem'
import variables from '@/styles/variables.scss'

export default {
  components: { SidebarItem, Logo },
  computed: {
    ...mapGetters([
      'sidebar'
    ]),
    routes() {
      return this.$router.options.routes
    },
    activeMenu() {
      const route = this.$route
      const { meta, path } = route
      // if set path, the sidebar will highlight the path you set
      if (meta.activeMenu) {
        return meta.activeMenu
      }
      return path
    },
    showLogo() {
      return this.$store.state.settings.sidebarLogo
    },
    variables() {
      return variables
    },
    isCollapse() {
      return !this.sidebar.opened
    }
  }
}
</script>
