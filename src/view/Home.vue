<template>
  <div class='pre' style="height:calc(100% - 44px)">
    <!-- 顶部 -->
    <nav-header title='Askme'></nav-header> 
    <!-- 内容 -->
	<div class='content'>
		<div class="nav">
			<flexbox class='flex-box' wrap="wrap">
				<flexbox-item><div class="flex-demo">公链</div></flexbox-item>
				<flexbox-item><div class="flex-demo">钱包</div></flexbox-item>
				<flexbox-item><div class="flex-demo">交易所</div></flexbox-item>
				<flexbox-item><div class="flex-demo">职场</div></flexbox-item>
			</flexbox>
			<flexbox class='flex-box flex-two' wrap="wrap">
				<flexbox-item><div class="flex-demo">联盟链</div></flexbox-item>
				<flexbox-item><div class="flex-demo">前端</div></flexbox-item>
				<flexbox-item><div class="flex-demo">java</div></flexbox-item>
				<flexbox-item><div class="flex-demo">更多</div></flexbox-item>
			</flexbox>
		</div>
	</div>
	
	<toast v-model="show_err" position='middle' type="text" :text="error"></toast>
    <!-- 底部 -->
    <tab class="tab_footer" :active='0'></tab>
  </div>
</template>

<script>
import { Group, Cell,Toast, Flexbox, FlexboxItem,Actionsheet } from 'vux'
import api from '../until/help/api'
import { mapState,mapMutations } from "vuex";
export default {
  components: {
    Group,
    Cell,
    Actionsheet,Toast,Flexbox,FlexboxItem,
    tab:()=>import('@/components/tab'),
    navHeader:()=>import('@/components/navHeader')
  },
  data () {
    return {
      	msg: 'Hello World!',
	  	val:'ww',
	  	error:'',
		show_err:false,
		list:[],
		isLoad:false,
    }
  },
  computed: {
	  ...mapState(["userInfo","totalPrice"])
  },
  created() {
  },
  mounted() {
	  if(this.list.length==0){
		  this.getPriceDetail()
	  }
  },
  activated() {
	  if(this.list.length>0){
		  this.asyncPrice();
	  }else this.getPriceDetail()
  },
  
  methods: {
	  ...mapMutations(['setTotalPrice']),
    goDetail(i){
      this.$router.push({name:'Detail',params:{hb:i}})
	},
	setDb(type){
		if(this.userInfo.id){
			this.$router.push({name:'Findb',query:{type:type}})
		}else{
			this.$router.push({name:'Login'})
		}
		
	},
	//获取价格详情
	 async getPriceDetail(){

		this.isLoad = false;//findAssetByUserAndCoinTypeId {userId :this.userInfo.id,coinTypeId:i.coinTypeId,status:1}
		
		if(!this.userInfo.id){
			this.isLoad = true;
			return false;
		}
		let res = await api.APIPOSTMAN('POST','/market/findAllMarket');
	
		let hb = await api.APIPOSTMAN('POST','/asset/findAssetByUserId',{userId:this.userInfo.id,status:'1'})
			if(hb.data.code==200){
				this.list = hb.data.result.list;
				this.asyncPrice()
			}else{
				this.error = res.data.message;
				this.show_err = true;
				this.isLoad = false;
			}
	},
	async asyncPrice(){
		let res = await api.APIPOSTMAN('POST','/market/findAllMarket');
		let totalPrice={
			price: 0,
			BTC:0,
			btc_price:0,
		}	
		if(res.data.code == 200){
			this.list.map( (i)=>{
				let hb_price = res.data.result.filter(item =>{
					if(item.coinId == i.coinTypeId){
						return item
					}
				})[0];
				let pr = i.usableBalance * hb_price.sell;
				i.price = parseInt(pr *100) / 100;
				i.detail = hb_price || {};
				totalPrice.price +=(i.price-0);
				if(i.detail.coinName=='BTC'){
					totalPrice.btc_price = Number(hb_price.sell);
				}
			})
		}
				
		totalPrice.BTC = parseInt((totalPrice.price / totalPrice.btc_price) * 100000000) /100000000 ;
		this.isLoad = true;
		console.log(totalPrice,this.list)
		this.setTotalPrice(totalPrice)
	}
  },
}
</script>

<style lang="less">
	.content {
		// padding: 0 10px;
		.nav{
			border: 1px solid #ccc;
			padding: 10px;
			.flex-two {margin-top: 10px;}
			.vux-flexbox-item {text-align: center;}
			.flex-demo {
				border: 1px solid #ddd;
				height: 60px;
				width: 60px;
				border-radius: 50%;
				line-height: 60px;
				text-align: center;
				display: inline-block;
			}
		}
	}
	
</style>
