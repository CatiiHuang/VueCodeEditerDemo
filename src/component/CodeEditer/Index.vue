<template>
  <div ref="editerWrap" class="code-editer-wrap"></div>
</template>

<script setup>
// 依赖挂载
import monaco from './hooks/dependencies';
import { computed, onMounted, ref, watch } from 'vue';

let codeEditer = null;
const editerWrap = ref(null);
const props = defineProps({
  modelValue: {
    type: String,
    default: '',
  },
  config: {
    type: Object,
    default: () => ({
      value: ``,
      fontSize: 14,
      readOnly: false,
      language: 'javascript',
    }),
  },
});
const emits = defineEmits(['change', 'update:modelValue', 'blur', 'focus']);

// 编辑器配置
const codeEditerConfig = computed(() =>
  Object.assign(
    {
      value: props.modelValue,
      fontSize: 14,
      readOnly: false,
      language: 'javascript',
      automaticLayout: true,
      minimap: {
        enabled: true,
      },
      scrollBeyondLastLine: false,
      overviewRulerBorder: false,
    },
    props.config
  )
);

// 初始化编辑器
const initCodeEditer = () => {
  codeEditer = monaco.editor.create(editerWrap.value, codeEditerConfig.value);
};

// 内容变化监听
const initCodeChange = () => {
  codeEditer.onDidChangeModelContent(() => {
    const value = getCodeEditerValue();
    emits('update:modelValue', value);
    emits('change', value);
  });
};

// 焦点变化监听
const blurAndFoucsMoniter = () => {
  codeEditer.onDidBlurEditorText((e) => {
    emits('blur', e);
  });
  codeEditer.onDidFocusEditorText((e) => {
    emits('focus', e);
  });
};

// 获取内容
const getCodeEditerValue = () => codeEditer.getValue();

// 设置内容
const setCodeEditerValue = (val) => codeEditer.setValue(val);

// 格式化代码
const codeEditerFormat = () => codeEditer.trigger('anything', 'editor.action.formatDocument');

// 更新配置
const updateCodeEditerConfig = () => {
  codeEditer.updateOptions(codeEditerConfig.value);
  console.log(codeEditerConfig.value);
};

// 双向绑定，更新编辑器内容
watch(
  () => props.modelValue,
  (val) => {
    if (val !== getCodeEditerValue()) setCodeEditerValue(val);
  }
);

// 监听配置修改
watch(
  () => props.config,
  () => {
    if (codeEditer) updateCodeEditerConfig();
  },
  { immediate: true, deep: true }
);

// 监听语言配置变更
watch(
  () => props.config.language,
  (val) => {
    if (codeEditer) monaco.editor.setModelLanguage(codeEditer.getModel(), val);
  },
  { immediate: true, deep: true }
);

onMounted(() => {
  initCodeEditer();
  initCodeChange();
  blurAndFoucsMoniter();
});

defineExpose({ codeEditerFormat });
</script>

<style lang="less" scoped>
.code-editer-wrap {
  border: 1px solid #ccc;
  height: 100%;
  width: 100%;
}
</style>
