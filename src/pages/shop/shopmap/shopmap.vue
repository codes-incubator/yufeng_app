<template>
  <view class="content">
    <map
      style="width: 100%; height: 300px"
      :latitude="latitude"
      :longitude="longitude"
      :markers="covers"
    >
    </map>
  </view>
</template>

<script>
import { mapState, mapMutations } from "vuex";

import { uniBadge } from "@dcloudio/uni-ui";
//import uniBadge from '@dcloudio/uni-ui/lib/uni-badge/uni-badge.vue' //也可使用此方式引入组件

export default {
  computed: mapState(["forcedLogin", "hasLogin", "userName"]),
  components: { uniBadge },
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
  data() {
    return {
      id: 0, // 使用 marker点击事件 需要填写id
      title: "map",
      latitude: 39.909,
      longitude: 116.39742,
      covers: [
        {
          latitude: 39.909,
          longitude: 116.39742,
          iconPath: "../../../static/img/map_hl.png",
        },
        {
          latitude: 39.9,
          longitude: 116.39,
          iconPath: "../../../static/img/map_hl.png",
        },
      ],
    };
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
