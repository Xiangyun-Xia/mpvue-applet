<template>
  <div>
    <!-- 此处修改了iview modal组件：1.修改click事件为btnClick 2.增加openType传参 3.增加传递getuserinfo方法 -->
    <i-modal
      :visible="modalFlag"
      :actions="buttons"
      @btnClick="handleClick"
      @getuserinfo="getuserinfo"
    >{{tips}}</i-modal>
    <i-message id="message"/>
  </div>
</template>

<script>
import globalStore from '@/stores/global'
const { $Message } = require('static/iview/base/index')

export default {
  props: {
    // 登录提示语
    tips: { type: String, default: '是否确认登录' }
  },

  data () {
    return {
      // 自定义按钮
      buttons: [{ name: '返回首页' }, { name: '微信登录', color: '#19be6b', openType: 'getUserInfo' }]
    }
  },

  computed: {
    // 弹窗标记
    modalFlag () {
      return !globalStore.state.loginMsg.isLogin
    }
  },

  methods: {
    /**
     * 按钮点击事件
     */
    handleClick (e) {
      const index = e.mp.detail.index
      if (index === 0) globalStore.commit('currentChange', 'homepage')
    },
    /**
     * 微信原生登录事件
     */
    getuserinfo (e) {
      const userInfo = e.mp.detail.userInfo
      console.log(userInfo)
      if (userInfo) {
        globalStore.commit('loginChange', userInfo)
        $Message({ content: '欢迎你，' + userInfo.nickName + '！', type: 'success' })
      }
    }
  }
}
</script>
