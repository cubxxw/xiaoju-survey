<template>
   <el-dialog
    :modelValue="visible"
    title="APP嵌入式问卷"
    width="500"
    :before-close="handleClose"
  >
    <el-form
      ref="formRef"
      style="max-width: 600px"
      :model="channelForm"
      label-width="auto"
      class="demo-ruleForm"
    >
      <el-form-item
        label="投放名称："
        prop="name"
        :rules="[
          { required: true, message: '请输入投放名称', trigger: 'blur' },
        ]"
      >
        <el-input
          v-model.number="channelForm.name"
          type="text"
          autocomplete="off"
        />
      </el-form-item>
    </el-form>
    <template #footer>
      <div class="dialog-footer">
        <el-button @click="handleClose">取消</el-button>
        <el-button type="primary" @click="handleConfirm">
          确认
        </el-button>
      </div>
    </template>
  </el-dialog>
</template>  
<script lang="ts" setup>
import { shallowRef, reactive, watch } from 'vue'
const props = defineProps({
  visible: {
    type: Boolean,
    default: false,
  },
  channel: {
    type: Object,
    default: () => {
      return {}
    },
  },
})
const emit = defineEmits(['close', 'confirm'])
const channelForm = reactive({
  name: '',
})
const formRef = shallowRef()
watch(
  () => props.visible,
  (val) => {
    if (val) {
      channelForm.name = props.channel.name || ''
    }
  }
)
const handleClose = () => {
  emit('close')
}
const handleConfirm = () => {
  formRef.value.validate(async (valid: boolean) => {
    if (valid) {
      emit('confirm', channelForm.name.toString())
    }
  })
}
</script>