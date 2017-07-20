<template>
	<div id="box">
		<input autofocus class="searchInp" type="text" v-model="keyWord" @keyup="get($event)" @keydown.down="down" @keydown.up="up"/>
		<div class="searchContent">
			<transition-group name="itemfade" tag="ul" mode="out-in" v-cloak>
				<li v-for="(v,i) in myData" :key="v" :class="[activeIndex==i?'active':'',hoverIndex==i?'hover':'']" @mouseover="over(i)" @mouseout="out" @click="sel(v,i)">{{v}}</li>
			</transition-group>
		</div>
	</div>
</template>
<script>
	var jsonp = require("jsonp");
	export default {
		name:'search',
		data(){
			return {
				myData:[],
				keyWord:"",
				activeIndex:-1,
				hoverIndex:-1,
			}
		},
		methods:{
			get:function(ev){
				var that = this;
				if( ev.keyCode == 38 || ev.keyCode == 40 ) return;
//				console.dir( jsonp );
				jsonp('https://sug.so.360.cn/suggest?word=' + this.keyWord + '&encodein=utf-8&encodeout=utf-8',{},function(res,data) {
					that.myData = data.s;
//					console.log( that.myData );
           		});
			},
			over:function(a){
				this.hoverIndex = a;
			},
			up:function(){
				this.activeIndex--;
				if(this.activeIndex<0)this.activeIndex = this.myData.length-1;
			},
			down:function(){
				this.activeIndex++;
				this.activeIndex %= this.myData.length;
			},
			out:function(){
				this.hoverIndex = -1;
			},
			sel:function(a,b){
				this.activeIndex = b;
				this.keyWord = a;
			}
		}
	}
</script>
<style>
	li{
		list-style: none;
		height: 40px;
		font: 20px/2 "微软雅黑";
	}
	
	/*li{
		
	}*/
	#box{
		overflow: hidden;
	}
	.searchInp{
		height: 50px;
		width: 500px;
		text-indent: 20px;
		outline: none;
		font: 20px/50 "微软雅黑";
	}
	.searchContent{
		width: 500px;
		margin: 0 auto;
	}
	.searchContent ul{
		padding: 0;
	}
	.searchContent li{
		transition: 0.4s;
		text-align: left;
		margin-bottom: 10px;
		background-color: pink;
	}
	
	.searchContent .active{
		background-color: orangered;
		color: #fff;
	}
	.searchContent .hover{
		background-color: yellowgreen;
		color: #fff;
	}
	.v-enter{
		transform: scale(2);
		
	}
	
	.itemfade-enter,.itemfade-leave-active {
	    opacity: 0;
	    transition: .4s .4s;
	}
	
	.itemfade-leave-active {
	    transform: scaleY(0);
	    height:0px
	    
	}

</style>