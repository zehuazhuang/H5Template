<script setup lang="ts">
  const show = defineModel<boolean>('show', {
    type: Boolean,
    required: true,
    default: false
  })

  const emit = defineEmits<{
    signin: []
  }>()
</script>

<template>
  <van-popup
    v-model:show="show"
    :close-on-click-overlay="true"
    class="login-required-popup"
    overlay-class="login-required-overlay"
  >
    <div class="login-card">
      <h3>Login Required</h3>
      <p>You need to follow each other to start a conversation.</p>
    </div>
    <div class="login-arrow" />
    <button class="signin-btn" type="button" @click="emit('signin')">
      Sign In
    </button>
  </van-popup>
</template>

<style lang="less" scoped>
  :global(.login-required-overlay) {
    background: transparent;
  }

  :global(.login-required-popup) {
    overflow: visible;
    width: 82%;
    max-width: 310px;
    background: transparent !important;
    box-shadow: none;
  }

  .login-card {
    position: relative;
    min-height: 124px;
    padding: 19px 22px 32px;
    border-radius: 24px;
    color: #1d1324;
    background: linear-gradient(180deg, #e4a3ff 0%, #fff 43%);
    text-align: center;
  }

  h3 {
    margin: 0;
    font-size: 16px;
    font-weight: 900;
    line-height: 22px;
  }

  p {
    margin: 13px auto 0;
    max-width: 218px;
    color: rgba(29, 19, 36, 0.45);
    font-size: 12px;
    font-weight: 400;
    line-height: 17px;
  }

  .login-arrow {
    width: 34px;
    height: 24px;
    margin: -13px auto 20px;
    border-radius: 0 0 18px 18px;
    background: #fff;
  }

  .signin-btn {
    display: block;
    width: 190px;
    height: 47px;
    margin: 0 auto;
    border: 0;
    border-radius: 999px;
    color: #fff;
    background: linear-gradient(180deg, #a420d9 0%, #d65cff 100%);
    font-size: 13px;
    font-weight: 800;
  }
</style>
