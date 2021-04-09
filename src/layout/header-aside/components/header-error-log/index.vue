<template>
  <div>
    <Tooltip
      theme="dark"
      :content="tooltipContent"
      placement="bottom">
      <Button
        class="d2-ml-0 d2-mr btn-text can-hover"
        type="text"
        @click="handleClick">
        <Badge
          v-if="logLength > 0"
          :overflow-count="99"
          :count="logLengthError"
          :dot="logLengthError === 0">
          <d2-icon
            :name="logLengthError === 0 ? 'dot-circle-o' : 'bug'"
            style="font-size: 20px"/>
        </Badge>
        <d2-icon
          v-else
          name="dot-circle-o"
          style="font-size: 20px"/>
      </Button>
    </Tooltip>
    <Modal
      :title="tooltipContent"
      :fullscreen="true"
      v-model="dialogVisible"
      :transfer="true">
      <div class="d2-mb-10">
        <Button type="danger" size="small" @click="handleLogClean">
          <d2-icon name="trash-o"/>
          清空
        </Button>
      </div>
      <d2-error-log-list/>
    </Modal>
  </div>
</template>

<script>
import { mapGetters, mapMutations } from 'vuex'
import D2ErrorLogList from './components/list'
export default {
  components: {
    D2ErrorLogList
  },
  data () {
    return {
      dialogVisible: false
    }
  },
  computed: {
    ...mapGetters('d2admin', {
      logLength: 'log/length',
      logLengthError: 'log/lengthError'
    }),
    tooltipContent () {
      return this.logLength === 0
        ? '没有日志或异常'
        : `${this.logLength} 条日志${this.logLengthError > 0
          ? ` | 包含 ${this.logLengthError} 个异常`
          : ''}`
    }
  },
  methods: {
    ...mapMutations('d2admin/log', [
      'clean'
    ]),
    handleClick () {
      if (this.logLength > 0) {
        this.dialogVisible = true
      }
    },
    handleLogClean () {
      this.dialogVisible = false
      this.clean()
    }
  }
}
</script>
