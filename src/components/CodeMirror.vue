<script lang="ts" setup>
import CodeMirror from 'codemirror'

import 'codemirror/lib/codemirror.css'
import 'codemirror/mode/javascript/javascript.js'
import 'codemirror/addon/fold/foldgutter.css'
import 'codemirror/addon/fold/foldcode.js'
import 'codemirror/addon/fold/brace-fold.js'
import 'codemirror/addon/fold/comment-fold.js'
import 'codemirror/addon/fold/indent-fold.js'
import 'codemirror/addon/fold/foldgutter.js'
import 'codemirror/addon/display/placeholder.js'
import 'codemirror/addon/selection/active-line.js'

import {
  onBeforeUnmount, onMounted, ref,
} from 'vue'

const props = withDefaults(defineProps<Props>(), {
  modelValue: '',
})

const emits = defineEmits<{ (e: 'update:modelValue', value: string): void }>()

const codeEditorNode = ref(null)

let editor: any | null = null
interface Props {
  modelValue: string
}

onMounted(() => {
  editor = CodeMirror.fromTextArea(codeEditorNode.value, {
    value: props.modelValue,
    mode: 'javascript',
    smartIndent: true,
    lineNumbers: true,
    matchBrackets: true,
    foldGutter: true,
    lineWrapping: true,
    gutters: ['CodeMirror-linenumbers', 'CodeMirror-foldgutter', 'CodeMirror-lint-markers'],
    styleActiveLine: true, // 光标行高亮
  })
  editor.on('change', () => {
    emits('update:modelValue', editor?.getValue())
  })
})
onBeforeUnmount(() => {
  if (editor !== null) {
    editor.toTextArea()
    editor = null
  }
})

function setValue(text: string) {
  editor.setValue(text)
}

defineExpose({
  setValue,
})
</script>

<template>
  <textarea
    ref="codeEditorNode"
  />
</template>

<style>
.CodeMirror {
    height: 100%;
        line-height: 1.5rem;
        font-size: 1rem;
        /* border: none;
        outline: none; */
        background: none;
}
</style>
