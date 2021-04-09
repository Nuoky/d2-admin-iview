<template>
  <Dropdown class="d2-mr">
    <span class="btn-text">{{info.name ? `你好 ${info.name}` : '未登录'}}</span>
    <DropdownMenu slot="list">
      <DropdownItem @click.native="logOff">
        <d2-icon name="power-off" class="d2-mr-5"/>
        注销
      </DropdownItem>
    </DropdownMenu>
  </Dropdown>
</template>

<script>
import { mapState, mapActions } from 'vuex'
export default {
  computed: {
    ...mapState('d2admin/user', [
      'info'
    ])
  },
  methods: {
    ...mapActions('d2admin/account', [
      'logout'
    ]),
    /**
     * @description 登出
     */
    logOff () {
      this.logout({
        vm: this,
        confirm: true
      })
    }
  }
}
</script>
