<script setup lang="ts">
import { createWorker } from 'tesseract.js';
import * as ExifReader from 'exifreader';

(async () => {
  const worker = await createWorker('eng+chi_sim');
  console.time('下载+识别时间：');
  const ret = await worker.recognize('https://tesseract.projectnaptha.com/img/eng_bw.png');
  console.log(ret.data.text);
  console.timeEnd('下载+识别时间：');
  await worker.terminate();
})();

async function selectFile(e: any) {
  console.log(e.target.files);

  // 异步加载文件并提取拍摄时间
  const tags = await ExifReader.load(e.target.files[0]);
  console.log(tags)
  const imageDate = tags['DateTimeOriginal']?.description;
  // 或者直接获取未处理的标签值
  const unprocessedTagValue = tags['DateTimeOriginal']?.value;

  console.log(imageDate, unprocessedTagValue)
}
</script>

<template>
  <div class="app">
    <img src="https://tesseract.projectnaptha.com/img/eng_bw.png" alt="" />
  </div>

  <div class="date">
    <input type="file" @change="selectFile" />
  </div>
</template>

<style scoped>
.app img {
  width: 300px;
}
</style>
