<template>
  <div class="container-fluid gx-0">
    <div class="row gx-0">
      <aside class="col-lg-6 col-md-12 col-12">
        <aside-part
          :cardInputName="state.cardInputName"
          :cardInputNum="state.cardInputNum"
          :cardInputYear="state.cardInputYear"
          :cardInputMonth="state.cardInputMonth"
          :cvcInput="state.cvcInput"
          :key="state.key"
        ></aside-part>
      </aside>
      <main class="col-lg-6 col-md-12 col-12 form-part">
        <form-part v-if="!state.isSubmitted"
          @getCardNameVal="getCardNameVal($event)"
          @getCardNumVal="getCardNumVal($event)"
          @getCardCVCVal="getCardCVCVal"
          @getCardYearVal="getCardYearVal"
          @getCardMonthVal="getCardMonthVal"
          @getIsSubmittedVal="getIsSubmittedVal"
        ></form-part>
        <complete-state v-else></complete-state>
      </main>
    </div>
  </div>
</template>

<script>
import AsidePart from "./components/aside/aside-part.vue";
import FormPart from "./components/form/form-part.vue";
import CompleteState from "./components/complete-state/complete-state.vue";
import { reactive } from "vue";
export default {
  name: "App",
  components: { AsidePart, FormPart, CompleteState },
  setup() {
    const state = reactive({
      cardInputNum: "0000 0000 0000 0000",
      cardInputName: "Aya Mahdy",
      cardInputMonth: "00",
      cardInputYear: "00",
      cvcInput: "000",
      key: 1,
      isSubmitted : false
    });
    const getCardNumVal = (val) => {
      state.cardInputNum = val;
      state.key += 1;
    };
    const getCardNameVal = (val) => {
      state.cardInputName = val;
      state.key += 1;
    };
    const getCardCVCVal = (val) => {
      state.cvcInput = val;
      state.key += 1;
    };
    const getCardMonthVal = (val) => {
      state.cardInputMonth = val;
      state.key += 1;
    };
    const getCardYearVal = (val) => {
      state.cardInputYear = val;
      state.key += 1;
    };
    const getIsSubmittedVal = () =>{
      alert("")
      state.isSubmitted = true
      state.key += 1;

    }
    return {
      state,
      getCardNumVal,
      getCardNameVal,
      getCardCVCVal,
      getCardMonthVal,
      getCardYearVal,
      getIsSubmittedVal,
      
    };
  },
};
</script>
<!-- Main style -->
<style lang="scss">
@import "assets/styles/main.scss";
</style>
