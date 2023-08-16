<template>
  <tempage>
    <template v-slot:header>
      <h1 class="h_title">日历待办</h1>
      <div class="h_add">添加事件</div>
    </template>
    <template v-slot:body>
      <div class="clanders">
        <div class="clander_head">
          <div class="front" @click="front()">
            <el-icon><ArrowLeft /></el-icon>
          </div>
          <div class="mouthtime" @click="select()">
            {{ `${year}年${mouth}月` }}
          </div>
          <div class="next" @click="next()">
            <el-icon><ArrowRight /></el-icon>
          </div>
        </div>
        <div class="clander_body" ref="clander_body">
          <div class="body_head">
            <ul>
              <li>一</li>
              <li>二</li>
              <li>三</li>
              <li>四</li>
              <li>五</li>
              <li>六</li>
              <li>日</li>
            </ul>
          </div>
          <div class="body_content" ref="clander_content">
            <ul v-for="(mouth, index) in bigarr" :key="index">
              <li v-for="(item, index) in mouth" :key="index">
                <div class="number">{{ item }}</div>
                <div class="point" v-if="true"></div>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <div class="affair">
        <ul>
          <li>
            <div class="status"></div>
            <div class="describle">
              <p>2023.5.2</p>
              <p>事件</p>
            </div>
            <div class="operate">
              <a href="" class="edit">编辑</a>
              <a href="" class="delete">删除</a>
            </div>
          </li>
          <li>
            <div class="status"></div>
            <div class="describle">
              <p>2023.5.2</p>
              <p>事件</p>
            </div>
            <div class="operate">
              <a href="" class="edit">编辑</a>
              <a href="" class="delete">删除</a>
            </div>
          </li>
          <li>
            <div class="status"></div>
            <div class="describle">
              <p>2023.5.2</p>
              <p>事件</p>
            </div>
            <div class="operate">
              <a href="" class="edit">编辑</a>
              <a href="" class="delete">删除</a>
            </div>
          </li>
        </ul>
      </div>
    </template>
  </tempage>
</template>

<script setup lang="ts">
import { ref } from 'vue'

let date = new Date()
console.log(date)
let year = ref(date.getFullYear())
let mouth = ref(date.getMonth() + 1)
let day = ref(date.getDate())
year.value = date.getFullYear()
mouth.value = date.getMonth() + 1
day.value = date.getDate()
let nmouth: number = mouth.value
let nyear: number = year.value
let bigarr: number[][] = [[1], [2], [3]]
function getthreemouth(nyear: number, nmouth: number) {
  for (let i = -1; i < 2; i++) {
    let tem: number[] = getmouth(nyear, nmouth + i)
    bigarr[i + 1] = tem
  }
}
function getmouth(nyear: number, nmouth: number): number[] {
  let lastmouth = new Date(nyear, nmouth - 1, 0)
  let nowmouth = new Date(nyear, nmouth, 0)
  let lday = lastmouth.getDay()
  let nday = nowmouth.getDate()
  let leastd = lastmouth.getDate()
  let nextl = 35 - lday - nday
  let arr: number[] = []
  for (let i = lday; i > 0; i--) {
    arr[i - 1] = leastd
    leastd--
  }
  for (let i = 0; i < nday; i++) {
    arr[i + lday] = i + 1
  }
  for (let i = 0; i < nextl; i++) {
    arr[lday + nday + i] = i + 1
  }
  // console.log(arr)
  return arr
}
getthreemouth(nyear, nmouth)

let clander_content = ref()
let clander_body = ref()
const clander_move = (way: boolean = true) => {
  if (way) {
    clander_content.value.style.transition = 'none'
    clander_content.value.style.transform = 'translateX(-33%)'
    setTimeout(() => {
      getthreemouth(year.value, mouth.value)
      clander_content.value.style.transition = 'all 1s'
      clander_content.value.style.transform = 'translateX(-66%)'
    }, 100)
  } else {
    clander_content.value.style.transition = 'none'
    clander_content.value.style.transform = 'translateX(-33%)'
    setTimeout(() => {
      getthreemouth(year.value, mouth.value)
      clander_content.value.style.transition = 'all 1s'
      clander_content.value.style.transform = 'translateX(0)'
    }, 100)
  }
}
const next = () => {
  mouth.value++
  if (mouth.value > 12) {
    mouth.value = 1
    year.value++
  }
  clander_move()
}
const front = () => {
  mouth.value--
  if (mouth.value < 1) {
    mouth.value = 12
    year.value--
  }
  clander_move(false)
}
const select = () => {
  clander_body.value.style.animationName = 'zoomOut'
}
</script>

