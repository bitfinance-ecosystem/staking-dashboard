<template>
  <div :class="containerClasses">
    <img
      v-if="this.hasLogo && !isImageLoaded && !loadedWithError"
      class="loader li-validator-image"
      src="~assets/images/loader.svg"
      alt="a small spinning circle to display loading"
    />
    <img
      v-if="this.hasLogo && !loadedWithError"
      :src="this.imageSrc"
      :style="{ display: isImageLoaded ? 'block' : 'none' }"
      class="li-validator-image"
      :alt="`validator logo for ` + this.name"
      @error="loadedWithError = true"
      @load="isImageLoaded = true"
    />
    <Avatar
      v-if="!this.hasLogo || loadedWithError"
      class="li-validator-image"
      alt="generic validator logo - generated avatar from address"
      :address="operatorAddress"
    />
  </div>
</template>

<script>
import Avatar from "common/Avatar"
import {fetchValidatorAvatarSrcByAddress} from "@/mock-service"
import {mapState} from "vuex";

export default {
  name: `ValidatorLogo`,
  components: {
    Avatar
  },
  props: ["operatorAddress", "hasLogo", "name", "size"],
  data() {
    return {
      isImageLoaded: false,
      loadedWithError: false
    }
  },
  methods: {
    fetchValidatorAvatarSrcByAddress
  },
  computed: {
    ...mapState({
      chainTitle: state => state.connection.chainTitle
    }),
    imageSrc() {
      return fetchValidatorAvatarSrcByAddress(this.chainTitle ,this.operatorAddress)
    },
    containerClasses() {
      return {
        "validator-logo-container": true,
        large: this.size === "large"
      }
    }
  }
}
</script>
<style lang="scss">
.validator-logo-container {
  margin-right: 10px;
  display: flex;

  > div {
    background-color: #ffffffa1;
  }

  .li-validator-image {
    border-radius: 0.25rem;
    height: 40px;
    width: 40px;
    border: 1px solid var(--bc-dim);
    padding: 5px;
  }

  .loader {
    padding: 10px 0;
    border: none;
  }

  &.large {
    margin-right: 20px;

    .li-validator-image {
      height: 64px;
      width: 64px;
      min-height: 64px;
      min-width: 64px;
    }

    .loader {
      padding: 15px 0;
    }
  }
}
</style>
