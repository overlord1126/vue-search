<template>
	<div id="box">
		<div class="search_pannel">
			<!--搜索框，ref为真实的DOM节点，便于当失去焦点的时候，找回焦点-->
			<input ref="inp" autofocus @blur="getFocus" class="searchInp" type="text" v-model="keyWord" @keyup="get($event)" @keydown.down="down" @keydown.up="up" @keydown.enter="search"/>
			<!--清空输入框中内容，输入框中由内容才会显示-->
			<span id="keyWordReset" @click="resetkeyWord" v-if="keyWord">&times;</span>
		</div>
		<div class="searchContent">
			<!--过渡，使用ul包裹元素-->
			<transition-group name="itemfade" tag="ul" mode="out-in" v-cloak>
				<li v-for="(v,i) in myData" :key="v" :class="{active:activeIndex==i,hover:hoverIndex==i}" @mouseover="over(i)" @mouseout="out" @click="sel(v,i)">{{v}}</li>
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
				searchTypeIndex:0,
				searchType: [{
	                name: '360搜索',
	                searchSrc: 'https://www.so.com/s?ie=utf-8&shb=1&src=360sou_newhome&q='
	            }, {
	                name: '百度搜索',
	                searchSrc: 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd='
	            }, {
	                name: '搜狗搜索',
	                searchSrc: 'https://www.sogou.com/web?query='
	            }]
			}
		},
		methods:{
			get:function(ev){
				var that = this;
				if( ev.keyCode == 38 || ev.keyCode == 40 ) return;
				jsonp('https://sug.so.360.cn/suggest?word=' + this.keyWord + '&encodein=utf-8&encodeout=utf-8',{},function(res,data) {
					that.myData = data.s;
           		});
			},
			getFocus:function(){
				this.$refs.inp.focus();
			},
			over:function(a){
				this.hoverIndex = a;
			},
			up:function(){
				this.activeIndex--;
				if(this.activeIndex<0)this.activeIndex = this.myData.length-1;
				this.keyWord = this.myData[this.activeIndex];
			},
			down:function(){
				this.activeIndex++;
				this.activeIndex %= this.myData.length;
				this.keyWord = this.myData[this.activeIndex];
			},
			out:function(){
				this.hoverIndex = -1;
			},
			sel:function(a,b){
				var that = this;
				this.activeIndex = b;
				this.keyWord = a;
				setTimeout(that.search,500)
			},
			resetkeyWord:function(){
				var that = this;
				this.keyWord = "";
				jsonp('https://sug.so.360.cn/suggest?word=' + this.keyWord + '&encodein=utf-8&encodeout=utf-8',{},function(res,data) {
					that.myData = data.s;
           		});
			},
			search:function(){
				window.open( this.searchType[this.searchTypeIndex].searchSrc + this.keyWord )
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
	#box{
		overflow: hidden;
	}
	.searchInp{
		height: 50px;
		width: 500px;
		text-indent: 20px;
		outline: none;
		font: 20px/50px "微软雅黑";
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
		width: 500px;
		height: 40px;
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
	
	.itemfade-move{
		transition: .5s;		
	}
	
	.itemfade-enter,.itemfade-leave-active {
	    opacity: 0;
	    transition: 1s;
	    position: absolute;
	}
	
	.itemfade-leave-active {
	    transform: scaleY(0);
	}
	
	#keyWordReset{
		position: absolute;
		right: 20px;
		top: 6px;
		font: bold 30px "微软雅黑";
		cursor: pointer;
	}
	#keyWordReset:hover{
		color: #ccc;
	}	
	.search_pannel {
		border: 1px solid #FF4500;
		display: inline-block;
		position: relative;
	}
</style>