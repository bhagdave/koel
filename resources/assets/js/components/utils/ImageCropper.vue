<template>
  <div class="w-full h-full fixed top-0 left-0 flex items-center justify-center z-[99] bg-black/70">
    <div class="relative max-w-full max-h-full rounded-md flex">
      <Cropper
        ref="cropper"
        :max-width="config.maxWidth"
        :min-width="config.minWidth"
        :src="source"
        :stencil-props="{ aspectRatio: 1 }"
      />
      <div class="fixed top-6 right-6 flex flex-1 gap-2">
        <Btn success @click.prevent="crop">Crop</Btn>
        <Btn transparent @click.prevent="emits('cancel')">Cancel</Btn>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, toRefs } from 'vue'
import { Cropper } from 'vue-advanced-cropper'
import 'vue-advanced-cropper/dist/style.css'

import Btn from '@/components/ui/form/Btn.vue'

const props = withDefaults(defineProps<{
  source?: string | null
  config?: {
    minWidth: number
    maxWidth?: number
  }
}>(), {
  source: null,
  config: () => ({
    minWidth: 192,
  }),
})

const emits = defineEmits<{
  (e: 'crop', result: string): void
  (e: 'cancel'): void
}>()
const { source, config } = toRefs(props)
const cropper = ref<typeof Cropper>()

const crop = () => emits('crop', cropper.value?.getResult().canvas.toDataURL())
</script>
