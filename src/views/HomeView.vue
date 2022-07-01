<template>
  <div class="home">
    <h1>Todo List</h1>
    <h3>Get things done, one item at a time.</h3>
    <input
      type="text"
      placeholder="添加ToDo"
      class="inp"
      v-model="inpVal"
      @keyup.enter="addItem"
    />
    <hr />
    <div class="func">
      <div class="processing">
        <div class="head">
          <h2 class="l">正在进行</h2>
          <div class="r">{{ compressingNum }}</div>
        </div>
        <ul class="box">
          <li v-for="(item, index) in compressingList" :key="item">
            <div class="left">
              <input
                type="checkbox"
                :checked="false"
                @click="changeStatus($event, 'compressing')"
              />
              <span>{{ item }}</span>
            </div>
            <div class="right" @click="delItem($event, 'compressing')">x</div>
          </li>
        </ul>
      </div>
      <div class="completed">
        <div class="head">
          <h2 class="l" @click="completedStatus = !completedStatus">
            已经完成
          </h2>
          <div class="r">{{ completedNum }}</div>
        </div>
        <ul class="box" v-if="completedStatus">
          <li v-for="(item, index) in completedList" :key="item">
            <div class="left">
              <input
                type="checkbox"
                :checked="true"
                @click="changeStatus($event, 'completed')"
              />
              <span>{{ item }}</span>
            </div>
            <div class="right" @click="delItem($event, 'completed')">x</div>
          </li>
        </ul>
      </div>
    </div>
    <button class="clearAll" @click="clearAll">All clear,are you sure?</button>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
export default {
  name: "HomeView",
  setup() {
    let inpVal = ref(""),
      compressingList = reactive([]),
      compressingNum = ref(0),
      completedList = reactive([]),
      completedNum = ref(0),
      completedStatus = ref(false);

    // 按下回车时将数据添加到“正在进行”
    let addItem = () => {
        // 输入框的值
        let inpValue = inpVal.value;
        if (inpValue.trim() === "") {
          alert("内容不允许为空");
          return;
        }
        // 正在进行的数量
        compressingNum.value++;
        // 将值放入正在进行的数组中
        compressingList.push(inpValue);
        // 清空输入框的值
        inpVal.value = "";
      },
      // 改变任务的状态
      changeStatus = (e, status) => {
        // 获取任务的值
        let currentVal = e.target.nextElementSibling.innerHTML;
        // 如果传递的状态是正在进行
        if (status === "compressing") {
          // 获取正在进行数组该值的下标
          let index = compressingList.findIndex((v) => v === currentVal);
          // 将该值从正在进行数组删除
          compressingList.splice(index, 1);
          // 将正在进行的任务数量减一
          compressingNum.value--;
          // 将该值放入已经完成数组
          completedList.push(currentVal);
          // 已经完成的数量加一
          completedNum.value++;
        } else {
          // 否则传递的状态是已经完成
          // 获取已经完成数组该值的下标
          let index = completedList.findIndex((v) => v === currentVal);
          // 将该值从已经完成数组删除
          completedList.splice(index, 1);
          // 将已经完成的任务数量减一
          completedNum.value--;
          // 将该值放入正在进行数组
          compressingList.push(currentVal);
          // 正在进行的数量加一
          compressingNum.value++;
        }
      },
      // 点击x删除数据
      delItem = (e, status) => {
        // 获取任务的值
        let currentVal = e.target.previousElementSibling.lastChild;
        // 如果传递的状态是正在进行
        if (status === "compressing") {
          // 获取正在进行数组该值的下标
          let index = compressingList.findIndex((v) => v === currentVal);
          // 将该值从正在进行数组删除
          compressingList.splice(index, 1);
          // 将正在进行的任务数量减一
          compressingNum.value--;
        } else {
          // 否则传递的状态是已经完成
          // 获取已经完成数组该值的下标
          let index = completedList.findIndex((v) => v === currentVal);
          // 将该值从已经完成数组删除
          completedList.splice(index, 1);
          // 将已经完成的任务数量减一
          completedNum.value--;
        }
      },
      // 删除所有正在进行的任务删除
      clearAll = () => {
        if (confirm("确定删除所有正在进行的任务？")) {
          compressingList.length = compressingNum.value = 0;
        }
      };

    return {
      inpVal,
      addItem,
      compressingList,
      compressingNum,
      completedList,
      completedNum,
      changeStatus,
      completedStatus,
      clearAll,
      delItem,
    };
  },
};
</script>

<style lang="scss">
.home {
  width: 400px;
  min-height: 600px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #eee;
  padding: 15px;
  h3 {
    margin-top: 5px;
  }
}
input.inp {
  width: 100%;
  height: 40px;
  padding-left: 15px;
  margin-top: 10px;
  box-sizing: border-box;
  border-radius: 10px;
  outline: none;
  border: none;
  font-size: 16px;
}
hr {
  margin: 15px 0;
}
.func {
  .head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    .l {
      cursor: pointer;
    }
    .r {
      display: block;
      background: #dfdfdf;
      width: 30px;
      height: 30px;
      line-height: 30px;
      text-align: center;
      border-radius: 50%;
    }
  }
  .box {
    li {
      display: flex;
      justify-content: space-between;
      padding: 0 5px;
      align-items: center;
      height: 30px;
      line-height: 30px;
      .left {
        input {
          margin-right: 5px;
          cursor: pointer;
        }
      }
      .right {
        cursor: pointer;
        font-size: 18px;
        &:hover {
          color: tomato;
        }
      }
    }
  }
  .processing {
    border-bottom: 1px solid #ccc;
    padding-bottom: 10px;
    margin-bottom: 10px;
  }
}
.clearAll {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translate(-50%);
  border-radius: 50px;
  cursor: pointer;
  padding: 5px 10px;
  outline: none;
  background: #fff;
  border: none;
  &:hover {
    color: tomato;
  }
}
</style>