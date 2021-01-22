<template>
	<view class="container">
	  <canvas type="webgl" id="myCanvas" @touchstart="touchEvent" @touchmove="touchEvent" @touchend="touchEvent" @touchcancel="touchEvent"></canvas>
	  <canvas type="2d" id="canvas2d"></canvas>
	</view>
</template>

<script>
	import {createPIXI} from "../../libs/pixi.miniprogram"
	var unsafeEval = require("../../libs/unsafeEval")
	var installSpine = require("../../libs/pixi-spine")
	var PIXI = {};
	var app = getApp()
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {
			var query2d = wx.createSelectorQuery();
					var query = wx.createSelectorQuery();
					query2d.select('#canvas2d').fields({ node: true, size: true }).exec((res2d) => {
						var canvas2d = res2d[0].node;
						query.select('#myCanvas').node().exec((res) => {
							var stageWidth = 750;//canvas宽度，跟小程序wxss指定的一样大小
							var stageHeight = 1220;//canvas高度，跟小程序wxss指定的一样大小
							var canvas = res[0].node;
							PIXI = createPIXI(canvas,stageWidth,canvas2d);//传入canvas，传入canvas宽度，用于计算触摸坐标比例适配触摸位置
							unsafeEval(PIXI);//适配PIXI里面使用的eval函数
							installSpine(PIXI);//注入Spine库
							var renderer = PIXI.autoDetectRenderer({width:stageWidth, height:stageHeight,'transparent':false,'view':canvas});//通过view把小程序的canvas传入
							var stage = new PIXI.Container();
							var bg = PIXI.Sprite.from("../../static/img/bg.jpg");
							stage.addChild(bg);
							var bg = PIXI.Sprite.from("../../static/img/bg.jpg");
							stage.addChild(bg);
							bg.interactive=true;
							bg.on("touchstart",function(e){
								console.log("touchstart",e.data.global)
							});
							bg.on("pointerup",function(e){
								console.log("touchend")
							});
							//小程序不支持加载本地fnt，json文件，所以涉及到fnt，json文件的加载需要放到网络服务器
							PIXI.Loader.shared
								.load(function(loader, res){
									
								// var btext = new PIXI.BitmapText('score:1234',{'font':{'name':'blog','size':'60px'},'tint':0xffff00});
								// btext.x = 40;
								// btext.y = 40;
								// stage.addChild(btext);
							
								// var spineBoyPro = new PIXI.spine.Spine(res.spineboypro.spineData);
								// spineBoyPro.x = stageWidth / 2;
								// spineBoyPro.y = 1200;
			
								// spineBoyPro.scale.set(0.5);
								// spineBoyPro.state.setAnimation(0, "hoverboard",true);
								// stage.addChild(spineBoyPro);
								
								const graphics = new PIXI.Graphics();
								graphics.beginFill(0xFF3300);
								graphics.drawRect(50, 250, 100, 100);
								graphics.endFill();
								stage.addChild(graphics);
			
								// 画矩形
								let rectangle_j = new PIXI.Graphics();
								rectangle_j.lineStyle(4, 0xFF3300, 1);
								rectangle_j.beginFill(0x66CCFF);
								rectangle_j.drawRect(0, 0, 64, 64);
								rectangle_j.endFill();
								rectangle_j.x = 170;
								rectangle_j.y = 170;
								stage.addChild(rectangle_j);
				
								// 画圆形
								let circle = new PIXI.Graphics();
								circle.beginFill(0x9966FF);
								circle.drawCircle(0, 0, 32);
								circle.endFill();
								circle.x = 64;
								circle.y = 130;
								stage.addChild(circle);
				
								// 画椭圆
								let ellipse = new PIXI.Graphics();
								ellipse.beginFill(0xFFFF00);
								ellipse.drawEllipse(0, 0, 50, 20);
								ellipse.endFill();
								ellipse.x = 180;
								ellipse.y = 130;
								stage.addChild(ellipse);
				
								// 画圆角矩形
								let roundBox = new PIXI.Graphics();
								roundBox.lineStyle(4, 0x99CCFF, 1);
								roundBox.beginFill(0xFF9933);
								roundBox.drawRoundedRect(0, 0, 84, 36, 10)
								roundBox.endFill();
								roundBox.x = 48;
								roundBox.y = 190;
								stage.addChild(roundBox);
				
								// 画线段
								let line = new PIXI.Graphics();
								line.lineStyle(4, 0xFFFFFF, 1);
								line.moveTo(0, 0);
								line.lineTo(80, 50);
								line.x = 32;
								line.y = 32;
								stage.addChild(line);
								// 画多边形
								let triangle = new PIXI.Graphics();
								triangle.beginFill(0x66FF33);
								triangle.drawPolygon([
										-32, 64,             //First point
										32, 64,              //Second point
										0, 0                 //Third point
								]);
								triangle.endFill();
								triangle.x = 180;
								triangle.y = 22;
								stage.addChild(triangle);
			
								renderer.render(stage);
							});
							function animate() {
								canvas.requestAnimationFrame(animate);
								renderer.render(stage);
							}
							animate();
						})
					})
		},
		methods: {
			touchEvent:function(e){
			console.log("触摸")
				//接收小程序的触摸事件传给PIXI
				PIXI.dispatchEvent(e);
			}
		},
		
	}
</script>

<style>
	#myCanvas{position:absolute;left:0;top:0;width:750rpx;height:1220rpx;}
	#canvs2d{position:absolute;left:-10000rpx;top:-10000rpx;width:16px;height:16px;}
</style>
