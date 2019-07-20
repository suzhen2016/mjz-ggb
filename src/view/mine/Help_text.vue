<template>
  <div class = 'pre' >
    <!-- 顶部 -->
    <nav-header title='钱包' back='true'></nav-header> 
    <!-- 内容 -->
    <div class="content new_add"  style="height:calc(100% - 44px)">
		<div class="txt">
			会带坏哈苏打好好撒大护法撒胡发到发撒发生 萨达刚发的发撒旦法好是否 发撒旦飞洒大福啥大家发撒发萨芬萨达发电房
		</div>
    </div>    
  </div>
</template>

<script>
import { Actionsheet, Group, XSwitch, Toast } from 'vux'

export default {
  components: {
    Group,
    XSwitch,
	Toast,
	Actionsheet,
    navHeader:()=>import('@/components/navHeader')
  },
  data () {
    return {
		
    }
  },
  mounted() {
    console.log('点击了',111)
  },
  methods: {
  }
}
</script>

<style lang="less">
.content {padding: 0 10px;background-color: #f0f0f0;}
.new_add{
  overflow: scroll;
  box-sizing: border-box;
  .txt {
	  margin-top: 10px;
	  height: 100px;
	  border-radius: 5px;
	  background-color: #fff;
	  font-size: 12px;
  }
  
}
</style>