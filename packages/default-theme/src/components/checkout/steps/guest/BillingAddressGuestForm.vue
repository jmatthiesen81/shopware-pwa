<template>
  <div>
    <SfCheckbox
      v-model="differentThanShipping"
      :label="$t('Use different address for billing')"
      name="copyShippingAddress"
      class="sw-form__checkbox"
      data-cy="different-address-for-billing"
    />
    <div v-if="differentThanShipping" class="sw-form" data-cy="form">
      <div class="inputs-group">
        <SwInput
          v-model="firstName"
          :valid="!validations.firstName.$error"
          :error-message="$t('This field is required')"
          :label="$t('First name')"
          data-cy="first-name"
          name="firstName"
          class="sw-form__input"
          required
        />
        <SwInput
          v-model="lastName"
          :valid="!validations.lastName.$error"
          :error-message="$t('This field is required')"
          :label="$t('Last name')"
          data-cy="last-name"
          name="lastName"
          class="sw-form__input"
          required
        />
        <SwInput
          v-model="street"
          :valid="!validations.street.$error"
          :error-message="$t('This field is required')"
          :label="$t('Street')"
          data-cy="street-name"
          name="street"
          class="sw-form__input"
          required
        />
      </div>
      <div class="inputs-group">
        <SwInput
          v-model="apartment"
          :valid="!validations.apartment.$error"
          :error-message="$t('This field is required')"
          :label="$t('House/Apartment number')"
          data-cy="apartment"
          name="apartment"
          class="sw-form__input"
          required
        />
        <SwInput
          v-model="city"
          :valid="!validations.city.$error"
          :error-message="$t('This field is required')"
          :label="$t('City')"
          data-cy="city"
          name="city"
          class="sw-form__input"
          required
        />
        <SwInput
          v-model="state"
          :valid="!validations.state.$error"
          :error-message="$t('This field is required')"
          :label="$t('State/Province')"
          data-cy="state"
          name="state"
          class="sw-form__input"
          required
        />
      </div>
      <div class="inputs-group">
        <SwInput
          v-model="zipcode"
          :valid="!validations.zipcode.$error"
          :error-message="$t('This field is required')"
          :label="$t('Zip code')"
          data-cy="zipcode"
          name="zipcode"
          class="sw-form__input"
          required
        />
        <SfSelect
          v-if="getCountries.length"
          v-model="countryId"
          :valid="!validations.countryId.$error"
          :error-message="$t('This field is required')"
          :label="$t('Country')"
          data-cy="country"
          class="sw-form__select sf-select--underlined"
          required
        >
          <SfSelectOption
            v-for="countryOption in getCountries"
            :key="countryOption.id"
            :value="countryOption.id"
          >
            {{ countryOption.name }}
          </SfSelectOption>
        </SfSelect>
        <SwInput
          v-model="phoneNumber"
          :valid="!validations.phoneNumber.$error"
          :error-message="$t('This field is required')"
          :label="$t('Phone number')"
          data-cy="phone"
          name="phone"
          class="sw-form__input"
          required
        />
      </div>
    </div>
  </div>
</template>
<script>
import { SfSelect, SfCheckbox } from "@storefront-ui/vue"
import { validationMixin } from "vuelidate"
import {
  usePaymentStep,
  usePaymentStepValidationRules,
} from "@/logic/checkout/usePaymentStep"
import { useCountries } from "@shopware-pwa/composables"
import SwInput from "@/components/atoms/SwInput"

export default {
  name: "BillingAddressGuestForm",
  components: {
    SwInput,
    SfSelect,
    SfCheckbox,
  },
  mixins: [validationMixin],
  setup(props, { root }) {
    const {
      validations,
      setValidations,
      firstName,
      lastName,
      street,
      apartment,
      city,
      state,
      zipcode,
      countryId,
      phoneNumber,
      differentThanShipping,
    } = usePaymentStep(root)

    const { getCountries } = useCountries(root)

    return {
      validations,
      setValidations,
      firstName,
      lastName,
      street,
      apartment,
      city,
      state,
      zipcode,
      countryId,
      phoneNumber,
      differentThanShipping,
      getCountries,
    }
  },
  watch: {
    $v: {
      immediate: true,
      handler() {
        this.setValidations(this.$v)
      },
    },
  },
  validations: {
    ...usePaymentStepValidationRules,
  },
}
</script>

<style lang="scss" scoped>
@import "@/assets/scss/forms";
</style>
