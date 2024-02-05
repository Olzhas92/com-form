<template>
  <div>
    <a-button type="primary" @click="openModal">Open Modal</a-button>
    <a-modal v-model:open="open" title="Заявка" :footer="null">
      <a-form
        :model="formState"
        @submit.prevent="submitHandle"
        :label-col="{ span: 8 }"
        :wrapper-col="{ span: 15 }"
        autocomplete="off"
      >
        <a-form-item
          label="Телефон"
          name="Phone"
          :rules="[
            {
              required: true,
              message:
                formState.phone.trim().length !== 15 &&
                'Введите номер телефона!',
              trigger: 'change',
            },
          ]"
        >
          <vue-tel-input
            v-model="formState.phone"
            :autoDefaultCountry="true"
            :validCharactersOnly="true"
            :autoFormat="true"
            autoComplete="off"
            :dropdownOptions="dropdownOptions"
            :inputOptions="inputOptions"
          ></vue-tel-input>
        </a-form-item>

        <a-form-item
          label="Ссылка на объект"
          name="reference"
          v-for="(ref, index) in formState.reference"
          :key="index"
          :rules="[
            {
              required: true,
              message: 'Добавьте ссылку на ЖК',
              trigger: 'change',
            },
          ]"
        >
          <a-input
            v-model:value="formState.reference[index]"
            placeholder="http:// или https://"
          />
        </a-form-item>

        <a-form-item :wrapper-col="{ span: 16, offset: 8 }">
          <a-button type="dashed" danger ghost>Удалить ссылку</a-button>
          <a-button
            style="margin-left: 8px"
            @click="addInput"
            type="primary"
            ghost
            >Добавить ссылку</a-button
          >
        </a-form-item>
        <a-form-item
          label="Комментарии"
          name="comments"
          :rules="[{ message: 'Оставьте комментарий', trigger: 'change' }]"
        >
          <a-textarea
            v-model:value="formState.comments"
            placeholder="Оставьте комментарий"
          />
        </a-form-item>
        <a-form-item :wrapper-col="{ span: 14, offset: 12 }">
          <a-button danger @click="resetHandle">Отменить</a-button>
          <a-button
            type="primary"
            style="margin-left: 12px"
            :disabled="
              formState.phone.trim().length !== 15 && !formState.reference
            "
            html-type="submit"
            >Отправить</a-button
          >
        </a-form-item>
      </a-form>
    </a-modal>
  </div>
</template>

<script setup>
import { ref, reactive } from "vue";

const open = ref(false);
const formState = reactive({
  phone: "",
  reference: [""],
  comments: "",
});

const dropdownOptions = {
  showFlags: false,
  showSearchBox: false,
  disabled: true,
};

const inputOptions = {
  type: "tel",
  required: true,
  placeholder: "Введите номер телефона",
  showDialCode: true,
};

const openModal = () => {
  open.value = true;
};

const resetHandle = () => {
  formState.phone = "+7";
  formState.reference = [""];
  formState.comments = "";
  open.value = true;
};

const submitHandle = () => {
  if (formState.reference[0] !== "" && formState.phone.trim().length === 15) {
    console.log(formState);
    open.value = false;
    formState.phone = "";
    formState.reference = [""];
    formState.comments = "";
    open.value = false;
  } else {
    open.value = true;
    console.log(`Phone number ${formState.phone} is invalid `);
  }
};

const addInput = () => {
  formState.reference.length++;
};
</script>

<style lang="scss" scoped></style>
