<template>
  <view class="content">
    <view>
      <view class="uni-padding-wrap" style="height: 400upx;">
        <view class="page-section swiper" >
          <view class="page-section-spacing">
            <swiper
              class="swiper"
              :indicator-dots="indicatorDots"
              :autoplay="autoplay"
              :interval="interval"
              :duration="duration"
			  style="height: 380upx;"
            >
              <swiper-item>
                <!-- <view class="swiper-item uni-bg-red">A</view> -->
				<image style="width: 100%; background-color: #eeeeee;" mode="aspectFill" src="../../static/images/ad1.jpg"
                        @error="imageError"></image>
              </swiper-item>
              <swiper-item>
                <!-- <view class="swiper-item uni-bg-green">B</view> -->
				<image style="width: 100%; background-color: #eeeeee;" mode="aspectFill" src="../../static/images/ad2.jpg"
                        @error="imageError"></image>
              </swiper-item>
              <swiper-item>
                <!-- <view class="swiper-item uni-bg-blue">C</view> -->
				<image style="width: 100%; background-color: #eeeeee;" mode="aspectFill" src="../../static/images/ad3.jpg"
                        @error="imageError"></image>
              </swiper-item>
            </swiper>
          </view>
        </view>
      </view>
    </view>
	<view>
	</view>
    <view>
      <!-- 图文卡片模式 -->
      <uni-card
        is-full="true"
        title="标题文字"
        mode="style"
        :is-shadow="true"
        thumbnail="https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/cbd.jpg"
        extra="Dcloud 2019-05-20 12:32:19"
        note="Tips"
      >
        那是一个秋意盎然、金风送爽的日子，我和父母一起来到了位于上师大旁的康健园。一踏进公园，一股浓郁的桂香扑鼻而来，泌人心脾,让我心旷神怡，只见一朵朵开得正烈的金色桂花，迎风而立，仿佛在向我招手。我们追着这桂香，走进了清幽的公园。
      </uni-card>
    </view>
  </view>
</template>

<script>
import { mapState, mapMutations } from "vuex";

import { uniBadge, uniCard } from "@dcloudio/uni-ui";
//import uniBadge from '@dcloudio/uni-ui/lib/uni-badge/uni-badge.vue' //也可使用此方式引入组件

export default {
  computed: mapState(["forcedLogin", "hasLogin", "userName"]),
  components: { uniBadge, uniCard },
  data() {
    return {
      background: ["red", "green", "blue"],
      indicatorDots: true,
      autoplay: true,
      interval: 2000,
      duration: 500,
    };
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

<style></style>
