<template lang='pug'>
  .markets.mt-5
    .d-flex.justify-content-center
      .col-md-7.alcor-card
        row.d-flex.justify-content-between
          nuxt-link(to='/votes')
            i.el-icon-back
          h4.text-center Create Proposal
          span
        ul.mt-4
          li
          .row
            .col
              el-alert(type="success" title="Tip!" show-icon)
                p
                 | Select an action and describe your proposal for the community. The proposal cannot be modified after submission, so please verify all information before submitting. The voting period will begin immediately and last for 7 days. To propose a custom action, read the docs.

</template>

<script>
import TokenImage from '~/components/elements/TokenImage'
import ChangePercent from '~/components/trade/ChangePercent'
import PairIcons from '~/components/PairIcons'

export default {
  components: {
    TokenImage,
    ChangePercent,
    PairIcons
  }
}
</script>

<style scoped>

</style>
