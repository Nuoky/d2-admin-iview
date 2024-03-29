<template>
  <div class="d2-multiple-page-control-group" flex>
    <div class="d2-multiple-page-control-content" flex-box="1">
      <div class="d2-multiple-page-control-content-inner">
        <d2-contextmenu
          :visible.sync="contextmenuFlag"
          :x="contentmenuX"
          :y="contentmenuY">
          <d2-contextmenu-list
            :menulist="tagName === '/index' ? contextmenuListIndex : contextmenuList"
            @rowClick="contextmenuClick"/>
        </d2-contextmenu>
        <Tabs
          class="d2-multiple-page-control"
          :value="current"
          type="card"
          :closable="true"
          @on-click="handleClick"
          @on-tab-remove="handleTabsEdit"
          @contextmenu.native="handleContextmenu">
          <TabPane
            v-for="page in opened"
            :key="page.fullPath"
            :label="page.meta.title || '未命名'"
            :name="page.fullPath"/>
        </Tabs>
      </div>
    </div>
    <div
      class="d2-multiple-page-control-btn"
      flex-box="0">
      <Dropdown
        @on-click="name => handleControlItemClick(name)">
        <Button>
            <Icon type="ios-arrow-down"></Icon>
        </Button>
        <DropdownMenu slot="list">
          <DropdownItem name="left">
            <d2-icon name="arrow-left" class="d2-mr-10"/>
            关闭左侧
          </DropdownItem>
          <DropdownItem name="right">
            <d2-icon name="arrow-right" class="d2-mr-10"/>
            关闭右侧
          </DropdownItem>
          <DropdownItem name="other">
            <d2-icon name="times" class="d2-mr-10"/>
            关闭其它
          </DropdownItem>
          <DropdownItem name="all">
            <d2-icon name="times-circle" class="d2-mr-10"/>
            全部关闭
          </DropdownItem>
        </DropdownMenu>
      </Dropdown>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
export default {
  components: {
    D2Contextmenu: () => import('../contextmenu'),
    D2ContextmenuList: () => import('../contextmenu/components/contentmenuList')
  },
  data () {
    return {
      contextmenuFlag: false,
      contentmenuX: 0,
      contentmenuY: 0,
      contextmenuListIndex: [
        { icon: 'times-circle', title: '关闭全部', value: 'all' }
      ],
      contextmenuList: [
        { icon: 'arrow-left', title: '关闭左侧', value: 'left' },
        { icon: 'arrow-right', title: '关闭右侧', value: 'right' },
        { icon: 'times', title: '关闭其它', value: 'other' },
        { icon: 'times-circle', title: '关闭全部', value: 'all' }
      ],
      tagName: '/index'
    }
  },
  computed: {
    ...mapState('d2admin/page', [
      'opened',
      'current'
    ])
  },
  methods: {
    ...mapActions('d2admin/page', [
      'close',
      'closeLeft',
      'closeRight',
      'closeOther',
      'closeAll'
    ]),
    /**
     * @description 右键菜单功能点击
     */
    handleContextmenu (event) {
      let target = event.target
      // 解决 https://github.com/d2-projects/d2-admin/issues/54
      let flag = false
      if (target.className.indexOf('el-tabs__item') > -1) flag = true
      else if (target.parentNode.className.indexOf('el-tabs__item') > -1) {
        target = target.parentNode
        flag = true
      }
      if (flag) {
        event.preventDefault()
        event.stopPropagation()
        this.contentmenuX = event.clientX
        this.contentmenuY = event.clientY
        this.tagName = target.getAttribute('aria-controls').slice(5)
        this.contextmenuFlag = true
      }
    },
    /**
     * @description 右键菜单的row-click事件
     */
    contextmenuClick (command) {
      this.handleControlItemClick(command, this.tagName)
    },
    /**
     * @description 接收点击关闭控制上选项的事件
     */
    handleControlItemClick (command, tagName = null) {
      if (tagName) {
        this.contextmenuFlag = false
      }
      const params = {
        pageSelect: tagName
      }
      switch (command) {
        case 'left':
          this.closeLeft(params)
          break
        case 'right':
          this.closeRight(params)
          break
        case 'other':
          this.closeOther(params)
          break
        case 'all':
          this.closeAll()
          break
        default:
          this.$message.error('无效的操作')
          break
      }
    },
    /**
     * @description 接收点击 tab 标签的事件
     */
    handleClick (tab, event) {
      // 找到点击的页面在 tag 列表里是哪个
      const page = this.opened.find(page => page.fullPath === tab)
      const { name, params, query } = page
      if (page) {
        this.$router.push({ name, params, query })
      }
    },
    /**
     * @description 点击 tab 上的删除按钮触发这里 首页的删除按钮已经隐藏 因此这里不用判断是 index
     */
    handleTabsEdit (tagName, action) {
      if (action === 'remove') {
        this.close({
          tagName,
          vm: this
        })
      }
    }
  }
}
</script>
