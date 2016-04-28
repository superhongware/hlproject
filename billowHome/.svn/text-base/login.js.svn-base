var Page = {
	init: function() {
		this.vue = E.vue(this.vueObj), this.initConfig()
	},
	initConfig: function() {
		var self = this;
		mui.init({
			"statusBarBackground": "#1F71B4",
			preloadPages: [{
				"id": "pages/tab/tab.html",
				"url": "pages/tab/tab.html",
			}]
		});
		mui.plusReady(function() {
			self.phoneType = plus.os.name.toLowerCase(),
				self.ws = plus.webview.currentWebview();
			self.indexPage = E.getWebview(E.preloadPages[2])
			self.cid = self.phoneType == "ios" ? encodeURIComponent(plus.push.getClientInfo().token) : encodeURIComponent(plus.push.getClientInfo().clientid)
			plus.device.vendor != 'CipherLab' ? E.setStorage("imei", "1") : E.setStorage("imei", "0");
			var rh = plus.screen.resolutionHeight
			var toGuide = document.getElementsByClassName("toGuide")[0]
			toGuide.style.top = (rh - 70) + "px";
		});
	},
	vueObj: {
		el: '#login',
		data: {
			orgCode: '426',
			store: '宏巍漕河泾分店',
			userName: 'hongware',
			password: 'admin123',
			showBer: false
//							orgCode: '8888',
//							store: '上海仓库',
//							userName: 'fsz',
//							password: '123456',
//							showBer: false
		},
		methods: {
			loginEvent: function() {
				if (!this.orgCode || !this.store || !this.userName || !this.password) {
					E.toast("信息不全！");
					return
				}
				var params = E.paramFn("V5.mobile.user.login")
				params = mui.extend(params, {
					orgCode: this.orgCode,
					store: this.store,
					userName: this.userName,
					password: this.password,
					phoneType: Page.phoneType,
					appId: plus.runtime.appid,
					cid: Page.cid
				})
				E.showLoading();
				var self = this;
				E.getData('userLogin', params, function(data) {
					if (!data.isSuccess) {
						E.alert(data.map.errorMsg), E.closeLoading()
						return
					}
					E.setStorage("orgCode", self.orgCode);
					E.setStorage("store", self.store);
					E.setStorage("op", data.op);
					self.getStoreList(function() {
						E.openWindow(E.preloadPages[2])
						E.getWebview(E.preloadPages[2]).evalJS("Page.loadChild()")
						plus.navigator.setStatusBarBackground("#007aff");
					})
				}, "get", this.errorFN())
			},
			getStoreList: function(callback) {
				var params = E.systemParam("V5.mobile.allocate.warehouse.search");
				var self = this;
				E.getData('warehouseSearch', params, function(data) {
					if (!data.isSuccess) {
						E.alert(data.map.errorMsg), E.closeLoading()
						return
					}
					data = data.stores;
					var status = 0
					for (var i = 0; i < data.length; i++) {
						if (data[i].storeName == self.store) {
							E.setStorage("myAddress", data[i].address);
							callback()
							status = 1;
							break;
						}
					}
					if (!status) {
						E.alert("门店列表中不存在此门店");
						E.closeLoading()
					}
				}, 'get')
			},
			errorFN: function() {

			},
			showBanner: function() {
				this.showBer = true
			},
			closeBanner: function() {
				this.showBer = false
			},
			tapTel: function() {
				plus.nativeUI.confirm("是否拨打电话？", function(e) {

					if (e.index == 0) {
						plus.device.dial("4007285858");
					} else {
						return;
					}
				}, "", ["确认", "取消"]);
			}
		}
	}
}
Page.init();