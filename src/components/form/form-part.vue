<template>
  <form action="" class="form-sec" @submit.prevent="submitForm">
    <div class="row">
      <div class="form-sec__group">
        <label for="cardholderName" class="form-label form-sec__label"
          >cardholder name</label
        >
        <input
          type="text"
          class="form-control form-sec__control"
          :class="
            v.form.cardholderName.$error
              ? 'form-sec__control--fail'
              : state.form.cardholderName != null &&
                state.form.cardholderName != ''
              ? 'form-sec__control--success'
              : ''
          "
          id="cardholderName"
          placeholder="e.g. Aya Mahdy"
          v-model="state.form.cardholderName"
          @blur="v.form.cardholderName.$touch"
          @input="v.form.cardholderName.$touch"
        />
        <div class="form-sec__error" v-if="v.form.cardholderName.$error">
          {{ v.form.cardholderName.$errors[0].$message }}
        </div>
      </div>
    </div>
    <div class="row">
      <div class="form-sec__group">
        <label for="cardholderNum" class="form-label form-sec__label"
          >card number</label
        >

        <input
          type="text"
          class="form-control form-sec__control"
          :class="
            v.form.cardholderNum.$error
              ? 'form-sec__control--fail'
              : state.form.cardholderNum != null &&
                state.form.cardholderNum != ''
              ? 'form-sec__control--success'
              : ''
          "
          v-model="state.cardholderNum.num"
          @blur="v.form.cardholderNum.$touch"
          id="cardholderNum"
          placeholder="eg. 1234 5678 9101 1112"
          @input="takeSpace(), v.form.cardholderNum.$touch()"
        />
        <div class="form-sec__error" v-if="v.form.cardholderNum.$error">
          {{ v.form.cardholderNum.$errors[0].$message }}
        </div>
      </div>
    </div>
    <div class="row">
      <div class="form-sec__group">
        <label for="cardholderNum" class="form-label form-sec__label"
          >issue date</label
        >

        <DatePicker
          mode="date"
          v-model="state.form.date"
          :columns="1"
          @blur="v.form.date.$touch"
          @input="v.form.date.$touch"
          color="purple"
          :max-date="state.maxDate"
          :masks="{ input: 'MM/YYYY' }"
        >
          <template v-slot="{ inputValue, inputEvents }">
            <input
              class="form-control form-sec__control"
              :value="inputValue"
              v-on="inputEvents"
              readonly
              placeholder="Issue Date"
            />
          </template>
        </DatePicker>
        <div class="form-sec__error" v-if="v.form.date.$error">
          Can't be blank
        </div>
      </div>
    </div>
    <div class="row gx-2">
      <div class="col-md-6 col-7">
        <div class="form-sec__group">
          <label for="cardholderMonth" class="form-label form-sec__label"
            >exp. date (mm / yy)</label
          >
          <div class="row gx-2">
            <div class="col-6">
              <input
                type="number"
                class="form-control form-sec__control"
                id="cardholderMonth"
                placeholder="MM"
                :class="
                  v.form.month.$error
                    ? 'form-sec__control--fail'
                    : state.form.month != null && state.form.month != ''
                    ? 'form-sec__control--success'
                    : ''
                "
                v-model="state.form.month"
                @blur="v.form.month.$touch"
                @input="v.form.month.$touch"
              />
            </div>
            <div class="col-6">
              <input
                type="number"
                class="form-control form-sec__control"
                id="cardholderYear"
                placeholder="YY"
                v-model="state.form.year"
                :class="
                  v.form.year.$error
                    ? 'form-sec__control--fail'
                    : state.form.year != null && state.form.year != ''
                    ? 'form-sec__control--success'
                    : ''
                "
                @blur="v.form.year.$touch"
                @input="v.form.year.$touch"
              />
            </div>
          </div>
          <div class="form-sec__error" v-if="v.form.month.$error">
            {{ v.form.month.$errors[0].$message }}
          </div>

          <div
            v-if="v.form.year.$error && !v.form.month.required.$invalid"
            class="form-sec__error"
          >
            {{ v.form.year.$errors[0].$message }}
          </div>
        </div>
      </div>
      <div class="col-md-6 col-5">
        <div class="form-sec__group">
          <label for="cardholderCVC" class="form-label form-sec__label"
            >cvc</label
          >
          <input
            type="number"
            class="form-control form-sec__control"
            id="cardholderCVC"
            placeholder="e.g. 123"
            :class="
              v.form.cvc.$error
                ? 'form-sec__control--fail'
                : state.form.cvc != null && state.form.cvc != ''
                ? 'form-sec__control--success'
                : ''
            "
            v-model="state.form.cvc"
            @blur="v.form.cvc.$touch"
            @input="v.form.cvc.$touch"
          />
          <div class="form-sec__error" v-if="v.form.cvc.$error">
            {{ v.form.cvc.$errors[0].$message }}
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-12">
        <button class="form-sec-btn" type="submit">confirm</button>
      </div>
    </div>
  </form>
