<template>
  <div class="todo">
    <h1>Todo List</h1>
    <h2>共有{{ lists.length }}项任务，其中{{ finished.length }}项已完成</h2>
    <div v-for="(item, index) in lists" :key="index">
      <div v-if="!item.checked" class="task">
        <button @click="remove(index)" id="del-btn">删除</button>
        <div v-if="!item.isEdit" @click="item.isEdit = true">
          {{ item.text }}
        </div>
        <input
          v-else
          type="text"
          v-model="item.text"
          @blur="change(item.text, index)"
          id="edit-inp"
        />
        <button @click="item.checked = !item.checked" id="ok-btn">完成</button>
      </div>
    </div>
    <hr />
    <div
      v-for="(item, index) in finished"
      :key="'finished' + index"
      class="finish"
    >
      {{ item.text }}
    </div>
    <div class="editor">
      <input
        type="text"
        placeholder="请在此处输入添加的Tasks"
        v-model="addContent"
        @keydown.enter="add"
        id="add-inp"
      />
      <button type="primary" @click="add" id="add-btn">+</button>
    </div>
  </div>
</template>

<script setup>
import { computed, reactive, toRefs, ref } from "vue";

const state = reactive({
  lists: [
    { text: "任务1", checked: false, isEdit: false },
    { text: "任务2", checked: false, isEdit: false },
    { text: "任务3", checked: false, isEdit: false },
  ],
  finished: computed(() => state.lists.filter((item) => item.checked == true)),
});

let addContent = ref(""); //预设内容，以便添加时使用
//添加新的Task
const add = () => {
  if (addContent.value) {
    state.lists.push({
      text: addContent.value,
      checked: false,
      isEdit: false,
    });
    addContent.value = "";
  } else {
    alert("内容信息不能为空");
  }
};

//删除Task
const remove = (index) => {
  state.lists.splice(index, 1);
};

//修改Task
const change = (text, index) => {
  if (text) {
    state.lists[index].isEdit = false;
  } else {
    state.lists[index].text = "空";
  }
};

const { lists, finished } = toRefs(state);
</script>

<style scoped>
.todo {
  max-width: 500px;
  margin: 10vh auto;
  border-radius: 30px;
  background-color: white;
  color: #6b397f;
}
h1 {
  margin: 0;
}
h2 {
  margin: 0;
  font-size: 16px;
  color: #ccc;
}
h3 {
  text-align: center;
}
hr {
  margin: 10px;
}
.task,
.finish,
#edit-inp {
  font-size: 20px;
  height: 40px;
  line-height: 40px;
  background-color: #f1f1f1;
  text-align: center;
  color: #6b397f;
}
.task {
  margin: 5px 0;
  display: flex;
  justify-content: space-between;
}
.finish {
  color: rgb(155, 155, 155);
  margin: 5px 0;
}
#del-btn {
  background-color: #a23630;
  color: white;
  border: none;
}
#ok-btn {
  background-color: #3e54a4;
  color: white;
  border: none;
}
.editor {
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  height: 10vh;
}
.editor > input {
  font-size: 20px;
  height: 60%;
  width: 85%;
  text-align: center;
  box-sizing: border-box;
  border-radius: 10px 0 0 10px;
  border-color: #6b397f;
}
.editor > button {
  height: 60%;
  width: 15%;
  border: none;
  background-color: #6b397f;
  color: white;
  font-size: 20px;
  font-weight: bold;
  border-radius: 0 10px 10px 0;
}
</style>
