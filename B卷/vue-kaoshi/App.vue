<template>
	<div id="app1">
		<div id="left">
			<span>用户名：</span>
			<input type="text" v-model="userName" v-if="isUserShow">
			<span v-else>{{userName}}</span>
			<span class="show" v-if="isShow">长度必须是6--18位之间</span>
			<br><br>
			<button @click="login1">{{login}}</button>
		</div>
		<div id="right">
			<div v-if="isShow2">
				<div class="mo" v-show="isMoShow"></div>
				<div class="item" v-for="(item,index) in infoList" :key="index" v-show="currentIndex == index">
					<h2>{{item.title}}</h2>
					<div v-for="(item1,index1) in item.options" :key="index1">
						<input type="radio" :value="index1" v-model="t"><span>{{item1}}</span><br>
					</div>
				</div>
				<button @click="nextTest" :disabled="dis">{{add}}</button>
				<p v-show="isShow1">{{num}}后进入下一题</p>
			</div>
			<div v-else>
				<span>没有题目了</span>
				<button @click="res">再来一遍</button>
			</div>
			
			<div class="footer">
				<span>共有{{currentNum}}/{{allNum}}道题,</span><span>答对：{{trueNum}}</span><span>答错:{{falseNum}}</span>
			</div>
		</div>
	</div>
</template>

<script>
	import data1 from "../assets/data1.json"
	export  default {
		name:"test2",
		data(){
			return {
				userName:"",
				login:"登录",
				isShow:false,
				isUserShow:true,
				isMoShow:true,
				infoList:data1.data,
				currentIndex:0,
				t:-1,
				add:"提交答案",
				currentNum:1,
				allNum:data1.data.length,
				trueNum:0,
				falseNum:0,
				num:3,
				isShow1:false,
				dis:false,
				isShow2:true
			}
		},
		methods:{
			login1(){
				if(this.login == "登录"){
					let reg = /^\w{6,18}$/
					let flag = reg.test(this.userName)
					if(flag){
						this.isMoShow = false
						this.login = "退出"
						this.isShow = false
						this.isUserShow = false
					}else{
						this.isShow = true						
					}
				}else{
					this.login = "登录"
					this.isMoShow = true
					this.isUserShow = true
					this.userName = ""
					this.isShow2 = true
					this.currentIndex = 0
					this.currentNum = 1
					this.trueNum = 0
					this.falseNum = 0
				}
				
			},
			nextTest(){
				if(this.infoList[this.currentIndex].right == this.t){
					this.trueNum++ 
					this.add = "回答正确"
				}else{
					this.falseNum++
					this.add = "回答错误"
				}
				this.dis = true
				this.isShow1 = true
				let timer = setInterval(()=>{					
					this.num--
					if(this.num == 0){
						this.dis = false
						this.isShow1 = false
						this.num = 3
						this.add = "提交答案"
						clearInterval(timer)
						this.currentIndex++
						this.currentNum++
						this.t = -1
						if(this.currentIndex == this.infoList.length){
							this.isShow2 = false
						}	
					}					
				},1000)
					
			},
			res(){
				this.isShow2 = true
				this.currentIndex = 0
				this.currentNum = 1
				this.trueNum = 0
				this.falseNum = 0		
			}
		}
	}
</script>

<style>
	#left{
		width: 40%;
		float: left;
		height: 400px;
		border: 1px  solid #ccc;
	}
	#right{
		position: relative;
		width: 40%;
		float: left;
		height: 400px;
		border: 1px solid #ccc;
	}
	.show{
		color: red;
	}
	.mo{
		width: 100%;
		height: 100%;
		position: absolute;
		top: 0px;
		left: 0px;
		background: rgba(0,0,0,.9);
	}
	.footer{
		position: absolute;
		bottom: 10px;
		left: 0px;
		right: 0px;
	}
</style>
