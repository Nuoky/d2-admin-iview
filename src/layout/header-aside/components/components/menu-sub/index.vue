<template>
  <Submenu :name="menu.path || uniqueId">
    <template slot="title">
      <i v-if="menu.icon" :class="`fa fa-${menu.icon}`"></i>
      <i v-if="menu.icon === undefined & !menu.iconSvg" class="fa fa-folder-o"></i>
      <d2-icon-svg v-if="menu.iconSvg" :name="menu.iconSvg"/>
      {{menu.title}}
    </template>
    <template v-for="(child, childIndex) in menu.children">
      <d2-layout-header-aside-menu-item v-if="child.children === undefined" :menu="child" :key="childIndex"/>
      <d2-layout-header-aside-menu-sub v-else :menu="child" :key="childIndex"/>
    </template>
  </Submenu>
</template>

<script>
import { uniqueId } from 'lodash'
// 组件
import d2LayoutMainMenuItem from '../menu-item'

export default {
  name: 'd2-layout-header-aside-menu-sub',
  components: {
    'd2-layout-header-aside-menu-item': d2LayoutMainMenuItem
  },
  props: {
    menu: {
      type: Object,
      required: false,
      default: () => {}
    }
  },
  data () {
    return {
      uniqueId: uniqueId('d2-menu-empty-')
    }
  }
}
</script>
