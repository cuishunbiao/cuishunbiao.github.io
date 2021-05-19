<div class="header-right-sub" ref="headerRightSubBox">
    <div class="header-right-close" @click.prevent="showModelFn"><span class="header-right-close-icon"></span></div>
    <ul class="header-right-sub-box" style="display:block" v-if="isShowModel">
        <li>111</li>
        <li>222</li>
        <li @click="logoutFn">退出</li>
    </ul>
</div>

const isShowModel = ref(false)// 是否显示 弹出窗
const isContains = ref(true)// 是否是后代节点
const headerRightSubBox = ref<null | HTMLElement>(null)// 声明 DOM 元素

// 显示和隐藏登录窗
const showModelFn = () => {
    isShowModel.value = !isShowModel.value
}

// 判断是否是后代节点方法
const handler = (e: MouseEvent) => {
    if (headerRightSubBox.value) {
        // 是否包含在自定义的 DOM 中
        if (headerRightSubBox.value.contains(e.target as HTMLElement)) {
            isContains.value = true
        } else {
            isContains.value = false
        }
    }
}

// 监听数据变化
watch(isContains, () => {
    // 当弹窗在显示的时候，并且点击的区域不在自定义节点的后代
    if (isShowModel.value && !isContains.value) {
        isShowModel.value = false
    }
})

onMounted(() => {
    document.addEventListener('click', handler)
})
onUnmounted(() => {
    document.removeEventListener('click', handler)
})
