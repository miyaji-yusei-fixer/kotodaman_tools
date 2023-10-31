<template>
  <v-container>
    <v-col class="mx-2">
      <v-row>
        <v-text-field
          id="banmenForm"
          :value="banmenValue"
          @input="inputBanmen"
          :rules="banmenRules"
          label="盤面"
          required
        />
      </v-row>
      {{ banmenValue.length }}{{ cursor }} {{ banmenClicked }}
      <v-row class="text-center mb-1">
        <v-btn
          v-if="!banmenValue"
          class="mx-1"
          flat
          style="background-color: yellow"
          size="x-large"
          icon="mdi-account"
          @click="clickBanmen(0)"
        ></v-btn>
        <v-btn
          class="mx-1"
          v-for="(moji, i) in banmenValue"
          :key="`search-${i}`"
          flat
          :style="
            cursor === i
              ? 'background-color: yellow'
              : 'background-color: black'
          "
          :color="cursor === i ? 'black' : 'white'"
          size="x-large"
          icon="mdi-account"
          @click="clickBanmen(i)"
          >{{ moji }}</v-btn
        >
      </v-row>
      <v-row>
        <v-radio-group v-model="mode" row>
          <v-radio label="含" value="center"></v-radio>
          <v-radio label="始" value="right"></v-radio>
          <v-radio label="終" value="left"></v-radio>
          <v-radio label="中含" value="centerOnly"></v-radio>
          {{ mode }}
        </v-radio-group>
      </v-row>
      <v-row>
        <v-radio-group v-model="length" row>
          <!-- <v-radio label="0" :value="0"></v-radio> -->
          <v-radio label="2" :value="2"></v-radio>
          <v-radio label="3" :value="3"></v-radio>
          <v-radio label="4" :value="4"></v-radio>
          <v-radio label="5" :value="5"></v-radio>
          <v-radio label="6" :value="6"></v-radio>
          <v-radio label="7" :value="7"></v-radio>
          <!-- {{ length }} -->
        </v-radio-group>
      </v-row>
      <v-row class="mb-4">
        <v-btn :disabled="isDisabledSearch" @click="search">けんさく</v-btn>
      </v-row>
      <v-row class="text-center mb-1">
        <v-btn
          class="mx-1"
          v-for="(moji, i) in searchValue"
          :key="`search-${i}`"
          flat
          style="background-color: black"
          color="white"
          size="x-large"
          icon="mdi-account"
          >{{ moji }}</v-btn
        >
      </v-row>
      <v-row class="text-center mb-1">
        <v-btn
          class="mx-1"
          v-for="(moji, i) in searchValue"
          :key="`search-${i}`"
          flat
          style="background-color: black"
          color="white"
          size="x-large"
          icon="mdi-account"
          >{{ moji }}</v-btn
        >
      </v-row>
    </v-col>
  </v-container>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
const cursor = ref(-1);
const banmenClicked = ref(false);
const clickBanmen = (i) => {
  console.log("やー", i);
  banmenClicked.value = true;
  const obj = document.getElementById("banmenForm");
  const val = i + 1;
  cursor.value = i;
  obj.focus();
  obj.setSelectionRange(val, val);
};
const banmenValue = ref("");
const inputBanmen = (value) => {
  // eslint-disable-next-line no-irregular-whitespace
  if (value.match(/^[ぁ-ん 　]*$/)) {
    const length = value.length - banmenValue.value.length;
    if (!banmenClicked.value) {
      banmenValue.value = value;
      cursor.value += length;
    } else {
      if (banmenValue.value.length > cursor.value) {
        banmenValue.value =
          banmenValue.value.slice(0, cursor.value) +
          value[cursor.value + 1] +
          banmenValue.value.slice(cursor.value + 1);
        cursor.value += length;
        clickBanmen(cursor.value);
      } else {
        banmenValue.value = value;
        cursor.value += length;
      }
    }
  }

  // const obj = document.getElementById("banmenForm");
};
const banmenRules = [
  (v) => !!v || "もじを入力してください。",
  (v) =>
    // eslint-disable-next-line no-irregular-whitespace
    v.match(/^[ぁ-ん 　]*$/) || "ひらがなか空白を入力してください。",
];
const mode = ref("center");
const length = ref(0);
const searchValue = ref("");
const isDisabledSearch = computed(
  // eslint-disable-next-line no-irregular-whitespace
  () => !banmenValue.value || !banmenValue.value.match(/^[ぁ-ん 　]*$/)
);
const search = () => {
  searchValue.value = banmenValue.value;
};
</script>
