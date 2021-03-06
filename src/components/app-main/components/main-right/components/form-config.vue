<template>
  <div>
    <attrs-header
      url="https://www.yuque.com/chaojie-vjiel/vbwzgu/dyw8a7"
      title="表单配置"
      v-model="keyword"
    />
    <ele-form
      :formData="formAttr"
      @input="updateFormAttr"
      :form-desc="filterFormDesc"
      :isShowBackBtn="false"
      :isShowSubmitBtn="false"
      :span="20"
      ref="ele-form"
      labelPosition="top"
    ></ele-form>
  </div>
</template>

<script lang="ts">
import store from "@/store";
import { changeFormLabel } from "@/helpers/tool";
import searchMixin from "./components/searchMixin";
import AttrsHeader from "./components/attrs-header.vue";
import { createComponent, toRefs, computed } from "@vue/composition-api";
import { FormDesc } from "@/types/formList";

export default createComponent({
  name: "AppFormConfig",
  components: { AttrsHeader },
  setup() {
    const { formAttr } = toRefs(store.state);

    const originDesc: FormDesc = {
      inline: {
        type: "radio",
        default: false,
        label: "inline模式 / layout模式",
        options: [
          { text: "layout模式", value: false },
          { text: "inline模式", value: true }
        ],
        on: {
          change: (val: any) => {
            if (!val) {
              formAttr.value.isResponsive = true;
            }
          }
        }
      },
      isResponsive: {
        type: "switch",
        label: "是否响应式",
        vif: (data: AnyObj) => !data.inline,
        options: [
          { text: "是", value: true },
          { text: "否", value: false }
        ]
      },
      labelPosition: {
        type: "select",
        label: "标签位置",
        options(data: AnyObj) {
          const options: any[] = ["left", "right", "top"];
          if (data.isResponsive && !data.inline) {
            options.unshift({ text: "响应式", value: null });
          }
          return options;
        }
      },
      span: {
        type: "select",
        label: "表单宽度",
        vif: (data: AnyObj) => !data.inline,
        options(data: AnyObj) {
          const options: any[] = Array.from({ length: 24 }, (v, i) => {
            return { text: `${24 - i} / 24`, value: 24 - i };
          });
          if (data.isResponsive) {
            options.unshift({ text: "响应式", value: null });
          }
          return options;
        },
        style: {
          width: "100%"
        }
      },
      isDialog: {
        type: "switch",
        label: "是否为弹窗"
      },
      isShowLabel: {
        type: "switch",
        label: "是否显示标签"
      },
      labelWidth: {
        type: "input",
        label: "标签宽度",
        attrs: {
          type: "number",
          min: 0,
          step: 10
        },
        tip: "默认值为auto"
      },
      disabled: {
        type: "switch",
        label: "全局禁用表单"
      },
      isShowSubmitBtn: {
        type: "radio",
        label: "提交按钮",
        options: [
          { text: "显示", value: true },
          { text: "隐藏", value: false }
        ]
      },
      isShowResetBtn: {
        type: "radio",
        label: "重置按钮",
        options: [
          { text: "显示", value: true },
          { text: "隐藏", value: false }
        ]
      },
      isShowBackBtn: {
        type: "radio",
        label: "返回按钮",
        options: [
          { text: "默认", value: null },
          { text: "显示", value: true },
          { text: "隐藏", value: false }
        ]
      },
      isShowCancelBtn: {
        type: "radio",
        label: "取消按钮",
        options: [
          { text: "默认", value: null },
          { text: "显示", value: true },
          { text: "隐藏", value: false }
        ]
      },
      formBtnSize: {
        type: "select",
        label: "表单按钮大小",
        options: [{ text: "默认", value: null }, "medium", "small", "mini"]
      },
      submitBtnText: {
        type: "input",
        label: "提交按钮文字"
      },
      backBtnText: {
        type: "input",
        label: "返回按钮文字"
      },
      cancelBtnText: {
        type: "input",
        label: "取消按钮文字"
      },
      resetBtnText: {
        type: "input",
        label: "返回按钮文字"
      }
    };

    const formDesc = computed(() => changeFormLabel(originDesc));
    return {
      updateFormAttr: (data: AnyObj) => store.commit("updateFormAttr", data),
      formAttr,
      ...searchMixin(formDesc)
    };
  }
});
</script>
