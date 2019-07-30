<template>
  <div class='pre'>
    <!-- 顶部 -->
    <nav-header title='助记词' back='true'></nav-header> 
    <!-- 内容 -->
    <div class="word">
        <span v-for="(name,i) in word_arr" :key = "i">{{name}}</span>
    </div> 
	<p class='toast'>请牢记助记词</p>
	<x-button class="next" type="primary" @click.native='nextSet()'>下一步</x-button>
	<toast v-model="show_err" position='middle' type="text" :text="err_txt"></toast>
    <!-- 底部 -->
  </div>
</template>

<script>
import factory from '../../until/factory' 
import { mapState,mapMutations } from "vuex";
import api from '../../until/help/api'
import { XButton, Toast } from 'vux'
export default {
	components: {
		Toast, XButton,
		navHeader:()=>import('@/components/navHeader')
	},
	data () {
		return {
			msg: 'Hello World!',
			key : '1234567890123456',
			iv : '2624750004598718',
			data : 'GPKYYIEuwOPOftyyxBE1h25nQyu2oJGtzLBXpFq1U7S3gxP4i+ovg8BVD/ZUSUDuDZ2GAgJn2r+//6t6LqdJU2WrB/cZ2XbpSXkEFPUt31I=',
			str : 'shed color since rude buyer enable rabbit rookie can harsh pause cheese',
			word_arr : [],
			err_txt: '生成助记词失败',
			show_err : false
		}
	},
	computed: {
			
	},
	mounted() {
		this.init();
	},

	methods: {
		async init(){
			try {
				const params = {"number":12, "language": "english", "passwd": "123457" };
				let res = await api.APIPOSTMAN('POST','/mnemonic', params);
				if (res.code=='200' && res.result && res.result.mnemonic ){
					this.word_arr = factory.decrypt(this.key, this.iv, res.result.mnemonic).split(' ');
					factory.Storage.set('helpWord',{sequence: res.result.sequence, mnemonic: res.result.mnemonic});
					
				}
				// this.word_arr = factory.decrypt(this.key,this.iv,this.data).split(' ');

				// factory.Storage.set('helpWord',{sequence: '', mnemonic: this.data});

			} catch (error) {
				console.log(error)
			}
		},

		nextSet() {
			this.$router.push({name:'Pick_word'})
		}
	},
}
</script>

<style lang="less">
	// 覆盖vux的组件样式
	.word{
		margin: 80px 20px 0px;
		padding: 10px;
		border-radius: 5px;
		background-color: #f9f9f9;
		color: #0c0c0c;
		span {display: inline-block;line-height: 15px;padding: 5px;}
	}
	.toast {
		text-align: center;
		margin-top: 20px;
	}

	.next.weui-btn {
		width: 40%;
		margin-top: 100px;
		width: 40%;
		line-height: 1.9;}

</style>
