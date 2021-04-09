<template>
  <div>
    <Tooltip
      theme="dark"
      content="主题"
      placement="bottom">
      <Button
        class="d2-ml-0 d2-mr btn-text can-hover"
        type="text"
        @click="dialogVisible = true">
        <d2-icon
          name="diamond"
          style="font-size: 16px"/>
      </Button>
    </Tooltip>
    <Modal
      title="主题"
      width="600px"
      v-model="dialogVisible"
      :transfer="true">
      <d2-theme-list style="margin-top: -25px;"/>
    </Modal>
  </div>
</template>

<script>
import themeList from './components/d2-theme-list'
export default {
  components: {
    'd2-theme-list': themeList
  },
  data () {
    return {
      dialogVisible: false
    }
  }
}
</script>
