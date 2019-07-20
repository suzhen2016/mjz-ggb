<template>
  <div class = 'pre ' >
    <!-- 顶部 -->
    <nav-header title='钱包' back='true'></nav-header> 
    <!-- 内容 -->
    <div class="content history_db"  style="height:calc(100% - 44px)">
		<div class="price-box">
			<p>0.00</p>
			<p>￥：0.00</p>
		</div>
		<h4>交易记录</h4>
		<!-- v-for="(i,index) in list" :key='index' 72 -->
		<div class="no-data" style="height: calc(100% - 250px)">
			<div class="bg"></div>
			<p>暂无记录</p>
		</div>
		<div class="trade" @click="goMoany()">
			收款
		</div>

    </div>    
  </div>
</template>

<script>
import { Actionsheet, Group, XSwitch, Toast } from 'vux'
import api from '../../until/help/api'
import { mapState,mapMutations } from "vuex";
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
			menus:{
				'put':{
					menu:'普通充币'
				},
				'pick':{
					menu:'提币'
				}
			},
			show:false,
			type:'',
			list:[]
		}
	},
	mounted() {
		console.log(this.$route)
		this.init()
	},
	computed: {
		...mapState(["userInfo","totalPrice"])
	},
	methods: {
		goMoany(){
			this.$router.push({name:'Findb',query:{type:this.$route.query.type}})
		},
		async init(){
			try {
				let res = await api.APIPOSTMAN('POST','/transRecord/findTransRecordByConditon',
				{userId:this.userInfo.id,coinTypeId:this.$route.query.type,status:'1'})
				if(res.data.code==200){
					this.list = res.data.result.list;
					console.log(this.list)
				}else{
					this.error = res.data.message;
					this.show_err = true;
					this.isLoad = false;
				}
			} catch (error) {
				
			}
		}
	}
}
</script>

<style lang="less">
    .history_db{
		overflow: scroll;
		background-color: #f0f0f0;
		.price-box {height: 60px;padding-top:60px;background-color:#fff; p {text-align: center;}}
		.price_list{
			background-color: #fff;
			box-sizing: border-box;
		}	
		h4 {font-weight: 500;padding: 5px 10px;}
		//无数据展示
		.no-data{
			width: 100%px;
			background-color:#fff;
			padding-top: 50px; 
			div{
				height: 98px;
				width: 130px;
				margin: 0 auto;
				background: url('../../assets/img/no_data.png') center center no-repeat;
				background-size: cover;
			}
			P{
				text-align: center;
				margin-top: 14px;
				height: 16px;
				line-height: 16px;
				font-size: 14px;
				color:#D5D5D5;
			}
		}
		.trade {
			height: 42px;
			line-height: 22px;
			background-color: #fff;
			text-align: center;
			cursor: pointer;
		}
}
</style>