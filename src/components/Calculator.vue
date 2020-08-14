<template>
  <div class="hello container">
    <h1 class="display-4">MwSt. Rechner</h1>
    <p class="lead mb-4">
      Einfach bekannte Zahl eingeben und der Rechner aktualisiert die fehlenden Felder.
    </p>

    <!--
    <div class="netto form-group row">
      <label for="netto" class="col-sm-2 col-form-label-lg">Netto: </label>
      <div class="col-sm-10">
        <input id="netto" name="netto" class="form-control-lg" type="text" v-model="netto" @change="changeNetto">
      </div>
    </div>
    -->
    <div class="inputs">
      <InputComponent name="netto" :model="netto" @update="netto = $event; changeNetto();"/>
      <InputComponent name="mwst" :model="mwst" @update="mwst = $event; changeMwst();"/>
      <InputComponent name="brutto" :model="brutto" @update="brutto = $event; changeBrutto();"/>
      <button class="btn btn-light" @click="clearFields">Zahlen löschen</button>
      <hr>
      <InputComponent name="steuersatz" :model="steuersatz" @update="steuersatz = $event; changeNetto();"/>
    </div>
    🐇
  </div>
</template>

<script>
// new Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(number)
import InputComponent from "@/components/InputComponent";

export default {
  name: 'Calculator',
  components: {InputComponent},
  data() {
    return {
      netto: "0",
      mwst: "0",
      brutto: "0",
      steuersatz: "16%"
    }
  },
  computed: {
    steuersatz_number() {
      return parseFloat(this.formattedToInteger(this.steuersatz)) / 100;
    },
    netto_number() {
      return parseFloat(this.formattedToInteger(this.netto));
    },
    mwst_number() {
      return parseFloat(this.formattedToInteger(this.mwst));
    },
    brutto_number() {
      return parseFloat(this.formattedToInteger(this.brutto));
    },
  },
  methods: {
    formattedToInteger(formatted) {
      return formatted
          .replace('€', '')
          .replace('.', '')
          .replace(',', '.');
    },
    changeMwst() {
      this.netto = this.roundToTwoDecimals(this.mwst_number / this.steuersatz_number);
      this.brutto = this.roundToTwoDecimals(this.mwst_number / this.steuersatz_number * (1 + this.steuersatz_number));
      this.mwst = new Intl.NumberFormat('de-DE', {style: 'currency', currency: 'EUR'}).format(this.mwst_number);
    },

    changeBrutto() {
      this.netto = this.roundToTwoDecimals((this.brutto_number / (1 + this.steuersatz_number)));
      this.mwst = this.roundToTwoDecimals((this.brutto_number / (1 + this.steuersatz_number)) * this.steuersatz_number);
      this.brutto = new Intl.NumberFormat('de-DE', {style: 'currency', currency: 'EUR'}).format(this.brutto_number);
    },

    changeNetto() {
      this.mwst = this.roundToTwoDecimals(this.netto_number * this.steuersatz_number);
      this.brutto = this.roundToTwoDecimals(this.netto_number * (1 + this.steuersatz_number));
      this.netto = new Intl.NumberFormat('de-DE', {style: 'currency', currency: 'EUR'}).format(this.netto_number);
    },

    roundToTwoDecimals(number) {
      number = Math.round(number * 100) / 100;
      return new Intl.NumberFormat('de-DE', {style: 'currency', currency: 'EUR'}).format(number);
    },

    clearFields() {
      this.netto = '';
      this.brutto = '';
      this.mwst = '';
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
