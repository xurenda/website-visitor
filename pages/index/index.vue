<template>
<view class="content">
  <view class="input">
    <picker
      :range="protocolRange"
      :value="protocolIndex"
      @change="pickerChange($event)"
      class="picker"
      mode="selector"
    >
      {{ protocol }}
    </picker>
    <input
      v-model="url"
      @confirm="goto()"
      @blur="blur()"
      focus
      class="text-input"
      type="text"
      placeholder="请输入网址"
      placeholder-class="placeholder"
      confirm-type="done"
    />
  </view>
  <view class="bar">
    <text>输入历史</text>
    <text class="clear" @click="clear()">清空</text>
  </view>
  <uni-list>
    <uni-list-item
      v-for="url in urls"
      @click="goto(url)"
      :title="url"
      :key="url"
    />
  </uni-list>
</view>
</template>

<script>
import uniList from "@/components/uni-list/uni-list.vue"
import uniListItem from "@/components/uni-list-item/uni-list-item.vue"
  
const reg = /^(http|https):\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&:/~\+#]*[\w\-\@?^=%&/~\+#])?$/

export default {
  components: { uniList, uniListItem },
  data() {
    return {
      url: '',
      urls: [],
      protocolRange: ['http://', 'https://'],
      protocolIndex: 0
    }
  },
  computed: {
    protocol() {
      return this.protocolRange[this.protocolIndex]
    }
  },
  methods: {
    pickerChange(event) {
      this.protocolIndex = event.detail.value
    },
    goto(url) {
      if (!url) {
        url = this.protocol + this.url.trim()
      }
      if (!reg.test(url)) {
        uni.showToast({ title: '请正确输入网址', icon: 'none' })
        return
      }
      if (this.urls.indexOf(url) === -1) {
        this.urls.unshift(url)
        uni.setStorageSync('urls', this.urls)
      }
      uni.navigateTo({
          url: `/pages/web/web?url=${url}`
      })
    },
    clear() {
      this.urls = []
      uni.clearStorageSync()
    },
    blur() {
      this.url = ''
    }
  },
  onLoad() {
    const urls = uni.getStorageSync('urls')
    if (urls) this.urls = urls
  }
}
</script>

<style>
.content {
  width: 100vw;
  height: 100vh;
  background-color: #F8F8F8;
  padding: 0 10upx;
  box-sizing: border-box;
}
.input {
  display: flex;
  align-items: center;
  margin: 20upx;
}
.picker {
  border: 1upx solid #CCCCCC;
  border-radius: 100upx;
  font-size: .8em;
  background-color: #FFFFFF;
  color: #007AFF;
  padding: 10upx 20upx;
}
.text-input {
  flex: 1;
  border: 1px solid #CCCCCC;
  padding: 10upx 30upx;
  margin-left: 10upx;
  border-radius: 100upx;
}
.placeholder {
  color: #666666;
  font-size: .8em;
  font-weight: 300;
}
.bar {
  display: flex;
  justify-content: space-between;
  margin: 10upx 20upx 20upx;
  font-size: 30upx;
  color: #555555;
}
.clear {
  text-decoration: underline;
  color: #007AFF;
}
</style>
