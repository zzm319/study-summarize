<template>
    <view class="content">
        <view class="code-area">
            <view class="propmt">已发送4位验证码至 +86 {{ phoneNum }}</view>
            <view class="code-input" @click="handleFocus">
                <input v-for="(item, index) in inputbox" type="number" :key="index" v-model="item.labelValue"
                    class="input-code" :maxlength="index === 0 && isMaxLength ? 4 : 1" @input="onInput($event, index)" />
            </view>
        </view>
    </view>
</template>
  
<script setup>
import { ref, reactive, watch, onBeforeUnmount, nextTick, onMounted, computed } from 'vue';

let inputbox = reactive(new Array(4).fill().map((item, index) => ({ id: index, labelValue: '' })));
let phoneNum = ref('');
let isMaxLength = ref(true);

// 四位短信验证码
const inputCodeValue = computed(() => inputbox.reduce((pre, item) => pre + item.labelValue, ''))

// 验证码的长度
const focusIndex = computed(() => inputCodeValue.value.length)

// 监听键盘上的删除按键(Backspace)
const handleListenDelete = (e) => {
    if (e.keyCode === 8 && focusIndex.value > 0) {
        inputbox[focusIndex.value - 1].labelValue = '';
        document.querySelectorAll('.uni-input-input')[focusIndex.value].focus();
    }
}


onMounted(() => {
    // #ifdef H5
    // 处理聚焦第一个输入框
    document.querySelectorAll('.uni-input-input')[0].focus()

    document.addEventListener('keydown', handleListenDelete)

    // 监听输入区域的粘贴事件
    document.querySelector('.code-input').addEventListener('paste', (event) => {
        const pasteText = (event.clipboardData || window.clipboardData).getData("text");
        const arr = pasteText.split('').filter(item => /\d/.test(Number(item)));
        const newArr = arr.slice(0, 4).map(item => Number(item));
        if (newArr.length) {
            inputbox[0].labelValue = newArr.join('');
        }
    })
    // #endif
})

// 监听第一个输入框的值：为了处理粘贴和短信自动填充时赋值和聚焦
watch(() => inputbox[0].labelValue, (val) => {
    if (val) {
        // 处理输入的时候限制输入长度
        isMaxLength.value = false;
    }
    nextTick(() => {
        if (val && val.length >= 2) {
            val.split('').forEach((element, index) => {
                inputbox[index].labelValue = element;
            });
        }

        setTimeout(() => {
            // 加个定时器 避免粘贴两次
            handleFocus();
        })
    })
})

watch(() => inputCodeValue.value, async (val) => {
    if (!val) {
        // 处理四位输入框为空时再次复制粘贴
        isMaxLength.value = true;
    }

    if (val.length === 4) {
        // 四位输入框的值已填满 做登录等逻辑操作
        console.log('to login')
    }
})

// 点击输入区域 自动聚焦到空的输入框
const handleFocus = () => {
    if (focusIndex.value === 4) {
        document.querySelectorAll('.uni-input-input')[3].focus();
        return;
    }
    document.querySelectorAll('.uni-input-input')[focusIndex.value].focus();
}

// 监听输入输入框 自动聚焦到下一个输入框
const onInput = (e, index) => {
    // index < 3 ，如果是第4格，不触发光标移动至下一个输入框。
    if (inputbox[index].labelValue && index < 3) {
        nextTick(() => {
            document.querySelectorAll('.uni-input-input')[index + 1].focus()
        })
    }
}


onBeforeUnmount(() => {
    document.removeEventListener('keydown', handleListenDelete);
})

</script>
  
  
<style lang="less" scoped>
@import url('./index.less');
</style>
  