<script setup>
import CodeEditer from '@/component/CodeEditer/Index.vue';
import { reactive, ref } from 'vue';
const codeEditerValue = ref('');
const codeEditerConfig = reactive({
  fontSize: 14,
  language: 'javascript',
  theme: 'vs-dark',
});
</script>

<template>
  <div class="app-component">
    <div class="center-box">
      <h1>Code Editer Demo</h1>
      <div class="opt-box">
        <el-select v-model="codeEditerConfig.language" class="m-2" placeholder="Select">
          <el-option
            v-for="item in ['python', 'javascript', 'json', 'css', 'html', 'typescript']"
            :key="item"
            :label="item"
            :value="item"
          />
        </el-select>
      </div>
      <div class="edit-area">
        <div class="edit-box">
          <CodeEditer v-model="codeEditerValue" class="code-editer-box" :config="codeEditerConfig"></CodeEditer>
        </div>
        <div class="log-box">
          <el-input
            v-model="codeEditerValue"
            class="textarea"
            :input-style="{ height: '100%' }"
            type="textarea"
            readonly
          ></el-input>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="less" scoped>
.app-component {
  display: flex;
  justify-content: center;
  align-content: center;

  h1 {
    text-align: center;
  }
  .opt-box {
    margin-bottom: 5px;
  }
  .edit-area {
    display: flex;
    width: 1000px;
    height: 600px;
    .edit-box {
      width: 50%;
      box-sizing: border-box;
      border-radius: 4px;
      overflow: hidden;
    }
    .log-box {
      width: 50%;
      box-sizing: border-box;
      margin-left: 20px;

      .textarea {
        height: 100%;
        font-size: 14px;
      }
    }
  }
}
</style>
