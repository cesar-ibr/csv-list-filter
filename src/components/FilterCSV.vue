<script setup>
import { ref } from 'vue';

const disableBtn = ref(false);
const contacts = {
  all: [],
  sent: [],
};

const handleFileSelect = (files = [], loader = str => str) => {
  const [file] = files;
  if (!file) { return; }

  const reader = new FileReader();
  reader.addEventListener('load', () => loader(reader.result));
  reader.readAsText(file);
};

const loadAllContacts = (fileStr = '') => {
  const allRows = fileStr.split('\n').filter(Boolean).slice(1);
  console.log('All Contacts: ', allRows.length);

  contacts.all = allRows.map(r => {
    const [name, sName, email] = r.split(',');
    const lastName = sName ? ` ${sName}` : '';
    return {
      name: name.concat(lastName),
      email,
    };
  });
};

const loadSentContacts = (fileStr = '') => {
  const allRows = fileStr.split('\n').filter(Boolean).slice(1);
  console.log('Sent Contacts: ', allRows.length);

  contacts.sent = allRows.map(r => {
    const [name, email] = r.split(',');
    return { name, email};
  });
};

const getFilteredContacts = () => {
  const {all, sent} = contacts;
  return all
    .filter(contact => !(sent.find(c => c.email === contact.email)));
};

const filterButtonHandler = () => {
  disableBtn.value = true;
  const filteredContacts = getFilteredContacts().map(c => `${c.name},${c.email}\n`);
  console.log('Skipped Contacts: ', filteredContacts.length);

  const blob = new Blob(filteredContacts, { type: 'text/csv;charset=utf-8;' });
  const url = URL.createObjectURL(blob);

  const link = document.createElement('a');
  link.href = url;
  link.setAttribute('download', 'skipped_contacts.csv');
  link.click();
  disableBtn.value = false;
};



</script>

<template>
<div class="container">
  <h1>Obtener Lista de No Enviados</h1>
  <label for="file-one">
    Lista Completa:
    <input
      id="file-one"
      type="file"
      name="file-one"
      @change="handleFileSelect($event.target.files, loadAllContacts)">
  </label>

  <label for="file-two">
    Contactos Enviados:
    <input
      id="file-two"
      type="file"
      name="file-two"
      @change="handleFileSelect($event.target.files, loadSentContacts)">
  </label>

  <button
    :disabled="disableBtn"
    @click="filterButtonHandler"
  >
    Obtener Lista
  </button>
</div>
</template>

<style scoped>
.container {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  padding: 20px;
  max-width: 800px;
  gap: 30px;
  align-items: center;
  text-align: center;
}

button {
  border-radius: 10px;
  color: #FFF;
  background-color: #d119db;
  border: none;
  padding: 10px;
  width: 200px;
  cursor: pointer;
}

button:disabled {
  background-color: #CCC;
}
</style>
