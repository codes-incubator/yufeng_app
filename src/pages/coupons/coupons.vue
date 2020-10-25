<template>
  <view class="content">
     <uni-collapse accordion="true" @change="change">
      <uni-collapse-item title="未使用优惠券">
        <uni-list>
          <uni-list-item
            title="标题文字"
            thumb="https://img-cdn-qiniu.dcloud.net.cn/new-page/hx.png"
          ></uni-list-item>
          <uni-list-item
            title="标题文字"
            note="描述信息"
            thumb="https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png"
          ></uni-list-item>
          <uni-list-item
            title="标题文字"
            note="描述信息"
            show-extra-icon="true"
            :extra-icon="{ color: '#4cd964', size: '22', type: 'spinner' }"
          ></uni-list-item>
        </uni-list>
      </uni-collapse-item>
      <uni-collapse-item title="已失效优惠券">
        <uni-list>
          <uni-list-item
            title="标题文字"
            thumb="https://img-cdn-qiniu.dcloud.net.cn/new-page/hx.png"
          ></uni-list-item>
          <uni-list-item
            title="标题文字"
            note="描述信息"
            thumb="https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png"
          ></uni-list-item>
          <uni-list-item
            title="标题文字"
            note="描述信息"
            show-extra-icon="true"
            :extra-icon="{ color: '#4cd964', size: '22', type: 'spinner' }"
          ></uni-list-item>
        </uni-list>
      </uni-collapse-item>
    </uni-collapse>
  </view>
</template>

<script>
import { mapState, mapMutations } from "vuex";

import {
  uniBadge,
  uniList,
  uniListItem,
  uniListChat,
  uniCollapse,
  uniCollapseItem,
} from "@dcloudio/uni-ui";
//import uniBadge from '@dcloudio/uni-ui/lib/uni-badge/uni-badge.vue' //也可使用此方式引入组件

export default {
  computed: mapState(["forcedLogin", "hasLogin", "userName"]),
  components: {
    uniBadge,
    uniList,
    uniListItem,
    uniListChat,
    uniCollapse,
    uniCollapseItem,
  },
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
  },
};
</script>

<style>
</style>
