<template>
  <div ref="test">
    <div id="preview_box2">
      <img v-if="previewImageUrl" :src="previewImageUrl" alt="Preview">
    </div>
    </div>
    
  <div
    ref="editor"
    class="editor-layout module-editor empty-content"
    :class="className"
    contenteditable
    v-html="editorContent"
    spellcheck="false"
    placeholder="记笔记..."
    @paste.prevent="paste"
    @input="changeEditorContent"
  >
  
</div>

  <section class="bottom-editor-tools">
    <template v-for="item in bottomIcons" :key="item.name">
      <button class="icon" :title="item.title" @click="editorIconHandle($event, item.name)">
        <i class="iconfont" :class="item.icon"></i>
      </button>
    </template>

    
<!--     
    <i class="icon iconfont">&#xe702;</i>
    <input style="width: 50px;"  class="icon" title="上传图片"  type="file" accept="image/*"  @change="handleImageUpload" > -->

  </section>

</template>
<script lang="ts">
import { defineComponent, onMounted, ref, Ref, watch } from 'vue';
import { debounce } from '@/utils';
import { editorIcons } from '@/config';
import { exeConfig } from '@/store/exeConfig.state';
export default defineComponent({
  props: {
    content: String,
    className: String
  },
  emits: ['on-input'],
  setup(props, { emit }) {
    let editor: Ref<HTMLDivElement | null> = ref(null);
    const bottomIcons = editorIcons;
    const editorContent: Ref<string | undefined> = ref('');
    watch(props, nv => {
      if (!editorContent.value) {
        editorContent.value = nv.content;
      }
    });

    onMounted(() => {
      focus();
    });
    
const previewImageUrl = ref('')
const handleImageUpload = (event: { target: any; }) => {
  const input = event.target;
  const files = input.files;

  if (files && files.length > 0) {
    const file = files[0];
    const reader = new FileReader();

    reader.onload = () => {
      previewImageUrl.value = reader.result as string;
    };

    reader.readAsDataURL(file);
  }
};

    const focus = () => {
      const range = document.createRange();
      range.selectNodeContents(editor.value as HTMLDivElement);
      range.collapse(false);
      const selecton = window.getSelection() as Selection;
      selecton.removeAllRanges();
      selecton.addRange(range);
    };

    const editorIconHandle = (e: Event, name: string) => {
      e.preventDefault();
      document.execCommand(name, false);
    };

    const changeEditorContent = debounce((e: InputEvent) => {
      const editorHtml = (e.target as Element).innerHTML;
      emit('on-input', editorHtml);
    }, exeConfig.syncDelay);

    const paste = (e: ClipboardEvent) => {
      const pasteText = e.clipboardData?.getData('text/plain');
      console.log(pasteText);
      document.execCommand('insertText', false, pasteText);
    };
    return {
      editor,
      editorIconHandle,
      bottomIcons,
      changeEditorContent,
      paste,
      editorContent,
      previewImageUrl,
      handleImageUpload
    };
  }
});
</script>

<style lang="less" scoped>

#img_label2 {
  background-color: #f2d547;
  border-radius: 5px;
  display: inline-block;
  padding: 10px;
}
#preview_box2 img {
  width: 200px;
  position: fixed;
  z-index: 9999; 
  top: 10px;
  left: 20px;
  background-color: red;
}
.editor-layout {
  width: 100%;
  height: 100%;
  padding: 12px;
  box-sizing: border-box;
  outline: none;
  font-size: 14px;
  line-height: 1.8;
  overflow-y: auto;
  overflow-x: hidden;
  * {
    font-size: 14px;
    line-height: 1.8;
    word-break: break-all;
  }
  div {
    max-width: 100%;
    overflow-x: auto;
    word-break: break-all;
  }
  span {
    max-width: 100%;
    word-break: break-all;
  }
}

.bottom-editor-tools {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: @iconSize;
  background-color: transparent;
  border-top: 1px solid rgba(0, 0, 0, 0.03);
}

.black-content::before {
  color: @gray-color;
}
</style>
