<template>
	<div id="list-demo" class="demo">
		<button class="add" @click="add">添加</button>
		<transition-group name="list" tag="ul">
			<li v-for="(item,i) in myArr" v-bind:key="item">
		    	{{ item }}
				<span class="delete" @click="del(i)">删除</span>
		    </li>
		</transition-group>
	</div>
</template>
<script>
	export default {
		data() {
				return {
					myArr: [
						111111111,
						222222222,
						333333333,
						444444444,
						555555555,
						666666666,
						777777777,
						888888888
					],
					now : 9
				}
			},
			methods: {
				del: function(i) {
//					this.myArr.splice(Math.floor( Math.random()*this.myArr.length ),0, ++this.now );
					this.myArr.splice(i,1);
					
				},
				add: function(){
//					this.myArr.push( ++this.now );
					this.myArr.splice(Math.floor( Math.random()*this.myArr.length ),0, ++this.now );
				}
			}
	}
</script>
<style scoped>
	#list-demo li {
		height: 40px;
		width: 300px;
		border: 1px solid #000;
		text-align: left;
		font: 16px/40px "微软雅黑";
		margin-bottom: 10px;
	}
	
	#list-demo .delete {
		float: right;
		width: 50px;
		background-color: coral;
		text-align: center;
		color: #fff;
	}
	
	.list-item {
		display: inline-block;
		margin-right: 10px;
	}
	
	.list-enter-active,
	.list-leave-active {
		transition: all .8s;	
		position: absolute;
	}
	
	.list-leave-to {
		opacity: 0;
		/*transform: translateX(300px) scale(.5) rotate(360deg);*/
		transform: translateX(300px);
	}
	
	.list-enter{
		/*transform-origin: top;
		transform: rotateX(90deg);*/
		
		transform: translateX(300px);
		
	}
	.list-enter-to{
		/*transform: rotateX(0deg);*/
	}
	.list-move {
		transition: all .5s;
	}
</style>