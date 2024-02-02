<template>
  <div>
    <a-button type="primary" @click="openModal">Open Modal</a-button>
    <a-modal v-model:open="open" title="Заявка" :footer="null">
      <a-form
        name="basic"
        ref="formRef"
        :model="formState"
        :label-col="{ span: 8 }"
        :wrapper-col="{ span: 15 }"
      >
        <a-form-item
          label="Телефон"
          name="Phone"
          :rules="[
            {
              required: true,
              message: 'Введите номер телефона!',
              trigger: 'change',
            },
          ]"
          has-feedback
        >
          <vue-tel-input
            v-model="formState.phone"
            placeholder="Введите номер телефона"
            invalidMsg="Добавьте ссылку на ЖК"
            :autoDefaultCountry="true"
            :validCharactersOnly="true"
            :autoFormat="true"
            autocomplete="off"
            :dropdownOptions="dropdownOptions"
            :inputOptions="inputOptions"
          ></vue-tel-input>
        </a-form-item>

        <a-form-item
          label="Ссылка на объект"
          name="reference"
          v-for="item in formState.reference"
          :key="item"
          :rules="[
            {
              required: true,
              message: 'Добавьте ссылку на ЖК',
              trigger: 'change',
            },
          ]"
          has-feedback
        >
          <a-input
            v-model:value="myInput"
            autocomplete="off"
            placeholder="Введите ссылку на ЖК"
            type="url"
            name="url"
            pattern="https://.*"
            size="30"
            required
          />
        </a-form-item>
        <a-form-item :wrapper-col="{ span: 14, offset: 12 }">
          <a-button type="dashed">Удалить</a-button>
          <a-button @click="addInput" type="dashed">Добавить</a-button>
        </a-form-item>
        <a-form-item
          label="Комментарии"
          name="comments"
          :rules="[{ message: 'Оставьте комментарий', trigger: 'change' }]"
          has-feedback
        >
          <a-textarea
            v-model:value="formState.comments"
            autocomplete="off"
            placeholder="Оставьте комментарий"
          />
        </a-form-item>
        <a-form-item :wrapper-col="{ span: 14, offset: 12 }">
          <a-button danger @click="resetHandle">Отменить</a-button>
          <a-button
            type="primary"
            style="margin-left: 12px"
            :disabled="
              formState.phone === '' || formState.reference.length === 0
            "
            @click="submitHandle"
            >Отправить</a-button
          >
        </a-form-item>
      </a-form>
    </a-modal>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from "vue";

const open = ref(false);
const formComplete = ref(false);
const myInput = ref("");
const formState = reactive({
  phone: "",
  reference: [""],
  comments: "",
});
const formRef = ref();

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
  formState.phone = "";
  formState.reference = [];
  formState.comments = "";
  formRef.value = null;
  open.value = true;
};

const submitHandle = () => {
  open.value = false;
  if (formState.reference.length > 0) {
    formComplete.value = true;
  }
  formState.reference.push(myInput.value);
  console.log(formState);
};

const addInput = () => {
  formState.reference.push(myInput.value);
  console.log(formState.reference);
  console.log(formState);
};
</script>

<style lang="scss" scoped></style>
