<template>
  <div class="row gx-0">
    <div class="col-lg-7 col-md-12">
      <div class="card">
        <div class="cards">
          <div class="card__front">
            <header class="card__front__header">
              <div class="card__front__header__lg-circle"></div>
              <div class="card__front__header__sm-circle"></div>
            </header>
            <main>
              <p class="card__front__text">{{ state.cardNum }}</p>
            </main>
            <div class="card__front__footer">
              <p>{{ state.cardName }}</p>
              <p>{{ state.cardMonth }} / {{ state.cardYear }}</p>
            </div>
          </div>
          <div class="card__back">
            <header class="card__back__header"></header>
            <main class="card__back__body">
              <div class="card__back__body__cvc">
                {{ state.cvc }}
              </div>
              <div class="card__back__body__first-line">
                <div></div>
                <div></div>
                <div></div>
                <div></div>
              </div>
              <div class="card__back__body__second-line">
                <div></div>
                <div></div>
                <div></div>
                <div></div>
              </div>
              <div class="card__back__body__third-line">
                <div></div>
                <div></div>
                <div></div>
                <div></div>
              </div>
            </main>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { reactive } from "vue";
export default {
  props: [
    "cardInputNum",
    "cardInputName",
    "cardInputMonth",
    "cvcInput",
    "cardInputYear",
  ],
  setup(props) {
    const state = reactive({
      cardName: props.cardInputName,
      cardNum: props.cardInputNum,
      cardMonth: props.cardInputMonth,
      cardYear: props.cardInputYear,
      cvc: props.cvcInput,
    });

    return {
      state,
    };
  },
};
</script>
