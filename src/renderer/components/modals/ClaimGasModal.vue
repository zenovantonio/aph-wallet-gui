<template>
  <modal-wrapper id="aph-claim-gas-modal" identifier="claimGas">
    <template>
      <div class="body" v-if="$store.state.gasClaim">
        <p>Claiming GAS which has accumulated from your NEO holdings takes several steps. We've automated the process for you but it may take up to 5 minutes.</p>
        <p>Please wait for the GAS claim to complete.</p>
        <p>Closing your wallet during this process may result in the GAS claim failing and require you to run it again.</p>
        <div class="checklist">
          <div class="checklist-header">Steps:</div>
          <ul>
            <li :class="stepClass(1)"><span>{{stepIndicator(1)}}</span>{{step1Label}}</li>
            <li :class="stepClass(2)"><span>{{stepIndicator(2)}}</span>{{step2Label}}</li>
            <li :class="stepClass(3)"><span>{{stepIndicator(3)}}</span>{{step3Label}}</li>
            <li :class="stepClass(4)"><span>{{stepIndicator(4)}}</span>{{step4Label}}</li>
            <li :class="stepClass(5)"><span>{{stepIndicator(5)}}</span>{{step5Label}}</li>
          </ul>

          <div class="error" v-if="error !== null">
            {{error}}
          </div>
        </div>
      </div>
      <div class="footer">
        <div class="cancel-btn" @click="close">{{closeLabel}}</div>
      </div>
    </template>
  </modal-wrapper>
</template>

<script>
import ModalWrapper from './ModalWrapper';
export default {
  components: {
    ModalWrapper,
  },
  computed: {
    closeLabel() {
      if (this.$store.state.gasClaim
        && (this.$store.state.gasClaim.step >= 5 || this.$store.state.gasClaim.error !== null)) {
        return 'Close';
      }
      return 'Cancel';
    },
    error() {
      if (this.$store.state.gasClaim && this.$store.state.gasClaim.error !== null) {
        return this.$store.state.gasClaim.error;
      }
      return null;
    },
    step1Label() {
      if (this.$store.state.gasClaim && this.$store.state.gasClaim.step > 1) {
        return `Transferred ${this.$store.state.gasClaim.neoTransferAmount} NEO to yourself.`;
      } else if (this.$store.state.gasClaim && this.$store.state.gasClaim.step === 1) {
        return `Transferring ${this.$store.state.gasClaim.neoTransferAmount} NEO to yourself.`;
      }
      return 'Transfer all NEO to yourself.';
    },
    step2Label() {
      if (this.$store.state.gasClaim && this.$store.state.gasClaim.step > 2) {
        return 'Received NEO transfer confirmation.';
      } else if (this.$store.state.gasClaim && this.$store.state.gasClaim.step === 2) {
        return 'Waiting for NEO transfer confirmation.';
      }
      return 'Wait for NEO transfer confirmation.';
    },
    step3Label() {
      if (this.$store.state.gasClaim && this.$store.state.gasClaim.step > 3) {
        return `Sent claim for ~${this.$store.state.gasClaim.gasClaimAmount} GAS.`;
      } else if (this.$store.state.gasClaim && this.$store.state.gasClaim.step === 3) {
        return `Sending claim for ~${this.$store.state.gasClaim.gasClaimAmount} GAS.`;
      }
      return 'Send GAS claim.';
    },
    step4Label() {
      if (this.$store.state.gasClaim && this.$store.state.gasClaim.step > 4) {
        return 'GAS claim confirmed.';
      } else if (this.$store.state.gasClaim && this.$store.state.gasClaim.step === 4) {
        return 'Waiting for GAS claim confirmation.';
      }
      return 'Wait for GAS claim confirmation.';
    },
    step5Label() {
      if (this.$store.state.gasClaim && this.$store.state.gasClaim.step === 5) {
        return 'Success!';
      }
      return 'Wait for GAS claim transaction details.';
    },
  },
  methods: {
    close() {
      this.$store.commit('setSendInProgress', false);
      this.$store.commit('setShowClaimGasModal', false);
    },
    stepClass(step) {
      if (this.$store.state.gasClaim.step === step) {
        return ['in-progress'];
      }
      if (this.$store.state.gasClaim.step > step) {
        return ['complete'];
      }
      return [];
    },
    stepIndicator(step) {
      if (this.$store.state.gasClaim.step <= step
        && this.$store.state.gasClaim.step < 5) {
        return `${step}.`;
      }
      return '✔';
    },
  },
};
</script>


<style lang="scss">
#aph-claim-gas-modal {
  .content {
    width: toRem(600px);
  }
  .header {
    padding: $space-lg $space-lg 0;
  }
  .body {
    display: block;
    padding: $space-lg;
    position: relative;
    text-align: center;
    p {
      line-height: $line-height;
      margin: 0;
      &:first-child {
        span {
          font-family: GilroySemibold;
        }
      }
      & + p {
        margin-top: $space-lg;
      }
    }
    .aph-icon {
      svg {
        height: $space-xl;
      }
      & + p {
        margin-top: $space-xl;
      }
    }
    .checklist {
      margin: 0 auto;
      max-width: toRem(450px);
      text-align: left;
      .checklist-header {
        font-size: toRem(20px);
        font-family: GilroyMedium;
        padding: $space-lg;
        text-align: center;
      }
      ul {
        margin: 0;
        list-style: none;
        li {
          color: $grey;
          padding: $space-sm 0;
          
          span {
            padding: $space-sm;
          }
          &.in-progress {
            color: $dark;
            font-family: GilroySemibold;
            &:last-child {
              color: $green;
            }
          }
          &.complete {
            color: $dark;
          
            span {
              color: $green;
            }
          }
        }
      }
      .error {
        color: $red;
        font-size: toRem(15px);
        font-family: GilroySemibold;
        text-align: center;
        padding: $space;
      }
    }
  }
  .footer {
    display: flex;
    > * {
      flex: 1;
    }
  }
  .cancel-btn {
    @extend %btn-footer-light;
    border-bottom-left-radius: $border-radius;
    border-bottom-right-radius: $border-radius;
  }
}
</style>
