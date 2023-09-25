<template>
  <Codemirror
      v-model:value="code"
      :options="cmOptions"
      border
      @change="onChange"
      @ready="onReady"
      @focus="onFocus"
  >
  </Codemirror>
</template>

<script lang="ts">
import {defineComponent, ref} from "vue";
import Codemirror from "codemirror-editor-vue3";

// @types/codemirror
import type {Doc, Editor, EditorChange, EditorConfiguration} from "codemirror";

// language
import "codemirror/mode/javascript/javascript.js";

export default defineComponent({
  components: {
    Codemirror,
  },
  setup() {
    const cminstance = ref<Editor>();
    const code = ref(`const ary = []
    for(var i = 0;i < 10;i ++) {
      window.alert(i);
    }
    `);

    const cmOptions: EditorConfiguration = {
      mode: "text/javascript",
      lineWrapping: true,
    };

    return {
      code,
      cmOptions,
      onReady(cm: Editor) {
        cminstance.value = cm;
      },
      onChange(value: string, cm: Editor) {
        console.log(value, cm);
      },
      onFocus(cm: Editor, event: FocusEvent) {
        console.log("onFocus", cm, event);
        cm.getDoc().on("beforeChange", (instance: Doc, obj: EditorChange) => {
          console.log("beforeChange", instance, obj);
        });
      },
    };
  },
});
</script>
