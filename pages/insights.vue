<template>
  <Page>
    <Nav
      :is-link="true"
      :nav-items="[
        {link: `/insights`, text: 'Overview'},
        {link: `/insights/network`, text: 'Network'},
        {link: `/insights/price`, text: 'Rune Price'},
        {link: `/insights/apr`, text: 'APR'},
      ]"
    />
    <nuxt-child />
  </Page>
</template>

<script>
export default {

}
</script>

<style lang="scss">
.footer {
  display: block;
  margin: 1rem 0 .3rem .5rem;
}

</style>