<style scoped lang="scss">
@mixin go {
  display: flex;
  align-items: center;
  .el-icon {
    width: 3vh;
    height: 3vh;
    color: #3a3a3a;
    cursor: pointer;
    svg {
      width: 100%;
      height: 100%;
    }
  }
}
@keyframes zoomOut {
  from {
    opacity: 1;
  }

  50% {
    opacity: 0;
    transform: scale3d(0.3, 0.3, 0.3);
  }

  to {
    opacity: 0;
  }
}
.content_head {
  .h_title {
    font-size: 5vh;
    color: #157fb4;
    min-height: 23px;
  }
  .h_add {
    width: 16vh;
    height: 5.5vh;
    position: absolute;
    bottom: 0;
    right: 0;
    background-color: #cccccc;
    border: 1px solid #cccccc;
    border-radius: 1.7vh;
    font-size: 2.7vh;
    text-align: center;
    min-height: 21px;
    min-width: 58px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
  }
}
.content_body {
  height: 72vh;
  .clanders {
    width: 46.87vh;
    height: 67.3vh;
    background-color: #ffffff;
    float: left;
    padding: 3vh 5vh;
    min-width: 203px;
    min-height: 261px;
    .clander_head {
      width: 100%;
      height: 5vh;
      display: flex;
      justify-content: space-between;
      .front {
        @include go;
      }
      .next {
        @include go;
      }
      .mouthtime {
        font-size: 2.96vh;
        color: #333333;
        cursor: pointer;
      }
    }

    .clander_body {
      width: 100%;
      margin-top: 5vh;
      overflow: hidden;
      transition: all 1s;
      .body_head {
        height: 5vh;
      }
      ul li {
        float: left;
        width: 14%;
        text-align: center;
        font-size: 2.5vh;
      }
      ul li:last-child {
        margin-right: 0;
      }
      .body_content {
        width: 300%;
        height: 55vh;
        min-width: 609px;
        min-height: 196px;
        transition: all 1s;
        transform: translateX(-33%);
        .body_content::after {
          content: '';
          display: block;
          clear: both;
        }
        ul {
          height: 60vh;
          float: left;
          width: 33%;
        }
        ul li {
          width: 14%;
          height: 5vh;
          text-align: center;
          margin-right: 0;
          margin-top: 4vh;
          .number {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 3.8vh;
            height: 3.8vh;
            margin: 0 auto;
            border-radius: 1vh;
          }
          .number:hover {
            background-color: #919191;
          }
          .point {
            width: 1vh;
            height: 1vh;
            border-radius: 50%;
            background-color: #51d386;
            margin: 0.5vh auto;
          }
        }
      }
    }
  }
  .affair {
    width: 61vh;
    height: 64.3vh;
    background-color: #f2efe6;
    float: right;
    min-width: 217px;
    ul {
      width: 100%;
      li {
        width: 100%;
        height: 8.5vh;
        background-color: white;
        box-shadow: 0px 0.6vh 0px rgba(0, 0, 0, 0.25);
        border-radius: 0.5vh;
        display: flex;
        align-items: center;
        margin-bottom: 3.7vh;

        .status {
          float: left;
          height: 5.6vh;
          width: 1.1vh;
          background-color: #43cf7c;
          border-radius: 0.9vh;
          margin: 1.5vh 3.5vh;
        }
        .describle {
          float: left;
          display: flex;
          flex-direction: column;
          align-items: flex-start;
          justify-content: space-between;
          p {
            font-size: 1.8vh;
          }
        }
        .operate {
          width: 10vh;
          float: right;
          background-color: white;
          justify-self: flex-end;
          margin-left: auto;
          a {
            width: 40%;
            font-size: 1.8vh;
            margin-left: 5%;
          }
          .edit {
            color: #000;
          }
          .delete {
            color: #ec4a4b;
          }
        }
      }
    }
  }
}

@media screen and (max-width: 2000px) {
  .content_body .affair ul li .operate {
    width: 17vh;
  }
}
@media screen and (max-width: 400px) {
  .content_body .affair {
    float: left;
    margin-top: 40px;
  }
}
</style>
