<template>
  <div class="transaction-detail">
    <div class="header">
      <h1 class="underlined">Transaction</h1>
    </div>
    <div class="body">
      <div class="section">
        <div class="row">
          <div class="column">
            <div class="label">Hash</div>
            <div class="value">{{ transaction.hash }}</div>
          </div>
        </div>
        <div class="row">
          <div class="column">
            <div class="label">Date</div>
            <div class="value">{{ date }}</div>
          </div>
          <div class="column">
            <div class="label">Time</div>
            <div class="value">{{ time }}</div>
          </div>
        </div>
      </div>
      <div class="section">
        <div class="row">
          <div class="column">
            <div class="label">Token</div>
            <div class="value red">{{ token }}</div>
          </div>
          <div class="column">
            <div class="label">Value</div>
            <div class="value">{{ value }}</div>
          </div>
        </div>
        <div class="row">
          <div class="column">
            <div class="label">Block</div>
            <div class="value purple">{{ block }}</div>
          </div>
          <div class="column">
            <div class="label">Status</div>
            <div class="value red">{{ transaction.status }}</div>
          </div>
        </div>
      </div>
      <div class="section">
        <div class="row">
          <div class="column">
            <div class="label">From</div>
            <div class="value">{{ transaction.fromHash }}</div>
          </div>
        </div>
        <div class="row">
          <div class="column">
            <div class="label">To</div>
            <div class="value purple">{{ transaction.toHash }}</div>
          </div>
        </div>
      </div>
      <div class="section">
        <div class="row">
          <div class="column">
            <div class="label">Network Fee</div>
            <div class="value">{{ networkFee }}</div>
          </div>
          <div class="column">
            <div class="label">System Fee</div>
            <div class="value">{{ systemFee }}</div>
          </div>
          <div class="column">
            <div class="label">Size</div>
            <div class="value">{{ size }}</div>
          </div>
        </div>
        <div class="row has-equal-columns">
          <div class="column">
            <div class="label">Confirmations</div>
            <div class="value">{{ confirmations }}</div>
          </div>
          <div class="column confirmed" v-if="this.transaction.confirmed">
            <aph-icon name="confirmed"></aph-icon>
            <div class="label">Confirmed</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    block() {
      return this.$accounting.formatNumber(this.transaction.block);
    },

    confirmations() {
      return this.$accounting.formatNumber(this.transaction.confirmations);
    },

    date() {
      return this.$moment(this.transaction.timestamp, 'X').format(this.$constants.formats.DATE);
    },

    networkFee() {
      return `${this.$accounting.formatNumber(this.transaction.networkFee)} GAS`;
    },

    size() {
      return `${this.$accounting.formatNumber(this.transaction.size)} Bytes`;
    },

    systemFee() {
      return `${this.$accounting.formatNumber(this.transaction.systemFee)} GAS`;
    },

    time() {
      return this.$moment(this.transaction.timestamp, 'X').format(this.$constants.formats.TIME);
    },

    token() {
      return `${this.$accounting.formatNumber(this.transaction.block)} ${this.transaction.currency}`;
    },

    value() {
      return `${this.$accounting.formatMoney(this.transaction.amount)} USD`;
    },
  },

  data() {
    return {
      transaction: {
        amount: 6000,
        block: 1864638,
        confirmations: 48754,
        confirmed: true,
        currency: 'APH',
        fromHash: '1FeexV6bAHb8ybZjqQMjJrcCrHGW9sb6uF',
        hash: this.hash,
        networkFee: 0,
        size: 219,
        status: 'Sent',
        systemFee: 0,
        timestamp: 1521643932,
        toHash: '1AZkUjL1rMUeqKsZfNf4cgrtUCqhCborz9',
        usd: 6000,
      },
    };
  },

  props: {
    hash: {
      type: String,
    },
  },
};
</script>

<style lang="scss">
.transaction-detail {
  background: white;
  border-radius: $border-radius;
  display: flex;
  flex-direction: column;
  padding-bottom: $space;

  .header {
    padding: $space;

    h1.underlined {
      @extend %underlined-header;

      flex: 1;
      margin-bottom: 0;
    }
  }

  .body {
    overflow: auto;
    padding: $space $space 0 $space;

    .section + .section {
      margin-top: $space-lg;
    }

    .row {
      display: flex;

      .column {
        flex: 1;

        .label {
          @extend %small-uppercase-grey-label;

          margin-bottom: $space-sm;
        }

        .value {
          font-family: GilroySemibold;
          font-size: toRem(12px);

          &.purple {
            color: $purple;
          }

          &.red {
            color: $red;
          }
        }

        & + .column {
          margin-left: $space-xl;
        }

        &:last-child {
          flex: 3;
        }

        &.confirmed {
          align-items: center;
          display: flex;

          .label {
            color: $green;
            margin: 0 0 0 $space-sm;
          }

          .aph-icon {
            svg {
              height: toRem(30px);

              .stroke {
                stroke: $green;
              }
            }
          }
        }
      }

      &.has-equal-columns {
        .column {
          flex: 1;
        }
      }

      & + .row {
        margin-top: $space;
      }
    }
  }
}
</style>
