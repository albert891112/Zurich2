<script setup>
import { onMounted, ref } from "vue";

const input = ref("");
const sysToken = ref("");

const callApi = async () => {
  try {
    const response = await fetch(
      "https://zurichapiuat-api-staging-d2awcmfxhmedg9a4.southeastasia-01.azurewebsites.net/api/v1/Auth/entry",
      {
        method: "POST",
        body: JSON.stringify({
          MsToken: input.value,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${sysToken.value}`,
        },
        redirect: "follow",
        mode:"cors"
      }
    );
    const data = await response.text();
    console.log(data);
    document.write(data);
    
  } catch (error) {
    console.error(error);
  }
};

onMounted(async () => {
  try {
    const response = await fetch(
      "https://zurichapiuat-hdd8ejd7hfeeeea6.southeastasia-01.azurewebsites.net/api/v1/Auth/SysLogin",
      {
        method: "POST",
        headers: {
          client_id: "WFPPublicClient",
        },
      }
    );
    const data = await response.json();
    console.log(data);
    sysToken.value = data.access_token;
  } catch (error) {
    console.error(error);
  }
});
</script>

<template>
  <main>
    <input v-model="input" />
    <button @click="callApi">Submit</button>
  </main>
</template>
