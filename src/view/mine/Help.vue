<template>
  <div class="pre" >
       <!-- 顶部 -->
      <nav-header title='设置' back='true'></nav-header> 
      <div class='content help' style="height:calc(100% - 44px)">
        <ul class='clearfix ul-box' style="height: calc(100% - 188px)">
          <li @click='goDetail()'>
            <i>@</i> 苏氏之道
          </li>
		  <li @click='goDetail()'>
            <i>@</i> 苏氏之道
          </li>
		  <li @click='goDetail()'>
            <i>@</i> 苏氏之道
          </li>
		  <li @click='goDetail()'>
            <i>@</i> 苏氏之道
          </li>
		  <li @click='goDetail()'>
            <i>@</i> 苏氏之道
          </li>
        </ul>
		<x-button class="btn"  type="primary" @click.native='goDetail()'>新建</x-button>
		<x-button  type="primary" @click.native='confirm()'>导入</x-button>
      </div>
  </div>
</template>

<script>
import api from '../../until/help/api'
import { XButton } from 'vux';
export default {
  name: 'help',
  data () {
    return {
      list:[
        {text:'交易所使用教程'},
        {text:'货币/提币'},
        {text:'区块链知识'},
        {text:'公告中心'}
      ]
    }
  },
  components: {
    navHeader:()=>import('@/components/navHeader'), XButton
  },
  methods: {
    goDetail(){
      this.$router.push({name:'New_add'})
    }
  },
}
</script>

<style  lang="less">
.gap{
  margin-top: 160px;
}
.help{
  .ul-box{
	padding: 0 20px;
	overflow-y: auto;
    li{
		margin-top: 20px;
		width: 100%;
		height: 57px;
		line-height: 57px;
		text-align: center;
		border:1px solid #ccc;
		border-radius: 10px;
		color:#fff;
		font-size: 16px;
		text-align: left;
		background-image:url("../../assets/img/help_1.png");
		background-size: cover;
		background-repeat: no-repeat;
		i {margin: 0 10px;}
    }
    // li:nth-child(2){
    //   background-image:url("../../assets/img/help_2.png");
    // }
    // li:nth-child(3){
    //   background-image:url("../../assets/img/help_3.png");
    // }
    // li:nth-child(4){
    //   background-image:url("../../assets/img/help_4.png");
    // }
  }
  button.weui-btn {width: 25%;line-height: 2;font-size: 14px;}
}

</style>
