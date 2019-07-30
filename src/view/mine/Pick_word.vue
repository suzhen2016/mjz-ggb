<template>
  <div class='pre'>
    <!-- 顶部 -->
    <nav-header title='助记词' back='true'></nav-header> 
    <!-- 内容 -->
    <div class="write-word">
        <group title="助记词">
            <x-textarea :max="200" name="description" v-model="query"  placeholder="请输入您的助记词"></x-textarea>
          </group>
    </div> 
	<p class='toast'>请输入助记词后进行下一步</p>
	<x-button class="next" type="primary" @click.native='nextSet()'>下一步</x-button>
	<toast v-model="show_err" position='middle' type="text" :text="err_txt"></toast>
    <!-- 底部 -->
  </div>
</template>

<script>
import factory from '../../until/factory' 
import { mapState,mapMutations } from "vuex";
import api from '../../until/help/api'
import { XButton, Toast, Group, XTextarea } from 'vux'
export default {
	components: {
		Toast, XButton,Group,XTextarea,
		navHeader:()=>import('@/components/navHeader')
	},
	data () {
		return {
			msg: 'Hello World!',
			key : '1234567890123456',
			iv : '2624750004598718',
			data : 'GPKYYIEuwOPOftyyxBE1h25nQyu2oJGtzLBXpFq1U7S3gxP4i+ovg8BVD/ZUSUDuDZ2GAgJn2r+//6t6LqdJU2WrB/cZ2XbpSXkEFPUt31I=',
			query : 'shed color since rude buyer enable rabbit rookie can harsh pause cheese',
			word_arr : [],
			err_txt: '提交失败',
			show_err : false
		}
	},
	computed: {
			
	},
	mounted() {
	},

	methods: {
		async init(){
			try {
				// let res = await api.APIPOSTMAN('POST','/mnemonic');
				// this.word_arr = this.decrypt(this.key,this.iv,this.data).split(' ');
				// console.log(this.word_arr)
			} catch (error) {
				console.log(error)
			}
		},
		
		async nextSet() {
			let wordAsync = factory.crypted(this.key,this.iv,this.query);
			let wordStorage = factory.Storage.get('helpWord') || {};
			if (wordAsync !== wordStorage.mnemonic) {
				this.err_txt = '填写助记词错误';
				this.show_err = true;
			} else {
				let res = await api.APIPOSTMAN('POST','/mnemonic', {sequence: wordStorage.sequence,number: 0, language: 'english', passwd: '123457'});
				if(res.code == '200') {
					this.$router.push({name:'Help'})
				}
				
				// this.$router.push({name:'Help'})
			}
		}
	},
}
</script>

<style lang="less">
	// 覆盖vux的组件样式
	.write-word{
		margin: 80px 20px 0px;
		padding: 10px;
		border-radius: 5px;
		background-color: #f9f9f9;
		color: #0c0c0c;
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
