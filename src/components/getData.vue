<template>
	<div id="box">
		<input class="getdata" type="button" value="获取数据" @click="getData"/>
		<ul id="list">
			<li v-for="(item,i) in myData">
				<img :src="item.images.large"/>
				<a :href="item.alt">{{item.title}}</a>
				<div>
					<a :href="v.alt" class="cast" v-for="(v,index) in item.casts">{{v.name}}</a>
				</div>
			</li>
		</ul>
	</div>
</template>
<script>
	var jsonp = require("jsonp");
	export default{
		data(){
			return {
				myData:[]
			}
		},
		methods:{
			getData:function(){
				var that = this;
				jsonp('https://api.douban.com/v2/movie/in_theaters',{},function(res,data){
					console.log(data.subjects);
					that.myData = data.subjects;
				});
			}
		}
		
		
	}
</script>
<style>
	ul{
		padding: 0;
		text-align: left;
	}
	li{
		list-style: none;
	}
	.getdata{
		font: 20px/2 "微软雅黑";
		color: #fff;
		width: 100px;
		height: 40px;
		background: yellowgreen;
		border-radius: 10px;
		border: none;
		outline: none;
	}
	.getdata:active{
		background-color: greenyellow;
	}
	#list{
		font: 14px/ 20px "微软雅黑";
	}
	#list li{
		width: 25%;
		float: left;
		height: 250px;
		border: 1px solid #000;
		box-sizing: border-box;
		text-align: center;
	}
	#list img{
		display: block;
		margin: 0 auto;
		width: 150px;
		height: 200px;
	}
	#list span{
		/*display: block;*/
		/*text-align: center;*/
	}
	#list .cast{
		display: inline-block;
		margin: 0px 2px;
	}
</style>