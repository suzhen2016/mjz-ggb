<template>
  	<div class = 'pre' >
    	<!-- 顶部 -->
		<nav-header title='钱包' back='true'></nav-header> 
		<!-- 内容 -->
		<div class="content new_add"  style="height:calc(100% - 44px)">
			<div class="txt">
				会带坏哈苏打好好撒大护法撒胡发到发撒发生
			</div>
			<div class='content Zhuce'>
				<group class='register-input'>
					<x-input  v-model = "query.phone" :max='11' name="mobile" type='tel' placeholder="请输入手机号码" keyboard="number" is-type="china-mobile"><x-button slot="right" mini @click.native="getCode()">{{time? time+'s重新获取':'发送验证码'}}</x-button></x-input>
					<x-input  v-model = "query.code" type="number" :max="6"  placeholder="请填写6位数验证码" ></x-input>
					<x-input  v-model = "query.loginPwd" type='password'  placeholder="至少6位任意字符" :min="6" ></x-input>
				</group>
				<x-button class="next" type="primary" @click.native='nextSet()'>下一步</x-button>
				<toast v-model="show_err" position='middle' type="text" :text="err_txt"></toast>
			</div>
		</div>    
  	</div>
</template>

<script>
import { Actionsheet, XInput, XButton,Group, Cell, XSwitch, Toast } from 'vux'
export default {
  components: {
    Group,
    XSwitch,
	Toast, XButton,XInput,Cell,
	Actionsheet,
    navHeader:()=>import('@/components/navHeader')
  },
  data () {
    return {
		query : {},
		show_err:false,
		err_txt:'',
		time_Interval:null,
		time:0,
    }
  },
	mounted() {
	},
	methods: {
	   //获取验证码
		getCode(){
			if(!this.query.phone){
				this.err_txt='请输入手机号'
				this.show_err = true;
				return false;
			}
			if(this.time>0){
				return false;
			}
			this.time_Interval = null;
			this.time +=1
			this.time_Interval = setInterval(() => {
				this.time +=1;
				if(this.time>60){
					clearInterval(this.time_Interval);
					this.time_Interval = null;
					this.time = 0;
				}
			}, 1000);
			// api.getCode('',{phone:this.query.phone}).then((res)=>{
			// 	console.log(res)
			// 	if(res.data.code==200){

			// 	}else {
			// 		this.err_txt=res.data.message;
			// 		this.show_err = true;
			// 	}
			// })
		},

		nextSet() {
			this.$router.push({name:'Z_word'})
		}
  	}
}
</script>

<style lang="less">
.new_add{
  overflow: scroll;
  padding: 0 10px;
  box-sizing: border-box;
	background-color: #f0f0f0;
	
  .txt {
	  margin-top: 50px;
	  border-radius: 5px;
    font-size: 14px;
    color: #b7b2b2;
    text-align: center;
	}
	
  .register-input {
	  .weui-cells {
		  border-radius: 8px;
	  }
	}
	
  .next {
	  margin-top: 100px;
		width: 40%;
		line-height: 1.9;
  }
  
}
</style>