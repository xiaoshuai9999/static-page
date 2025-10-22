<script setup lang="ts">
import {computed, ref} from "vue";
import html2canvas from "html2canvas";

const fieldText = ref<string>('稀土 掘金')
const previewText = computed(() => {
  if (!fieldText.value) return ['', '']
  const splits = fieldText.value.trim().split(' ')
  const s0 = (splits[0] || '').trim()
  if (splits.length === 1) {
    const sp = splitString(s0)
    if (s0.length > 2 && isAllChinese(sp[0] || '')) return [s0.slice(0, 2), s0.slice(-2)]
    return [(sp[0] || '').slice(0, 9), (sp[1] ?? '').slice(-2)]
  }
  return [isAllLetters(s0) ? s0.slice(0, 9) : s0.slice(0, 2), (splits[1] || '').trim().slice(-2)]
})

const fieldChange = (val: string) => {
  console.log(val)
  console.log(pickedColor.value)
}

function splitString(str: string) {
  // 检查字符串是否以字母开头
  if (/^[a-zA-Z]/.test(str)) {
    // 匹配开头的字母部分和剩余部分
    const match = str.match(/^([a-zA-Z]+)(.*)$/);
    if (match) {
      return [match[1], match[2]];
    }
  }
  // 如果不是以字母开头，返回原始字符串和空字符串
  return [str, ''];
}

function isAllLetters(str: string) {
  return /^[a-z]+$/i.test(str);
}

function isAllChinese(str: string) {
  if (str.length === 0) return false;

  // 匹配中文字符的Unicode范围：\u4E00-\u9FFF
  return /^[\u4E00-\u9FFF]+$/.test(str);
}

const pickedColor = ref<string>('#2d71fb')
const DEFAULT_SIZE = '32px';
const previewSize = computed<string[]>(() => {
  let v0 = previewText.value[0] ?? ''
  if (!fieldText.value || !v0.length) return [DEFAULT_SIZE, DEFAULT_SIZE]
  if (fieldText.value.trim().length === 2) return ['36px', DEFAULT_SIZE]

  const v1 = previewText.value[1] ?? ''
  if (v1.length === 2) {
    if (v0.length === 2) {
      return [DEFAULT_SIZE, DEFAULT_SIZE]
    } else if (v0.length > 5) {
      return ['20px', DEFAULT_SIZE]
    } else {
      return ['28px', DEFAULT_SIZE]
    }
  }
  return [DEFAULT_SIZE, DEFAULT_SIZE]
})

const downloadPicture = () => {
  const target = document.querySelector("#final-icon") as HTMLElement
  html2canvas(target).then(canvas => {
    const link = document.createElement("a")
    link.download = `${previewText.value.join("")}.png`
    link.href = canvas.toDataURL()
    link.click()
  })
}
</script>

<template>
<div class="w-full h-full flex justify-center">
  <div class="w-1/5 max-h-80 min-w-[265px] py-8 mt-40 bg-common border border-[#eee]">
    <van-cell-group inset>
      <van-cell>
        <template #title>
          <div class="flex items-center justify-start gap-x-8">
            <div>预览</div>
            <div
                id="final-icon"
              class="w-[96px] h-[96px] gap-y-[10px] flex-col-center rounded-xl text-white font-bold"
              :style="{backgroundColor: pickedColor}"
            >
              <p v-if="previewText[0]" :style="{fontSize: previewSize[0]}">{{previewText[0]}}</p>
              <p v-if="previewText[1]" :style="{fontSize: previewSize[1]}">{{previewText[1]}}</p>
            </div>
          </div>
        </template>
      </van-cell>
      <van-field
        v-model="fieldText"
        type="text"
        label="文字"
        label-width="50px"
        input-align="left"
        @update:model-value="fieldChange"
      >
      </van-field>
      <van-field
          v-model="pickedColor"
          type="color"
          label="颜色"
          label-width="50px"
          input-align="left"
          style="width: 12rem"
      >
      </van-field>
      <van-button type="success" style="margin: .5rem 1rem;width: 8rem" size="normal" @click="downloadPicture">下载</van-button>
    </van-cell-group>
  </div>
</div>
</template>

<style scoped>

</style>