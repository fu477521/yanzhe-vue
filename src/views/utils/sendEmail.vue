<template lang="html">
<!--此页面需要-->
  <div class="container">
    <div>
        <movie-index-header ></movie-index-header>   <!--  展示引入的header组件 -->
    </div>
    <div class="userMessage">
      <user-message></user-message>
    </div>
    <!--用户的相关信息-->
    <label>收件箱</label>
    <div>
      <email-list v-for="item in receive_items" :key="item.id" :title="item.title" :fromUser="item.fromUser" :context="item.context"></email-list>
    </div>
    <label>发件箱</label>
    <div>
      <email-list v-for="item in send_items" :key="item.id" :title="item.title" :fromUser="item.fromUser" :context="item.context"></email-list>
    </div>

    <send-talk-box></send-talk-box>
    <common-footer></common-footer>  <!--  展示引入的footer组件 -->
  </div>
</template>
<script>
import MovieIndexHeader from '../../components/MovieIndexHeader'
import CommonFooter from '../../components/commonFooter'
import UserMessage from '../../components/UserMessage'
import EmailList from '../../components/EmailList.vue'
import SendTalkBox from '../../components/SendTalkBox.vue'
export default {
  name: 'HelloWorld',
  data () {
    return {
      receive_items: [],
      send_items: [],
      detail: []
    }
  },
  components: {
    MovieIndexHeader,
    CommonFooter,
    UserMessage,
    EmailList,
    SendTalkBox
  },

  //  这里用于获取数据，需要获得主页推荐，主页新闻列表，主页电影列表
  created () {
    let userId = localStorage._id
    let sendData = {
      token: localStorage.token,
      user_id: localStorage._id,
      receive: 0
    }
    let receiveData = {
      token: localStorage.token,
      user_id: localStorage._id,
      receive: 1
    }

    if (userId) {
      this.$http.post('http://localhost:3000/users/showEmail', sendData).then((data) => {
        if (data.body.status === 1) {
          alert(data.body.message)
        } else {
          this.send_items = data.body.data
        }
        console.log(data.body.data)
      })
      this.$http.post('http://localhost:3000/users/showEmail', receiveData).then((data) => {
        if (data.body.status === 1) {
          alert(data.body.message)
        } else {
          this.receive_items = data.body.data
        }
        console.log(data.body.data)
      })
    } else {
      alert('用户信息错误')
    }
  },
  methods: {

  }
}
</script>

<style lang="css" scoped>
.box{
  display: inline-flex;
}
.container {
  width: 100%;
  margin: 0 auto;
}
.userMessage{
  padding-top:60px;
  margin-top:-10px;
  margin-left: -10px;
}
</style>
