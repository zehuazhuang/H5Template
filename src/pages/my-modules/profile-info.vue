<script setup lang="ts">
  import { closeToast, showLoadingToast, showSuccessToast } from 'vant'
  import { reactive } from 'vue'
  import defaultHead from '@/assets/public/default-head.png'
  import { useFile } from '@/hooks/useFile'
  import { useJump } from '@/hooks/useJump'
  import { useUserStore } from '@/stores'

  defineOptions({
    name: 'ProfileInfo'
  })

  const { userInfo } = useUserStore()
  const { imgUrl, clickElement } = useFile()
  const { appParams } = useJump()

  const locationOptions = [
    'United States',
    'China',
    'Japan',
    'United Kingdom',
    'Canada',
    'Germany',
    'France',
    'Italy',
    'Spain',
    'Australia',
    'India',
    'Brazil',
    'Mexico',
    'South Korea',
    'Russia',
    'Netherlands',
    'Switzerland',
    'Sweden',
    'Norway',
    'Denmark',
    'Finland',
    'Poland',
    'Turkey',
    'Saudi Arabia',
    'United Arab Emirates',
    'Singapore',
    'Thailand',
    'Malaysia',
    'Indonesia',
    'Philippines',
    'Vietnam',
    'New Zealand',
    'South Africa',
    'Egypt'
  ].map(value => ({ text: value, value }))

  const formData = reactive({
    name: userInfo.name || '',
    birthday: '2003-01-01',
    location: 'United States',
    gender: 'male',
    avator: userInfo.avator || ''
  })

  const showLocationPicker = ref(false)

  const onLocationConfirm = ({ selectedValues }: { selectedValues: unknown[] }) => {
    formData.location = String(selectedValues[0] || '')
    showLocationPicker.value = false
  }

  const onSubmit = async () => {
    showLoadingToast({
      message: 'Saving...',
      forbidClick: true,
      duration: 0
    })

    await new Promise(resolve =>
      setTimeout(
        resolve,
        Math.floor(Math.random() * (2000 - 500 + 1)) + 500
      )
    )

    const data = {
      avator: imgUrl.value || userInfo.avator,
      name: formData.name || userInfo.name
    }

    closeToast()
    showSuccessToast('Saved successfully')

    setTimeout(() => {
      appParams({
        key: 'getinfo',
        value: data,
        state: 1
      })
    }, 1000)
  }
</script>

<template>
  <div safe-area-inset-top class="profile-info_box">
    <div class="top-glow" />

    <div flex justify-center relative class="avatar-wrap">
      <van-image
        round
        h-16
        w-16
        :src="imgUrl || formData.avator || defaultHead"
        fit="cover"
        @click="clickElement"
      />
      <button class="avatar-upload" type="button" @click="clickElement">
        <van-icon name="photograph" />
      </button>
    </div>

    <div class="form-panel">
      <div class="field-title">Name</div>
      <van-field
        v-model="formData.name"
        placeholder="Please enter"
        class="public-input"
      />

      <div class="field-title">Birthday</div>
      <van-field
        v-model="formData.birthday"
        readonly
        class="public-input select-input"
        right-icon="arrow-down"
      />

      <div class="field-title">Location</div>
      <van-field
        v-model="formData.location"
        readonly
        class="public-input select-input"
        right-icon="arrow-down"
        @click="showLocationPicker = true"
      />

      <div class="field-title">Gender</div>
      <div class="gender-row">
        <button
          type="button"
          class="gender-btn is-male"
          :class="{ active: formData.gender === 'male' }"
          @click="formData.gender = 'male'"
        >
          <span class="gender-icon">♂</span>
        </button>
        <button
          type="button"
          class="gender-btn is-female"
          :class="{ active: formData.gender === 'female' }"
          @click="formData.gender = 'female'"
        >
          <span class="gender-icon">♀</span>
        </button>
      </div>

      <div flex justify-center class="save-wrap">
        <button class="save-btn" type="button" @click="onSubmit">Save</button>
      </div>
    </div>

    <van-popup v-model:show="showLocationPicker" round position="bottom">
      <van-picker
        :columns="locationOptions"
        @cancel="showLocationPicker = false"
        @confirm="onLocationConfirm"
      />
    </van-popup>
  </div>
</template>

<style lang="less" scoped>
  .profile-info_box {
    position: relative;
    overflow: hidden;
    padding-top: calc(var(--van-nav-bar-height) + 20px);
    min-height: 100vh;
    background: var(--ai-edit-bg-color);
  }

  .top-glow {
    position: absolute;
    top: -84px;
    left: -30px;
    right: -30px;
    height: 170px;
    background:
      radial-gradient(
        circle at 50% 12%,
        rgba(220, 47, 255, 0.95) 0,
        rgba(164, 47, 206, 0.9) 30%,
        rgba(86, 35, 120, 0.45) 55%,
        rgba(14, 16, 24, 0) 78%
      );
    filter: blur(10px);
    pointer-events: none;
  }

  .avatar-wrap {
    z-index: 1;
    margin-bottom: 14px;
  }

  .avatar-upload {
    position: absolute;
    right: calc(50% - 34px);
    bottom: -1px;
    width: 22px;
    height: 22px;
    border: 2px solid var(--ai-edit-bg-color);
    border-radius: 50%;
    color: #fff;
    background: linear-gradient(135deg, #bc49ff 0%, #8355ff 100%);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;
  }

  .form-panel {
    position: relative;
    z-index: 1;
    padding: 0 15px 30px;
  }

  .field-title {
    margin: 8px 0 9px;
    color: #fff;
    font-size: 16px;
    font-weight: 800;
    line-height: 20px;
  }

  :deep(.public-input) {
    height: 43px;
    padding: 0 12px;
    border-radius: 8px;

    .van-field__body,
    .van-field__control {
      height: 100%;
    }

    .van-field__control {
      color: rgba(255, 255, 255, 0.86);
      font-size: 12px;
    }

    .van-field__right-icon {
      color: rgba(255, 255, 255, 0.46);
      font-size: 18px;
    }
  }

  .gender-row {
    display: flex;
    align-items: center;
    gap: 58px;
    padding-left: 28px;
  }

  .gender-btn {
    width: 47px;
    height: 47px;
    border: 0;
    border-radius: 50%;
    background: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;
  }

  .gender-icon {
    display: block;
    width: 24px;
    height: 24px;
    color: currentColor;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 30px;
    font-weight: 400;
    line-height: 24px;
    text-align: center;
    transform: translateY(-1px);
  }

  .gender-btn.is-male {
    color: #646cff;
  }

  .gender-btn.is-female {
    color: #ff49c7;
  }

  .gender-btn.active {
    color: #fff;
    background: linear-gradient(135deg, #bf4cff 0%, #536cff 100%);
  }

  .save-wrap {
    margin-top: 74px;
  }

  .save-btn {
    width: 218px;
    height: 44px;
    border: 0;
    border-radius: 999px;
    color: #fff;
    background: linear-gradient(180deg, #b032e8 0%, #d756ff 100%);
    font-size: 12px;
    font-weight: 800;
  }
</style>
