<template>
  <div class="dashboard-container">
    <div class="dashboard-text">{{msg}}</div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'Dashboard',
  data(){
    return {
      msg:''
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  created() {
    this.$http({
      url: this.$http.adornUrl('/list'),
      method: 'get',
    }).then(res=>{
      this.msg=res;
    })
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
