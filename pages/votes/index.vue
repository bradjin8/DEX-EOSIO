<template lang='pug'>
  .markets.mt-5
    .proposals
      .col-lg-12.center.d-flex.justify-content-between
        h3 Proposals
        nuxt-link(to="/votes/create-proposal")
          el-button(tag="el-button" size="large" icon="")
            i.el-icon-circle-plus-outline  Create Proposal
      ul(v-for='proposal in proposals')
        li.alcor-card.mt-4.justify-content-between.d-flex
          span.el-tag.el-tag--large.el-tag--light {{proposal.ver}}
          h4.mb-0 {{proposal.desc}}
          nuxt-link(:to="proposal.btn_event")
            el-button(tag="el-button" size="large" icon="") {{proposal.btn_name}}

</template>

<script>
import { captureException } from '@sentry/browser'

import { mapGetters, mapState } from 'vuex'
import TokenImage from '~/components/elements/TokenImage'
import ChangePercent from '~/components/trade/ChangePercent'
import PairIcons from '~/components/PairIcons'

export default {
  components: {
    TokenImage,
    ChangePercent,
    PairIcons
  },
  data() {
    return {
      search: '',

      to_assets: [],
      select: {
        from: '',
        to: ''
      },

      proposals: [
        {
          ver: '1.0',
          desc: 'This is our first Dex project, let\'s improve together',
          btn_name: 'EXECUTED',
          btn_event: ''
        },
        { ver: '1.1', desc: 'Next project is growing up!', btn_name: 'EXECUTED', btn_event: '' },
        { ver: '1.2', desc: 'Our Dex will grow up?', btn_name: 'EXECUTED', btn_event: '' },
        { ver: '1.3', desc: 'Will be final version?', btn_name: 'EXECUTED', btn_event: '' }
      ],

      loading: true
    }
  },

  computed: {
    ...mapState(['network']),
    ...mapGetters(['user']),
    ...mapState(['markets']),
    ...mapState('settings', ['favMarkets']),

    markets_active_tab: {
      get() {
        return this.$store.state.market.markets_active_tab || this.network.baseToken.symbol
      },

      set(value) {
        this.$store.commit('market/setMarketActiveTab', value)
      }
    },

    showVolumeInUSD: {
      get() {
        return this.$store.state.market.showVolumeInUSD
      },

      set(value) {
        this.$store.commit('market/setShowVolumeInUSD', value)
      }
    },

    filteredMarkets() {
      if (!this.markets) return []

      let markets = []
      if (this.markets_active_tab == 'all') {
        markets = this.markets
      } else if (this.markets_active_tab == this.network.baseToken.symbol) {
        markets = this.markets.filter(
          (i) => i.base_token.contract == this.network.baseToken.contract
        )
      } else if (this.markets_active_tab == 'USDT') {
        markets = this.markets.filter(
          (i) => i.base_token.contract == 'tethertether'
        )
      } else if (this.markets_active_tab == 'fav') {
        markets = this.markets.filter(
          (i) => this.favMarkets.includes(i.id)
        )
      } else if (this.markets_active_tab == 'Terraformers') {
        markets = this.markets.filter(
          (i) => i.quote_token.contract == 'unboundtoken'
        )
      } else {
        const ibcTokens = this.$store.state.ibcTokens.filter(
          (i) => i != this.network.baseToken.contract
        )

        markets = this.markets.filter((i) => {
          return (
            ibcTokens.includes(i.base_token.contract) ||
            ibcTokens.includes(i.quote_token.contract) ||
            Object.keys(this.network.withdraw).includes(i.quote_token.str) ||
            Object.keys(this.network.withdraw).includes(i.base_token.str)
          )
        })
      }

      markets = markets.filter((i) =>
        i.slug.includes(this.search.toLowerCase())
      )

      return markets.reverse()
    }
  },

  mounted() {
    const { tab, search } = this.$route.query

    if (tab) {
      this.markets_active_tab = tab
    }

    if (search) this.search = search
  },

  methods: {
    clickOrder(a, b, event) {
      if (event && event.target.tagName.toLowerCase() === 'a') return

      this.$router.push({ name: 'trade-index-id', params: { id: a.slug } })
    }
  }
}
</script>

<style lang='scss'>
.theme-dark {
  .el-input__inner {
    background-color: var(--bg-alter-2);
  }
}

.theme-light .markets .el-card {
  border: none !important;
}

.markets {
  .custom-radio .el-radio-button__inner {
    padding: 8px 15px !important;
  }

}
</style>

<style lang='scss' scoped>
.table-intro {
  display: flex;
  align-items: center;
  //justify-content: space-between;
  flex-wrap: wrap;
  padding: 20px 0;

  .search-container {
    width: 450px;
  }
}

.table td,
.table th {
  border: 0 !important;
}

.last-price-item {
  width: 180px !important;
}

.pair-item {
  width: 300px !important;
}

.theme-dark {
  .markets {
    .el-card__body {
      padding: 0px;
    }
  }

  .el-input__inner {
    background-color: var(--bg-alter-2);
  }
}

.markets {
  .el-table__row {
    cursor: pointer;
  }
}

@media only screen and (max-width: 640px) {
  .table-intro {
    padding: 14px 0;
    flex-direction: column-reverse;
    justify-content: center;

    .search-container {
      width: 100%;
      margin-bottom: 12px;
    }
  }
}
</style>
