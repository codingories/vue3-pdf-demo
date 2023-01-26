<template>
  <div @click="changePdf" class="divStyle">click</div>
  <div @click="changePdf" class="paint">click</div>
  <div class="pdfStyle" ref="pdfStyle">
    <vue-pdf-embed :source="source1"
                   @loaded="pdfLoaded"
                   :disableAnnotationLayer="true"
                   @rendered="rendered"
                   ref="myPdfRef"
    />
  </div>
</template>

<script setup lang="ts">
import VuePdfEmbed from 'vue-pdf-embed'
import { ref } from 'vue';
let source1 = ref<string>('./test.pdf')
const myPdfRef = ref<HTMLElement | null>(null)
const pdfStyle = ref<HTMLElement | null>(null)


const canvas = ref<HTMLElement | null>(null)
const context = ref(null)
const isDrawing = ref<boolean>(false)


const changePdf = () => {
  source1.value = 'https://arkokoley.github.io/pdfvuer/nationStates.pdf'
}
const pdfLoaded = (pdf) => {
  // pdf 已经加载完成
  console.log('pdf', pdf)
}

const rendered = () => {
  const value = myPdfRef.value
  console.log('rendered', myPdfRef.value.$el.children[0].querySelector('canvas'))
  console.log('pdfStyle',pdfStyle.value)
  let myPdfCanvas = myPdfRef.value.$el.children[0].querySelector('canvas')
  canvas.value = myPdfCanvas
  console.log('canvas1111',canvas.value)
  let myContext = canvas.value.getContext('2d')
  context.value = myContext
  console.log('context', context)
  canvas.value.addEventListener('mousedown', startDrawing)
  canvas.value.addEventListener('mousemove', draw)
  canvas.value.addEventListener('mouseup', mouseup)
  canvas.value.addEventListener('mouseout', mouseout)
}


const startDrawing = (event) => {
  console.log('startDrawing', event)
  isDrawing.value = true
  context.value.beginPath()
  context.value.moveTo(event.clientX, event.clientY)
}

const draw = (event) => {
  console.log('draw', event)
}


const mouseup = () => {
  console.log('mouseup')
}


const mouseout = () => {
  console.log('mouseout')
}


// const pdfPageChanged = (page) => {
//   // pdf 的页面已经改变
//   console.log('page', page)
// }



</script>


<style scoped lang="scss">
.divStyle {
  border: 1px solid red;
  width: 200px;
  height: 100px;
}
.pdfStyle {
  border: 1px solid green;
  display: flex;
  //justify-content: center;
  width: 100vh;
  .vue-pdf-embed {
    width: 100vh;
  }
}
</style>
