<template>
<view>
  <web-view :src="url"></web-view>
</view>
</template>

<script>
export default {
  data() {
    return {
      url: ''
    }
  },
  onLoad(option) {
    this.url = option.url
  },
  onNavigationBarButtonTap({ index }) {
    switch (index) {
    case 0:
      uni.reLaunch({
        url: '/pages/index/index'
      })
      break
    }
  }
}
</script>
