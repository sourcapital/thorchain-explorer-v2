<template>
  <Card :is-loading="isLoading || !(tableSettings && tableSettings.length > 0)" extra-class="stat-table" :title="header" :img-src="iconSrc">
    <div class="stat-table-container">
      <div v-for="(row, ri) in tableSettings" :key="ri" class="stat-table-row">
        <div
          v-for="(colItem, ci) in row"
          :key="ci + ',' + ri"
          class="stat-table-group"
        >
          <div v-if="colItem.image" class="img-div">
            <img :src="colItem.image" alt="item-icon">
          </div>
          <div class="stat-table-item">
            <div class="col-header">
              {{ colItem.name }}
              <unknown-icon class="header-icon" v-if="colItem.extraInfo" v-tooltip="colItem.extraInfo" />
            </div>
            <div class="col-value">
              <template v-if="!$slots[colItem.slotName]">
                <template v-if="colItem.filter">
                  <pre v-if="colItem.value && colItem.runeValue" class="rune-value">{{ colItem.value | number('0,0.00') }} <small>RUNE</small></pre>
                  <pre v-else-if="colItem.value !== 0">{{ colItem.value || '-' }}</pre>
                  <pre v-if="colItem.value === 0" :class="{'rune-value': colItem.runeValue}">0</pre>
                </template>
                <template v-else>
                  <template v-if="colItem.value !== 0">
                    {{ colItem.value | number('0,0') }}
                  </template>
                  <template v-else-if="colItem.value === 0">
                    0
                  </template>
                  <template v-else>
                    -
                  </template>
                </template>
              </template>
              <slot v-else :name="colItem.slotName" />
              <span v-if="colItem.value && colItem.usdValue" class="usd-value">({{ colItem.value * runePrice | currency }})</span>
              <span v-if="colItem.extraText" class="usd-value">({{ colItem.extraText }})</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Card>
</template>

<script>
import { mapGetters } from 'vuex'
import { AssetCurrencySymbol } from '@xchainjs/xchain-util'
import UnknownIcon from '~/assets/images/unknown.svg?inline'

export default {
  name: 'StatTable',
  components: { UnknownIcon },
  props: {
    iconSrc: String,
    header: String,
    tableSettings: Array,
    isLoading: Boolean
  },
  computed: {
    ...mapGetters({
      runePrice: 'getRunePrice'
    })
  },
  methods: {
    runeCur () {
      return AssetCurrencySymbol.RUNE
    }
  }
}
</script>

<style lang="scss">
.stat-table {
  width: 100%;

  .stat-header {
    display: flex;
    justify-content: space-between;
    font-weight: bold;
    padding: 1rem;

    background-color: var(--card-bg-color);
    border: 1px solid var(--border-color);
    border-radius: 5px 5px 0 0;
    border-bottom: none;

    .stat-header-btn {
      background-color: transparent;
      border: none;
    }

    .stat-header-text {
      display: flex;
      align-items: center;
      font-size: 1.125rem;
      font-weight: 700;
    }

    .header-icon {
      margin-right: 10px;
      height: 1.5rem;
    }
  }

  .stat-table-container {
    background-color: var(--bg-color);
    border-radius: 0 0 5px 5px;
    border: 1px solid var(--border-color);
  }

  .stat-table-row {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    border-bottom: 1px solid var(--border-color);

    &:last-of-type {
      border-bottom: none;
    }

    .stat-table-group {
      display: flex;
      align-items: center;
      padding: 0.5rem 1rem;
      flex: 1 0;

      .img-div {
        display: inherit;
        img {
          border-radius: 50%;
          margin-right: .5rem;
          width: 1.5rem;
        }
      }
    }

    .stat-table-item {
      flex: 1;

      .col-value {
        color: var(--sec-font-color);

        .usd-value {
          color: var(--font-color);
          font-size: .8rem;
        }
      }

      .col-header {
        display: flex;
        font-size: 0.75rem;

        .header-icon {
          margin-left: 5px;
          fill: var(--font-color);
          margin-right: 10px;
          height: .9rem;
          width: .9rem;
        }
      }
    }
  }
}

pre {
  font-family: 'ProductSans';
  white-space: pre-wrap;
  word-wrap: break-word;
  word-break: break-all;
  margin: 0;
}

pre.rune-value {
  font-family: 'Roboto Mono';
  font-size: .8rem;
}
</style>
