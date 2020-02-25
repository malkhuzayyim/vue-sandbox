<template>
  <v-form
    ref="cardForm"
    @submit.prevent="submit()"
    v-model="valid"
    lazy-validation
  >
    <div class="pa-1 title">
      Credit Card Form
    </div>
    <v-btn @click="$vuetify.rtl = !$vuetify.rtl">
      Switch to {{ $vuetify.rtl ? `LTR` : `RTL` }}
    </v-btn>
    <div class="pt-6"></div>
    <v-text-field
      v-model="card_holder_name"
      :rules="rulesCardHolderName"
      label="Card Name"
      validate-on-blur
      name="card_holder_name"
      prepend-inner-icon="mdi-account"
      outlined
      class="mx-1"
    ></v-text-field>
    <v-text-field
      :id="$vuetify.rtl ? `card-number` : ``"
      v-model="cardNumberMasked"
      v-mask="cardMask"
      :rules="rulesCardNumber"
      label="Card Number"
      prepend-inner-icon="mdi-credit-card-outline"
      validate-on-blur
      name="card_number"
      outlined
      class="mx-1"
    >
    </v-text-field>
    <div class="d-flex">
      <v-text-field
        :id="$vuetify.rtl ? `card-expiry` : ``"
        v-model="expiryDateMasked"
        v-mask="`## / ##`"
        :rules="rulesExpiryDate"
        label="Expirty Date"
        validate-on-blur
        name="expiry_date"
        placeholder="MM / YY"
        prepend-inner-icon="mdi-calendar"
        outlined
        class="mx-1"
      ></v-text-field>
      <v-text-field
        v-model="card_security_code"
        v-mask="`###`"
        :rules="rulesSecurityCode"
        label="Security Code"
        validate-on-blur
        name="card_security_code"
        prepend-inner-icon="mdi-lock"
        outlined
        class="mx-1"
      ></v-text-field>
    </div>
  </v-form>
</template>

<script>
import { mask } from "vue-the-mask";

export default {
  directives: {
    mask
  },
  data() {
    return {
      valid: true,
      card_holder_name: "",
      cardNumberMasked: "",
      expiryDateMasked: "",
      card_security_code: "",
      cardMask: {
        mask: [`CCCC CCCC CCCC CCCC`],
        tokens: {
          C: { pattern: /(\d|\*)/ }
        }
      },
      rulesCardHolderName: [
        v => !!v || "required",
        v => /.{3,49}/.test(v) || "mustBe3to49",
        v =>
          // eslint-disable-next-line
          /^[a-zA-Z\s\.~'`-]{3,49}$/.test(v) || "invalid-format"
      ],
      rulesCardNumber: [
        v => !!v || "required",
        v => /.{19}/.test(v) || "incomplete"
      ],
      rulesExpiryDate: [
        v => !!v || "required",
        v => /\d{2}\s\/\s\d{2}/.test(v) || "incomplete",
        v => /^[0-1][1-9]/.test(v) || "invalid-month",
        v => /[2-5][0-9]$/.test(v) || "invalid-year"
      ],
      rulesSecurityCode: [
        v => !!v || "required",
        v => /\d{3}/.test(v) || "mustBe3digits"
      ]
    };
  },
  methods: {
    submit() {
      // validate form
      if (this.$refs.cardForm.validate()) {
        return;
      }
    }
  }
};
</script>

<style>
input#card-number {
  direction: ltr;
  text-align: right;
}
input#card-expiry {
  direction: ltr;
  text-align: right;
}
</style>
