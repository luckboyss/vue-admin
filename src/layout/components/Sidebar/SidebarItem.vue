<template>
  <div v-if="!item.hidden" class="menu-wrapper">
    <template
      v-if="
        hasOneShowingChild(item.children, item) &&
          (!onlyOneChild.children || onlyOneChild.noShowingChildren) &&
          !item.alwaysShow
      "
    >
      <app-link v-if="onlyOneChild.meta" :to="resolvePath(onlyOneChild.path)">
        <el-menu-item :index="resolvePath(onlyOneChild.path)" :class="{ 'submenu-title-noDropdown': !isNest }">
          <item :icon="onlyOneChild.meta.icon || (item.meta && item.meta.icon)" :title="onlyOneChild.meta.title" />
        </el-menu-item>
      </app-link>
    </template>

    <el-submenu v-else ref="subMenu" :index="resolvePath(item.path)" popper-append-to-body>
      <template slot="title">
        <item v-if="item.meta" :icon="item.meta && item.meta.icon" :title="item.meta.title" />
      </template>
      <sidebar-item
        v-for="child in item.children"
        :key="child.path"
        :item="child"
        :base-path="resolvePath(child.path)"
        :is-nest="true"
        class="nest-menu"
      />
    </el-submenu>
  </div>
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
  </div>
  <div class="menu-wrapper">
    <el-menu-item index="2" class="submenu-title-noDropdown">
      <i class="el-icon-menu svg-icon"></i>
      <span slot="title">导航二</span>
    </el-menu-item>
  </div> -->
</template>

<script>
import path from 'path'
import { isExternal } from '@/utils/validate'
import Item from './Item'
import AppLink from './Link'

export default {
  name: 'SidebarItem',
  components: {
    Item,
    AppLink
  },
  props: {
    // route objct
    item: {
      type: Object,
      required: true
    },
    isNest: {
      type: Boolean,
      default: false
    },
    basePath: {
      type: String,
      default: ''
    }
  },
  data() {
    // To fix https://github.com/PanJiaChen/vue-admin-template/issues/237
    // TODO: refactor with render function
    this.onlyOneChild = null
    return {}
  },
  methods: {
    hasOneShowingChild(children = [], parent) {
      const showingChildren = children.filter((item) => {
        if (item.hidden) {
          return false
        } else {
          // Temp set(will be used if only has one showing child)
          this.onlyOneChild = item
          return true
        }
      })

      // When there is only one child router, the child router is displayed by default
      if (showingChildren.length === 1) {
        return true
      }

      // Show parent if there are no child router to display
      if (showingChildren.length === 0) {
        this.onlyOneChild = { ...parent, path: '', noShowingChildren: true }
        return true
      }

      return false
    },
    resolvePath(routePath) {
      if (isExternal(routePath)) {
        return routePath
      }
      if (isExternal(this.basePath)) {
        return this.basePath
      }
      return path.resolve(this.basePath, routePath)
    }
  }
}
</script>
