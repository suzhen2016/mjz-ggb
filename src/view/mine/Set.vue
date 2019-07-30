<template>
  <div class='pre'>
    <!-- 顶部 -->
    <nav-header title='设置' back='true'></nav-header> 
    <!-- 内容 -->
    <div class="content set " style="height:calc(100% - 44px)">
      <group>
        <cell :title="item.title" is-link v-for='(item,index) in list' :key='index' :link='item.link'>
          <img slot="icon" width="20" style="display:block;margin-right:5px;" src="../../assets/img/shi.png" v-if='index==0'>
          <img slot="icon" width="20" style="display:block;margin-right:5px;" src="../../assets/img/change_pass.png" v-if='index==1'>
          <img slot="icon" width="20" style="display:block;margin-right:5px;" src="../../assets/img/change_zijin.png" v-if='index==2'>
        </cell>
      </group>
    </div> 
    <!-- 底部 -->
  </div>
</template>

<script>
import { Group, Cell } from 'vux'
import factory from '../../until/factory' 
import { mapState,mapMutations } from "vuex";
export default {
  components: {
    Group,
    Cell,
    tab:()=>import('@/components/tab'),
    navHeader:()=>import('@/components/navHeader')
  },
  data () {
    return {
      list:[
        {title:'修改名称', img:'', link:'/change_name',url:'src/assets/img/change_pass.png'},
        {title:'修改密码', img:'', link:'changeFund',url:'src/assets/img/change_zijin.png'},
        {title:'钱包备份', img:'', link:'/custom',url:'src/assets/img/kefu.png'},
      ],
      msg: 'Hello World!',
    }
  },
  computed: {
         
  },
  mounted() {
  },
  methods: {
  },
}
</script>

<style lang="less">
// 覆盖vux的组件样式
.set{
  .weui-cells::before{
    border-top:0px !important;
  }
  .vux-no-group-title{
    margin-top: 0px !important;
  }  
  .weui-cells{
    &::after{
      border-bottom:0px !important;
    }
    margin-top: 0px;
    .vux-label{
      font-size: 16px;
      font-weight: 400;
      line-height: 16px;
      color:#333;
      text-indent: 13px;
    }
    .weui-cell::before{
      border-top:0px;
    }
    .weui-cell::after{
      border-top:0px;
    }
    
  }
}

</style>
