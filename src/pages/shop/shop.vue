<template>
  <view class="content">
 
    <!-- 左侧显示略缩图、图标 -->
    <uni-list>
      <uni-list-item
        title="闵行区车商"
        note="在闵行区解放路路口"
		thumb="https://ftp.bmp.ovh/imgs/2020/10/052febbf70e8aa67.png"
		thumbSize="lg"
		showExtraIcon="false"
        extraIcon="{color: '#4cd964',size: '22',type: 'spinner'}"
        rightText="地址详情"
		link to="/pages/shop/shopmap/shopmap" 
		@click="onClick($event,1)"
      ></uni-list-item>
	  <uni-list-item
        title="浦东新区车商"
        note="在闵行区解放路路口"
		thumb="https://ftp.bmp.ovh/imgs/2020/10/052febbf70e8aa67.png"
		thumbSize="lg"
		showExtraIcon="false"
        extraIcon="{color: '#4cd964',size: '22',type: 'spinner'}"
        rightText="地址详情 >"
      ></uni-list-item>
	  <uni-list-item
        title="黄埔区车商"
        note="在闵行区解放路路口"
		thumb="https://ftp.bmp.ovh/imgs/2020/10/052febbf70e8aa67.png"
		thumbSize="lg"
		showExtraIcon="false"
        extraIcon="{color: '#4cd964',size: '22',type: 'spinner'}"
        rightText="地址详情 >"
      ></uni-list-item>
	  <uni-list-item
        title="奉贤区车商"
        note="在闵行区解放路路口"
		thumb="https://ftp.bmp.ovh/imgs/2020/10/052febbf70e8aa67.png"
		thumbSize="lg"
		showExtraIcon="false"
        extraIcon="{color: '#4cd964',size: '22',type: 'spinner'}"
        rightText="地址详情 >"
      ></uni-list-item>
	  <uni-list-item
        title="青浦区车商"
        note="在闵行区解放路路口"
		thumb="https://ftp.bmp.ovh/imgs/2020/10/052febbf70e8aa67.png"
		thumbSize="lg"
		showExtraIcon="false"
        extraIcon="{color: '#4cd964',size: '22',type: 'spinner'}"
        rightText="地址详情 >"
      ></uni-list-item>
	  <uni-list-item
        title="静安区车商"
        note="在闵行区解放路路口"
		thumb="https://ftp.bmp.ovh/imgs/2020/10/052febbf70e8aa67.png"
		thumbSize="lg"
		showExtraIcon="false"
        extraIcon="{color: '#4cd964',size: '22',type: 'spinner'}"
        rightText="地址详情 >"
      ></uni-list-item>
    </uni-list>
  </view>
</template>

<script>
import { mapState, mapMutations } from "vuex";

import { uniIcons, uniList, uniListItem, uniListChat } from "@dcloudio/uni-ui";
//import uniBadge from '@dcloudio/uni-ui/lib/uni-badge/uni-badge.vue' //也可使用此方式引入组件

export default {
  computed: mapState(["forcedLogin", "hasLogin", "userName"]),
  components: { uniIcons, uniList, uniListItem, uniListChat },
  onLoad() {
    const loginType = uni.getStorageSync("login_type");
    if (loginType === "local") {
      this.login(uni.getStorageSync("username"));
      return;
    }
    let uniIdToken = uni.getStorageSync("uniIdToken");
    if (uniIdToken) {
      this.login(uni.getStorageSync("username"));
      uniCloud.callFunction({
        name: "user-center",
        data: {
          action: "checkToken",
        },
        success: (e) => {
          console.log("checkToken success", e);

          if (e.result.code > 0) {
            //token过期或token不合法，重新登录
            if (this.forcedLogin) {
              uni.reLaunch({
                url: "../login/login",
              });
            } else {
              uni.navigateTo({
                url: "../login/login",
              });
            }
          }
        },
        fail(e) {
          uni.showModal({
            content: JSON.stringify(e),
            showCancel: false,
          });
        },
      });
    } else {
      this.guideToLogin();
    }
  },
  methods: {
    ...mapMutations(["login"]),
    guideToLogin() {
      uni.showModal({
        title: "未登录",
        content: "您未登录，需要登录后才能继续",
        /**
         * 如果需要强制登录，不显示取消按钮
         */
        showCancel: !this.forcedLogin,
        success: (res) => {
          if (res.confirm) {
            /**
             * 如果需要强制登录，使用reLaunch方式
             */
            if (this.forcedLogin) {
              uni.reLaunch({
                url: "../login/login",
              });
            } else {
              uni.navigateTo({
                url: "../login/login",
              });
            }
          }
        },
      });
	},
	onClick(event, num){
		console.log(event)
		console.log(num)
	}
  },
};
</script>

<style>
</style>
