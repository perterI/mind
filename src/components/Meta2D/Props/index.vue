<!--
 * @Descripttion:
 * @version: 1.0.0
 * @Author: htang
 * @Date: 2024-04-03 10:23:59
 * @LastEditors: htang
 * @LastEditTime: 2024-04-03 10:31:48
-->
<template>
  <div class="app-props">
    <FileProps ref="fileProps" v-if="selections.mode === SelectionMode.File" />
    <PenProps v-else-if="selections.mode === SelectionMode.Pen" />
  </div>
</template>

<script lang="ts">
import { ref, getCurrentInstance, defineComponent, watch, nextTick } from "vue";
import FileProps from "@/components/Meta2D/FileProps/index.vue";
import PenProps from "@/components/Meta2D/PenProps/index.vue";
import { useSelection, SelectionMode } from "@/services/selections";
export default defineComponent({
  components: { FileProps, PenProps },
  setup(props) {
    let { proxy } = getCurrentInstance();
    const { selections } = useSelection();

    let data = ref({});

    function onInit(dataPens) {
      data.value = dataPens;
      if (proxy.$refs.fileProps) {
        proxy.$refs.fileProps.onInit(dataPens);
      }
    }

    watch(
      () => selections.mode,
      (newValue) => {
        if (newValue == SelectionMode.File) {
          nextTick(() => {
            if (proxy.$refs.fileProps) {
              proxy.$refs.fileProps.onInit(meta2d.data());
            }
          });
        }
      }
    );

    return {
      data,
      selections,
      SelectionMode,
      onInit,
    };
  },
});
</script>

<style lang="less" scoped>
.app-props {
  height: calc(100vh - 40px);
  padding: 0 12px;
  border-left: 1px solid #ddd;
  overflow-y: auto;
  z-index: 2;
}
</style>