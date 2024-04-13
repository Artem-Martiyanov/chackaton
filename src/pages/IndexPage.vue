<script setup lang="ts">
import { ref } from 'vue';
import { useQuasar } from 'quasar';

defineOptions({
  name: 'IndexPage'
});


const $q = useQuasar();

const file = ref<File | null>(null);
const isLoading = ref<boolean>(false);
const resultFileUrl = ref<string>('');


const formSubmitHandler = async () => {
  if (!file.value) {
    $q.notify({
      message: 'Ошибка! Не удалось найти файл',
      color: 'negative'
    });
    return;
  }

  try {
    isLoading.value = true;

    const baseURL: string = import.meta.env.VITE_API_URL;

    const data = new FormData();
    data.append('file', file.value);


    $q.notify({
      message: 'Файл уже обрабатывается...',
      color: 'info'
    });


    const response = await fetch(`${baseURL}/file/upload`, {
      method: 'POST',
      body: data
    });

    $q.notify({
      message: 'Братский успешный успех!',
      color: 'positive'
    });

    console.log(response);

    const resultData = await response.json();

    console.log(resultData);
  } catch (e) {
    $q.notify({
      message: `Братья потерпели неудачу... Извините...сь (${JSON.stringify(e)})`,
      color: 'negative'
    });
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <q-page class="row items-center justify-evenly">
    <section class="result" v-if="resultFileUrl">
      <div>
        <q-btn :href="resultFileUrl" label="Скачать результат" target="_blank" icon="download" color="primary" />
      </div>
      <q-btn label="Отправить ещё раз" icon="refresh" dense @click="resultFileUrl = ''" />
    </section>
    <q-form class="form column" @submit.prevent="formSubmitHandler" v-else>
      <q-uploader
        label="Загрузите фалй с датасетом"
        @added="(files) => file = files[0]"
        accept=".csv"
        :disable="isLoading"
      />
      <q-btn label="Отправить" color="primary" :loading="isLoading" type="submit" :disable="!file" />
    </q-form>
  </q-page>
</template>


<style lang="scss" scoped>

.form {
  border: 1px solid var(--q-primary);
  padding: 2.5rem;
  border-radius: 5px;
  box-shadow: 3px 3px 10px rgba(29, 29, 29, 0.5);
  display: flex;
  flex-direction: column;
  gap: 15px;

}

.result {
  display: flex;
  flex-direction: column;
  gap: 15px;
}


.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  border: 0;
  clip: rect(0 0 0 0);
}

</style>
