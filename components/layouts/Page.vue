<template>
  <div class="page-container">
    <slot v-if="!error" />
    <Card v-else>
      <div class="page-error-container">
        <img src="@/assets/images/error.png" alt="error image">
        <h3>Can't fetch the data, please try again later.</h3>
      </div>
    </Card>
  </div>
</template>

<script>
export default {
  props: ['error']
}
</script>

<style lang="scss" scoped>
.page-container {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 10px;
  max-width: 90rem;
  margin: auto;
}

.page-error-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 1rem;

  img {
    width: 150px;
    margin-bottom: 1rem;
  }
}
</style>
