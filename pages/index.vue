<template>
  <div>
    <div>基础 index 页面</div>
    <div>您当前访问 ip 为（客户端渲染）：{{ ip }}</div>
    <div>您当前访问 ip 为（服务端渲染）：{{ ip2 }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ip: null,
    }
  },
  // asyncData({ $axios }) {
  //   return $axios.$get('https://icanhazip.com/').then((res) => {
  //     // 返回的对象会合并到组件的 data 中
  //     return { ip2: res }
  //   })
  // },
  async asyncData({ $axios }) {
    let res = await $axios.$get('https://icanhazip.com/')
    return { ip2: res }
  },
  created() {
    // this.$axios.get('https://icanhazip.com/').then((res) => {
    //   this.ip = res.data
    // })
    // this.$axios.$get('https://icanhazip.com/').then((res) => {
    this.$axios.$get('/').then((res) => {
      this.ip = res
    })
  },
}
</script>
