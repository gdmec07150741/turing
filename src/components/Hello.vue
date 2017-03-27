<template>
  
  <div class="hello">
    <p>
      <p>07150741</p>
  <p>石文莉</p>
      我是图灵机器人，请您提问：
      <input v-model="question">
    </p>
    <p>答案:{{ answer }}</p>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  data () {
    return {
      question: '',
      answer: '只有你提了问题，才会出现答案！'
    }
  },
  watch: {
    question: function (newQuestion) {
      this.answer = '等待你停止输入...'
      this.getAnswer()
    }
  },
  methods: {
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('?') === -1) {
          vm.answer = '问题动结尾通常都问号的哦。；－）'
          return
        }
        vm.answer = '努力思考中...'
        let baseUrl = 'api/robot/index?'
        let info = encodeURI(this.question)
        let userId = '13760795885'
        let key = 'e7168d4ec8a09bf383c9395a0ca2f42e'
        let targetUrl = baseUrl + 'info=' + info + '&userid=' + userId + '&key=' + key
        this.$http.get(targetUrl)
          .then((response) => {
            console.log(response)
            vm.answer = response.data.result.text
          })
          .catch(function (error) {
            vm.answer = '出错了，无法访问图灵机器人APT. ' + error
          })
      },
      500
      )
  }
}
</script>
