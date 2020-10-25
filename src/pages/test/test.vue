<template>
  <view class="content">
    <view v-if="hasLogin" class="hello">
      <view class="title"> 您好 {{ userName }}，您已成功登录。 </view>
      <view class="ul">
        <view>这是 uni-app 带登录模板的示例App首页。</view>
        <view>在 “我的” 中点击 “退出” 可以 “注销当前账户”</view>
      </view>
    </view>
    <view v-if="!hasLogin" class="hello">
      <view class="title"> 您好 游客。 </view>
      <view class="ul">
        <view>这是 uni-app shop。</view>
        <view>shop list”</view>
      </view>
      <view>
        <uni-badge text="1"></uni-badge>
        <uni-badge text="2" type="success" @click="bindClick"></uni-badge>
        <uni-badge text="3" type="primary" :inverted="true"></uni-badge>
      </view>
      <view>
        <button
          class="cu-btn bg-red margin-tb-sm lg"
          role="button"
          aria-disabled="false"
        >
          嫣红
        </button>
      </view>
      <view>
        <view class="cu-bar tabbar bg-white">
          <view class="action">
            <view class="cuIcon-cu-image">
              <!-- <image src="/static/tabbar/basics_cur.png"></image> -->
              <text class="lg text-gray cuIcon-my"></text>
            </view>
            <view class="text-green">元素</view>
          </view>
          <view class="action">
            <view class="cuIcon-cu-image">
              <!-- <image src="/static/tabbar/component.png"></image> -->
              <text class="lg text-gray cuIcon-myfill"></text>
            </view>
            <view class="text-gray">组件</view>
          </view>
          <view class="action">
            <view class="cuIcon-cu-image">
              <!-- <image src="/static/tabbar/plugin.png"></image> -->
              <text class="lg text-gray cuIcon-emoji"></text>
              <view class="cu-tag badge">99</view>
            </view>
            <view class="text-gray">扩展</view>
          </view>
          <view class="action">
            <view class="cuIcon-cu-image">
              <!-- <image src="/static/tabbar/about.png"></image> -->
              <text class="lg text-gray cuIcon-favor"></text>
              <view class="cu-tag badge"></view>
            </view>
            <view class="text-gray">关于</view>
          </view>
        </view>
      </view>
    </view>
    <!-- 单行内容显示 -->
    <uni-list>
      <uni-list-item title="列表文字"></uni-list-item>
      <uni-list-item :disabled="true" title="列表禁用状态"></uni-list-item>
    </uni-list>
    <!-- 多行内容显示 -->
    <uni-list>
      <uni-list-item title="列表文字" note="列表描述信息"></uni-list-item>
      <uni-list-item
        :disabled="true"
        title="列表文字"
        note="列表禁用状态"
      ></uni-list-item>
    </uni-list>
    <!-- 右侧显示角标、switch -->
    <uni-list>
      <uni-list-item
        title="列表右侧显示角标"
        :show-badge="true"
        badge-text="12"
      ></uni-list-item>
      <uni-list-item
        title="列表右侧显示 switch"
        :show-switch="true"
        @switchChange="switchChange"
      ></uni-list-item>
    </uni-list>
    <!-- 左侧显示略缩图、图标 -->
    <uni-list>
      <uni-list-item
        title="列表左侧带略缩图"
        note="列表描述信息"
        thumb="https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png"
        thumb-size="lg"
        rightText="右侧文字"
      ></uni-list-item>
      <uni-list-item
        :show-extra-icon="true"
        :extra-icon="extraIcon1"
        title="列表左侧带扩展图标"
      ></uni-list-item>
    </uni-list>

    <!-- 一般用法 -->
    <uni-collapse @change="change">
      <uni-collapse-item title="标题文字">
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
      <uni-collapse-item title="默认开启" open="true">
        <view style="padding: 30rpx"> 折叠内容主体，可自定义内容及样式 </view>
      </uni-collapse-item>
      <uni-collapse-item title="禁用状态" disabled="true">
        <view style="padding: 30rpx"> 禁用状态 </view>
      </uni-collapse-item>
    </uni-collapse>

    <!-- 手风琴效果 -->
    <uni-collapse accordion="true">
      <uni-collapse-item title="标题文字">
        <view style="padding: 30rpx"> 手风琴效果 </view>
      </uni-collapse-item>
      <uni-collapse-item title="标题文字">
        <view style="padding: 30rpx"> 手风琴效果 </view>
      </uni-collapse-item>
      <uni-collapse-item title="标题文字">
        <view style="padding: 30rpx"> 手风琴效果 </view>
      </uni-collapse-item>
    </uni-collapse>

    <!-- 带图标 -->
    <uni-collapse>
      <uni-collapse-item
        title="标题文字"
        thumb="https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png"
      >
        <view style="padding: 30rpx"> 折叠内容主体，可自定义内容及样式 </view>
      </uni-collapse-item>
      <uni-collapse-item
        title="标题文字"
        thumb="https://img-cdn-qiniu.dcloud.net.cn/new-page/hx.png"
      >
        <view style="padding: 30rpx"> 折叠内容主体，可自定义内容及样式 </view>
      </uni-collapse-item>
    </uni-collapse>

    <view>
      <button
        class="sys_btn"
        open-type="getUserInfo"
        lang="zh_CN"
        @getuserinfo="appLoginWx"
      >
        小程序登录授权
      </button>
    </view>
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
    appLoginWx() {
      // #ifdef MP-WEIXIN
      uni.getProvider({
        service: "oauth",
        success: function (res) {
          if (~res.provider.indexOf("weixin")) {
            uni.login({
              provider: "weixin",
              success: (res2) => {
                uni.getUserInfo({
                  provider: "weixin",
                  success: (info) => {
                    //这里请求接口
                    console.log(res2);
                    console.log(info);
                  },
                  fail: () => {
                    uni.showToast({ title: "微信登录授权失败", icon: "none" });
                  },
                });
              },
              fail: () => {
                uni.showToast({ title: "微信登录授权失败", icon: "none" });
              },
            });
          } else {
            uni.showToast({
              title: "请先安装微信或升级版本",
              icon: "none",
            });
          }
        },
      });
      //#endif
    },
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
