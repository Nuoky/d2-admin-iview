<template>
  <div>
    <img
      src="./image/d2-help-button@2x.png"
      style="width: 138px;"
      @click="dialogVisible = true">
    <Modal
      title="帮助"
      width="600px"
      v-model="dialogVisible"
      :transfer="true">
      <div style="margin-top: -25px; margin-bottom: -25px;">
        <ButtonGroup class="d2-mb">
          <Button @click="$open('https://github.com/d2-projects/d2-admin')">
            <d2-icon name="github" class="d2-mr-5"/>
            主页
          </Button>
          <Button @click="$open('https://doc.d2admin.fairyever.com/zh/')">
            <d2-icon name="book" class="d2-mr-5"/>
            中文文档
          </Button>
          <Button @click="$open('https://github.com/d2-projects/d2-admin/issues')">
            <d2-icon name="question" class="d2-mr-5"/>
            issues
          </Button>
          <Button @click="$open('https://github.com/d2-projects/d2-admin/issues/new/choose')">
            <d2-icon name="plus" class="d2-mr-5"/>
            New issue
          </Button>
        </ButtonGroup>
        <Row :gutter="20">
          <Col :span="12">
            <Alert :closable="false" type="info" title="2000人QQ群 [ 推荐 ]" class="d2-mb"/>
            <img src="./image/qq.jpg" style="width: 100%;">
          </Col>
          <Col :span="12">
            <Alert :closable="false" type="info" title="与作者成为微信好友 邀请进微信群" class="d2-mb"/>
            <img src="./image/we.jpg" style="width: 100%;">
          </Col>
        </Row>
      </div>
    </Modal>
  </div>
</template>

<script>
export default {
  name: 'd2-page-cover',
  data () {
    return {
      dialogVisible: false
    }
  }
}
</script>
