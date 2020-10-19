---
title: pay.js
date: 2020-10-19 14:21:45
tags: js
---
# 关于微信小程序的支付封装
```
import util from './util.js';
import md5 from './md5.js';
import api from './https.js';
const callBack='';//自定义支付回调函数,用于返回订单页面监听是否支付成功
const mallTag='';// 1客房速达 2心选商城
module.exports={
	/**
	 * 心选商城订单支付轮询结果
	 * @param {Object} data 订单信息
	 * @param {Object} queryCount 轮询次数
	 */
	featedQueryOrder(data, queryCount) {
		var queryCount = queryCount;
		var that = this;
		var bisTransOrderVo = {
			"orderNbr": data.orderNbr,
			mallTag:'2'
		};
		api.featedQueryOrder(bisTransOrderVo).then(queryData => {
			if (queryCount > 5) {
				uni.showToast({
					title: '查询支付结果失败',
					icon: 'none'
				})
				that.callBack(false)
				return
			} else {
				if (queryData.success) {
					queryCount = 0;
					uni.hideLoading()
					uni.showToast({
						title: '支付成功'
					})
					that.callBack(true)
				} else {
					queryCount++;
					that.featedQueryOrder(data, queryCount)
				}
			}
		})
	},
	/**
	 * 客房速达订单支付轮询结果
	 * @param {Object} data 订单信息
	 * @param {Object} queryCount 轮询次数
	 */
	queryOrder(data, queryCount) {
		var queryCount = queryCount;
		var bisTransOrderVo = {
			"orderNbr": data.orderNbr,
		};
		api.queryOrderResult(bisTransOrderVo).then(queryData => {
			if (queryCount > 5) {
				uni.showToast({
					title: '查询支付结果失败',
					icon: 'none'
				})
				this.callBack(false)
				return false
			} else {
				if (queryData.success) {
					queryCount = 0;
					uni.showToast({
						title: '支付成功'
					})
					this.callBack(true)
					return true
				} else {
					queryCount++;
					this.queryOrder(data, queryCount)
					return false
				}
			}
		})
	},
	/**
	 * 发起微信支付
	 * @param {Object} res 下单成功的返回值
	 */
	pay(res) {
		var that = this;
		var data = res.data;
		var timestamp = Date.parse(new Date());
		timestamp = timestamp / 1000;
		var nonceStr = util.randomString(32).toUpperCase();
		var sign =
			'appId=wxc2f5ba7e9a8af767&nonceStr=' + nonceStr +
			'&package=prepay_id=' + data.prepayId +
			'&signType=MD5&timeStamp=' + timestamp +
			'&key=' + data.key;
		uni.requestPayment({
			//发起支付
			appid: 'wxc2f5ba7e9a8af767',
			nonceStr: nonceStr,
			package: 'prepay_id=' + data.prepayId,
			paySign: md5.hexMD5(sign).toUpperCase(),
			timeStamp: timestamp.toString(),
			signType: 'MD5',
			total_fee: data.totalFee,
			success: function(res) {
				//支付成功回调
				let queryCount = 0;
				if(that.mallTag=='2'){
					that.featedQueryOrder(data,queryCount)
				}else{
					that.queryOrder(data, queryCount)
				}
			},
			fail: function(res) {
				// let queryCount = 0;
				// that.queryOrder(data, queryCount)
				uni.showToast({
					title: '支付失败',
					icon: 'none'
				})
				console.log('1')
				that.callBack(false)
			}
		})
	},
	/**
	 * 客房速达下单、创建订单
	 * @param {Object} data 下单参数
	 * @param String  mallTag 1客房速达 2心选商城
	 */
	placeOrder(data,fn,mallTag) {
		var that = this;
		that.callBack=fn;
		that.mallTag=mallTag;
		uni.showLoading({
			title: '支付中...',
			mask:true
		})
		api.mallPlaceOrder(data).then(res => {
			uni.hideLoading()
			if (res.success) {
				that.pay(res)
			} else {
				uni.showToast({
					title: res.message,
					icon: 'none'
				})
				that.callBack(false)
			}
		})
	},
	/**
	 * 心选商城下单、创建订单
	 * @param {Object} data 下单参数
	 * @param String  mallTag 1客房速达 2心选商城
	 */
	featedPlaceOrder(data,fn,mallTag) {
		var that = this;
		that.callBack=fn;
		that.mallTag=mallTag;
		uni.showLoading({
			title: '支付中...',
			mask:true
		})
		api.featedPlaceOrder(data).then(res => {
			uni.hideLoading()
			if (res.success) {
				that.pay(res)
			} else {
				uni.showToast({
					title: res.message,
					icon: 'none'
				})
				that.callBack(false)
			}
		})
	},
	
	/**
	 * 再次发起支付
	 * @param {data}  下单参数
	 */
	agianPay:function(data,fn,mallTag){
		var that = this;
		that.callBack=fn;
		that.mallTag=mallTag;
		uni.showLoading({
			title: '支付中...',
			mask:true
		})
		api.againPay(data).then(res => {
			uni.hideLoading()
			if (res.success) {
				that.pay(res)
			} else {
				uni.showToast({
					title: res.message,
					icon: 'none'
				})
				that.callBack(false)
			}
		})
	}
}

```