</template>
<script>
import { reactive, watch, computed } from "vue";
import useVuelidate from "@vuelidate/core";
import { DatePicker } from "v-calendar";
import "v-calendar/dist/style.css";
import dayjs from "dayjs";
import preParsePostFormat from "dayjs/plugin/preParsePostFormat";
dayjs.extend(preParsePostFormat);
import {
  required,
  helpers,
  numeric,
  minLength,
  maxLength,
  minValue,
} from "@vuelidate/validators";
export default {
  components: {
    DatePicker,
  },
  emits: [
    "getCardNumVal",
    "getCardNameVal",
    "getCardYearVal",
    "getCardMonthVal",
    "getCardCVCVal",
  ],

  setup(props, context) {
    const state = reactive({
      form: {},
      cardholderNum: {},
      maxMonth: 0,
      maxYear: 0,
      maxDate: null,
    });
    const rules = computed(() => {
      return {
        form: {
          cardholderName: {
            required: helpers.withMessage("Can't be blank", required),
          },
          cardholderNum: {
            required: helpers.withMessage("Can't be blank", required),
            numeric: helpers.withMessage("Wrong format, number only", numeric),
            minLength: helpers.withMessage(
              "Wrong format, must be 16 number",
              minLength(16)
            ),
            maxLength: helpers.withMessage(
              "Wrong format, must be 16 number",
              maxLength(16)
            ),
          },
          month: {
            required: helpers.withMessage("Can't be blank", required),
            maxValue: helpers.withMessage(
              `Month must be greater than or equal  ${state.maxMonth}`,
              minValue(state.maxMonth)
            ),
          },
          year: {
            required: helpers.withMessage("Can't be blank", required),
            maxValue: helpers.withMessage(
              `Year must be greater than or equal ${state.maxYear} `,
              minValue(state.maxYear)
            ),
          },
          date: { required },
          cvc: {
            required: helpers.withMessage("Can't be blank", required),
            minLength: helpers.withMessage(
              "Wrong format, must be 3 number",
              minLength(3)
            ),
          },
        },
      };
    });
    watch(state.cardholderNum, (val) => {
      state.form.cardholderNum = val.num.replaceAll(" ", "");
    });
    watch(state.form, (val) => {
      if (val.cardholderName != null && val.cardholderName != "") {
        context.emit("getCardNameVal", val.cardholderName);
      } else {
        context.emit("getCardNameVal", "Aya Mahdy");
      }
      if (val.cardholderNum != null && val.cardholderNum != "") {
        context.emit("getCardNumVal", val.cardholderNum);
      } else {
        context.emit("getCardNumVal", "0000 0000 0000 0000");
      }
      if (val.month != null && val.month != "") {
        context.emit("getCardMonthVal", val.month);
        let year = Number("20" + state.form.year);
        let month = Number(state.form.month) - 2;

        state.maxDate = new Date(year, month);
      } else {
        context.emit("getCardMonthVal", "00");
      }
      if (val.year != null && val.year != "") {
        context.emit("getCardYearVal", val.year);
        let year = Number("20" + state.form.year);
        let month = Number(state.form.month) - 2;
        state.maxDate = new Date(year, month);
      } else {
        context.emit("getCardYearVal", "00");
      }
      if (val.cvc != null && val.cvc != "") {
        context.emit("getCardCVCVal", val.cvc);
      } else {
        context.emit("getCardCVCVal", "000");
      }
      if (val.date != null && val.date != "") {
        state.maxMonth = Number(dayjs(val.date).format("MM")) + 1;
        state.maxYear = Number(dayjs(val.date).format("YY")) + 1;
      }
    });

    const takeSpace = () => {
      state.cardholderNum.num = formatNumber(
        state.cardholderNum.num.replaceAll(" ", "")
      );
    };

    const formatNumber = (number) =>
      number.split("").reduce((seed, next, index) => {
        if (index !== 0 && !(index % 4)) seed += " ";
        return seed + next;
      }, "");

    const v = useVuelidate(rules, state);
    return {
      state,
      takeSpace,
      rules,

      v,
    };
  },
  methods: {
    submitForm() {
      this.v.$validate();
      if (!this.v.$error) {
        this.$emit("getIsSubmittedVal");
      }
    },
  },
};
</script>
