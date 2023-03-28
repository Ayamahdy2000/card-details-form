<template>
  <div class="complete-state">
    <img
      :src="require('@/assets/images/icon-complete.svg')"
      alt="complete-icon"
      class="complete-state__icon"
    />
    <h3 class="complete-state__heading">thank you!</h3>
    <p class="complete-state__note">We've added your card details</p>
    <button class="complete-state__btn" type="submit">Continue</button>
  </div>
</template>
<script>
export default {};
</script>
