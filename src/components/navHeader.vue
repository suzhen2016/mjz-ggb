<template>
  <div class="hb-header">
    <x-header :left-options="{showBack: back}" :right-options="{showMore: false}" @on-click-more="showMenus = true">
      	{{title}}
        <a slot="right" v-if='right' @click='goNext()'>{{right}}</a>
		<a slot="left" class='back' v-if='left' @click='goNext()'> <i class="left-arrow"></i>{{left}}</a>
    </x-header>
  </div>
</template>

<script>
import { XHeader, Actionsheet, TransferDom, ButtonTab, ButtonTabItem } from 'vux'

export default {
	directives: {
		TransferDom
	},
	components: {
		XHeader,
		Actionsheet,
		ButtonTab,
		ButtonTabItem
	},
	props:{
		title:{
			type:String,
		},
		back:{
			type:String,
		// required:true,
		},
		right:{
			type:String,
		},
		left: {
			type: String,
		},
		right_link:{
			type:String,
			required:false,
		},
		query:{
			type:Number,
			required:false,
		}
    
  	},
	data () {
		return {
			menus: {
				menu1: 'Take Photo',
				menu2: 'Choose from photos'
			},
			showMenus: false
		}
	},
	created() {},
	methods:{
		goNext(){
			if(this.right_link){
				if(!this.query) this.$router.push({name:this.right_link})
				else this.$router.push({name:this.right_link,query:{type:this.query}})
			}
		}
	}
}
</script>

<style lang="less">
	.vux-header{
		background-color: #fff !important;
		border-bottom: 1px solid #ccc;
	}
	.vux-header-back{
		color:#000 !important;
	}
	.vux-header-title{
			color:#000 !important;
	}
	.hb-header{
		.vux-header .vux-header-right a {
			color:#333;
		}
		.vux-header-left .back {
			padding-left: 16px;
			color: #000;
			.left-arrow {
				color : #ccc;
			}
		}
	}

  
</style>