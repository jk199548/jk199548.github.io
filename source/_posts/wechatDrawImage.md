<!--
 * @Author: your name
 * @Date: 2020-09-27 10:51:55
 * @LastEditTime: 2020-09-27 11:07:55
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \jk199548.github.io\source\_posts\微信小程序分享缩略图动态绘制.md
-->
---
title: 微信小程序分享缩略图动态绘制
date: 2020-09-27 10:51:55
tags:
---
## 微信小程序分享缩略图动态绘制
### 1、初始化生成主图，根据canvas.createImage()去生成图片对象
```
var that = this;
const query = wx.createSelectorQuery();
query.select('#mycanvas')
    .fields({
        node: true,
        size: true
    })
    .exec(async function(res) {
        const canvas = res[0].node;
        const ctx = canvas.getContext('2d');
        const dpr = uni.getSystemInfoSync().pixelRatio;
        canvas.width = res[0].width * dpr;
        canvas.height = res[0].height * dpr;
        console.log('canvas',canvas)
        ctx.scale(dpr,dpr)
        ctx.fillStyle = '#fff';
        ctx.fillRect(0, 0, 500, 400);
        //生成主图
        const mainImg = canvas.createImage();
        mainImg.src = that.prodDetail.prodImage;
        let mainImgPo = await new Promise((resolve, reject) => {
            mainImg.onload = () => {
                resolve(mainImg)
            }
            mainImg.onerror = (e) => {
                reject(e)
            }
        });
        ctx.drawImage(mainImgPo, 8, 57.5 ,267/2, 263/2);
        //生成购买按钮图片
        const buyImage = canvas.createImage();
        buyImage.src = '../../static/15.png';
        let buyImagePo = await new Promise((resolve, reject) => {
            buyImage.onload = () => {
                resolve(buyImage)
            }
            buyImage.onerror = (e) => {
                reject(e)
            }
        });
        ctx.drawImage(buyImagePo, 152, 154,180/2, 70/2);
        //填充文字
        //产品名称
        ctx.textBaseline = "top";
        ctx.textAlign = 'left';
        ctx.font = "17px bold"; //设置字体大小，默认10
        ctx.fillStyle = '#1E151E'; //文字颜色：默认黑色
        ctx.fillText(that.prodDetail.prodName, 8, 11)//绘制文本
        //惊喜价
        ctx.font = "16px bold"; //设置字体大小，默认10
        ctx.fillStyle = '#000000'; //文字颜色：默认黑色
        ctx.fillText('惊喜价:', 152, 82)//绘制文本
        //价格
        ctx.font = "23px bold"; //设置字体大小，默认10
        ctx.fillStyle = '#E0251B'; //文字颜色：默认黑色
        ctx.fillText('￥'+that.prodDetail.prodMarketPrice, 152, 120)//绘制文本
        setTimeout(() => {
            that.saveImg()
        }, 1000)
    })
```
### 2、将绘制完成的图片保存到本地
```
	async saveImg() {
        let self = this;
        const query = wx.createSelectorQuery();
        const canvasObj = await new Promise((resolve, reject) => {
            query.select('#mycanvas')
                .fields({
                    node: true,
                    size: true
                })
                .exec(async (res) => {
                    resolve(res[0].node);
                })
        });
        wx.canvasToTempFilePath({
            canvas: canvasObj, //现在的写法
            width: canvasObj.width,
            height: canvasObj.height,
            success: (res) => {
                console.log(res);
                //保存图片
                self.shareImage = res.tempFilePath
            },
            fail(res) {
                console.log(res);
            }
        }, this)
    },
```