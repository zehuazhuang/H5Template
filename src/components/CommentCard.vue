<script setup lang="ts">
  import Head from '@/assets/public/Head.png'
  import { useAppImgStyle } from '@/hooks/useAppImgStyle'
  import { detailId } from '@/hooks/useDetail'
  import { useJump } from '@/hooks/useJump'
  import { useUserStore } from '@/stores'

  const { reportIcon } = useAppImgStyle()
  const { userInfo } = useUserStore()
  const { appParams } = useJump()

  const props = withDefaults(
    defineProps<{
      list?: CommentInfo[]
    }>(),
    {
      list: () => []
    }
  )

  // 举报弹框
  const isReport = ref(false)
  const isLoginRequired = ref(false)

  const onOpenReport = (id: string) => {
    if (userInfo.userId === 'yk912') {
      isLoginRequired.value = true
      return
    }
    detailId.value = id
    isReport.value = true
  }

  const onGoSignIn = () => {
    appParams({ key: 'gosignin', state: 2 })
  }
</script>

<template>
  <div safe-area-inset-bottom>
    <empty v-if="props.list.length === 0" />
    <div
      v-for="(item, index) in props.list"
      :key="index"
      p-4
      ai-fill-bg
      ai-rounded
      class="card-comment"
    >
      <ul flex items-center justify-between>
        <li flex items-center>
          <van-image
            round
            ai-avatar
            :src="item?.avator || Head"
            fit="cover"
          />
          <span ml-3 ai-user-name>{{ item?.name || '' }}</span>
        </li>
        <li v-if="userInfo.userId !== item.userId" flex items-center>
          <van-image 
            :src="reportIcon"
            :style="{
              width: 'var(--report-image-width)',
              height: 'var(--report-image-height)'
            }"
            @click="onOpenReport(item.userId)"
          />
        </li>
      </ul>
      <span mt-2 ai-text-desc>{{ item?.content || '' }}</span>
    </div>

    <report-box v-model:show="isReport" />
    <login-required-popup
      v-model:show="isLoginRequired"
      @signin="onGoSignIn"
    />
  </div>
</template>

<style lang="less" scoped>
  .card-comment + .card-comment {
    margin-top: 12px;
  } 
</style>
