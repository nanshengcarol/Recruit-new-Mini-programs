<template>
  <view class="wholepage">
    <view class="nav">
      <view
        v-for="item in userDirectionStore.list"
        :key="item.id"
        class="direction"
        @click="directionDetail"
      >
        <view
          class="navDirection"
          @click="userProcedureStore.chooseDirection = item.id"
          :class="{ activeDirection: item.id === userProcedureStore.chooseDirection }"
        >
          {{ item.name }}
        </view>
      </view>
    </view>

    <view class="content" v-if="userProcedureStore.directionTrue">
      <view class="content_box" :class="colorArr[userProcedureStore.interview]">
        <view class="content_desc_1">面试</view>
        <view class="content_desc_2">({{ nameArr[userProcedureStore.interview] }})</view>
      </view>
      <view class="icon_box">
        <view class="square" :class="colorArr[userProcedureStore.interview]"></view>
        <view class="line" :class="colorArr[userProcedureStore.interview]"></view>
        <view class="arrow">
          <up-icon name="arrow-down" color="#9773FF" size="14" bold="true"></up-icon>
        </view>
      </view>

      <view class="content_box content_box_2" :class="colorArr[userProcedureStore.statusOne]">
        <view class="content_desc_1">一轮考核</view>
        <view class="content_desc_2">({{ nameArr[userProcedureStore.statusOne] }})</view>
      </view>
      <view class="icon_box icon_box_2">
        <view class="square" :class="colorArr[userProcedureStore.statusOne]"></view>
        <view class="line line_2" :class="colorArr[userProcedureStore.statusOne]"></view>
        <view class="arrow">
          <up-icon name="arrow-down" color="#9773FF" size="14" bold="true"></up-icon>
        </view>
      </view>

      <view class="content_box content_box_3" :class="colorArr[userProcedureStore.statusTwo]">
        <view class="content_desc_1">二轮考核</view>
        <view class="content_desc_2">({{ nameArr[userProcedureStore.statusTwo] }})</view>
      </view>
      <view class="icon_box icon_box_3" v-if="userProcedureStore.ui">
        <view class="square" :class="colorArr[userProcedureStore.statusTwo]"></view>
        <view class="line" :class="colorArr[userProcedureStore.statusTwo]"></view>
        <view class="arrow">
          <up-icon name="arrow-down" color="#9773FF" size="14" bold="true"></up-icon>
        </view>
      </view>

      <view
        class="content_box content_box_4"
        :class="colorArr[userProcedureStore.success]"
        v-if="userProcedureStore.ui"
      >
        <view class="content_desc_1">三轮</view>
        <view class="content_desc_2">({{ nameArr[userProcedureStore.success] }})</view>
      </view>
    </view>
  </view>
</template>
<script setup lang="ts">
import { ref, reactive } from 'vue'

import { getProcedure } from '@/api/procedure'
import { onLoad } from '@dcloudio/uni-app'
import { useDirectionStore } from '@/stores/modules/reservation'
import { useUserDetailStore } from '@/stores/modules/registration'
import { useProcedureStore } from '@/stores/modules/procedure'
const userDetailStore = useUserDetailStore()
const userDirectionStore = useDirectionStore()
const userProcedureStore = useProcedureStore()
// const colorArr = ['will', 'ongoing', 'finish']
// const nameArr = ['未开始', '进行中', '已完成']
const colorArr = ['will', 'ongoing', 'pass', 'fail']
const nameArr = ['未开始', '进行中', '已通过', '未通过']

const directionDetail = () => {
  for (const item of userDetailStore.directionNum) {
    userProcedureStore.getDirectionDetail()
  }
}

onLoad(async () => {
  userDirectionStore.getDirectionName()
  if (userDetailStore.directionNum.length === 0) {
    userProcedureStore.directionTrue = false
  } else {
    userProcedureStore.directionTrue = true
  }
  for (const item of userDetailStore.directionNum) {
    userProcedureStore.chooseDirection = item + 1
    if (item == 3) {
      userProcedureStore.ui = true
    } else {
      userProcedureStore.ui = false
    }
    const res = await getProcedure(item)
    userProcedureStore.getDirectionStatus(item, res)
    return
  }
})
</script>

<style lang="scss" scoped>
.wholepage {
  background-color: #f8f7ff;
  height: 100vh;
}
::v-deep .u-subsection {
  height: 128rpx !important;
}
::v-deep .u-subsection__item {
  border: none !important;
}
::v-deep .u-subsection__bar {
  margin-top: 48rpx;
  width: 144rpx !important;
  margin-left: 4rpx;
}
::v-deep .u-subsection__bar--center.data-v-7b2e14a2 {
  border-radius: 50rpx;
}
::v-deep .u-subsection__bar--first.data-v-7b2e14a2 {
  border-radius: 50rpx;
}
::v-deep .u-subsection__bar--last.data-v-7b2e14a2 {
  border-radius: 50rpx;
}
::v-deep .u-subsection__item__text {
  font-size: 32rpx !important;
  font-weight: 500;
  color: #1a1a1a;
}
::v-deep .u-subsection__item {
  width: 156rpx;
  height: 64rpx;
  margin-top: 48rpx;
}
.content {
  margin-top: 80rpx;
}
.content_box {
  width: 254rpx;
  height: 166rpx;
  margin: auto;
  border-radius: 20rpx;
  background-color: #9773ff;
  display: flex;
  flex-direction: column;
  gap: 16rpx;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  font-size: 32rpx;
}
.content_box_2 {
  background-color: #9773ff;
  transform: translate(0, -16rpx);
}
.content_box_3,
.content_box_4 {
  background-color: #9773ff;
  transform: translate(0, -16rpx);
}
.content_box_4 {
  transform: translate(0, -30rpx);
}
.icon_box_2,
.icon_box_3 {
  transform: translate(0, -16rpx);
}
.square {
  width: 16rpx;
  height: 16rpx;
  background-color: #9773ff;
  margin: auto;
  transform: rotate(45deg);
}
.line {
  height: 104rpx;
  width: 6rpx;
  background-color: #9773ff;
  margin: auto;
}

::v-deep .u-icon__icon.data-v-1c933a9a {
  margin: auto;
  transform: translate(0, -14rpx);
}

::v-deep .icon_box_2.data-v-a718f74a {
  transform: translate(0, -11rpx);
  height: 134rpx;
}

.nav {
  display: flex;
  align-items: center;
  justify-content: center;
}

.navDirection {
  margin-top: 36rpx;
  width: 144rpx;
  height: 70rpx;
  color: #1a1a1a;
  font-size: 16px;
  font-weight: 500;
  border-radius: 16px;
  background-color: transparent;
  border: none !important;
  cursor: pointer;
  outline: none !important;
  display: flex;
  align-items: center;
  justify-content: center;
}
.navDirection:focus {
  outline: none !important; /* 去除按钮获取焦点时的默认边框 */
}
.activeDirection {
  background-color: #7f52ff;
  color: #ffffff !important;
  border-radius: 24px;
  font-weight: 700;
}
.pass {
  background-color: #aeb4c2;
}
.ongoing {
  background-color: #7f52ff;
}
.will {
  background-color: #9773ff;
}
.fail {
  background-color: #9773ff;
}
</style>
