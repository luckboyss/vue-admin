<template>
  <div>
    <logo v-if="showLogo" :collapse="isCollapse" />
    <el-scrollbar wrap-class="scrollbar-wrapper">
      <el-menu
        mode="vertical"
        :default-active="activeMenu"
        class="el-menu-vertical-demo"
        :collapse="isCollapse"
        :background-color="variables.menuBg"
        :text-color="variables.menuText"
        :active-text-color="variables.menuActiveText"
        :collapse-transition="false"
        :unique-opened="false"
      >
        <sidebar-item v-for="route in permission_routes" :key="route.path" :item="route" :base-path="route.path" />
        <!-- <div class="menu-wrapper">
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-location svg-icon"></i>
              <span slot="title">导航一</span>
            </template>
            <div class="menu-wrapper nest-menu">
              <el-menu-item index="1-1">选项1</el-menu-item>
            </div>
            <div class="menu-wrapper nest-menu">
              <el-menu-item index="1-2">选项2</el-menu-item>
            </div>
            <div class="menu-wrapper nest-menu">
              <el-menu-item index="1-3">选项3</el-menu-item>
            </div>
            <div class="menu-wrapper nest-menu">
              <el-submenu index="1-4" popper-append-to-body>
                <span slot="title">选项4</span>
                <div class="menu-wrapper nest-menu">
                  <el-menu-item index="1-4-1">选项1</el-menu-item>
                </div>
                <div class="menu-wrapper nest-menu">
                  <el-menu-item index="1-4-2">选项2</el-menu-item>
                </div>
              </el-submenu>
            </div>
          </el-submenu>
        </div> -->
        <!-- <div class="menu-wrapper">
          <el-menu-item index="2" class="submenu-title-noDropdown">
            <i class="el-icon-menu svg-icon"></i>
            <span slot="title">导航二</span>
          </el-menu-item>
        </div>
        <div class="menu-wrapper">
          <el-menu-item index="3" class="submenu-title-noDropdown">
            <i class="el-icon-document svg-icon"></i>
            <span slot="title">导航三</span>
          </el-menu-item>
        </div>
        <div class="menu-wrapper">
          <el-menu-item index="4" class="submenu-title-noDropdown">
            <i class="el-icon-setting svg-icon"></i>
            <span slot="title">导航四</span>
          </el-menu-item>
        </div> -->
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
  components: {
    SidebarItem,
    Logo
  },
  computed: {
    ...mapGetters(['permission_routes', 'sidebar']),
    variables() {
      return variables
    },
    showLogo() {
      return this.$store.state.settings.sidebarLogo
    },
    isCollapse() {
      return !this.sidebar.opened
    },
    activeMenu() {
      const route = this.$route
      const { meta, path } = route
      // if set path, the sidebar will highlight the path you set
      if (meta.activeMenu) {
        return meta.activeMenu
      }
      return path
    }
  }
}
</script>
