<template>
  <div class="container">
    <h1>Поиск пользователя GitHub</h1>
    <Input v-model="userName" :define="define" />
    

    <div class="user-details">
      <h4 v-if="error" class="error">{{ error }}</h4>
      <h4 v-if="isLoading" >Ищем пользователя...</h4>
      <div v-if="userData" class="user-card">
        <img :src="userData.avatar_url" alt="Аватар" class="avatar">
        <div class="user-info">
          <h2>{{ userData.name }} <a :href="userData.html_url" target="_blank">(@{{ userData.login }})</a></h2>
          <p class="bio">{{ userData.bio || 'Нет информации в биографии.' }}</p>
          <ul>
            <li><strong>Репозиториев:</strong> {{ userData.public_repos }}</li>
            <li><strong>Подписчиков:</strong> {{ userData.followers }}</li>
            <li><strong>Подписок:</strong> {{ userData.following }}</li>
          </ul>
        </div>
    </div>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios';
import Input from './components/Input.vue';
import { ref } from 'vue';

const userName = ref('');
const userData = ref(null);
const error = ref(null);
const isLoading = ref(null);

async function define() {
  if(!userName.value) {
    error.value = 'Введите пользователя.';
    return;
  }

  error.value = '';
  isLoading.value = true;
  userData.value = null;


  try {
    const responce = await axios.get(`https://api.github.com/users/${userName.value}`);

    userData.value = responce.data;
  } catch(e) {
    if(e.response && e.response.status === 404) {
      error.value = `Пользователь с ником ${userName.value} не найден.`;
    } else {
      error.value = 'Произошла ошибка. Повторите позже.';
      console.log(e);
    }
  } finally {
    isLoading.value = false;
  }
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  font-family: sans-serif;
  background-color: #0d1117;
  border-radius: 10px;
  border: 1px solid #3d444d;
  text-align: center;
}
.user-card {
  display: flex;
  text-align: left;
  gap: 20px;
  margin-top: 40px;
  padding: 20px;
  border: 1px solid #3d444d;
  border-radius: 6px;
  background-color: #171c22;
}
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
}
.user-info h2 {
  margin: 0;
}
.user-info h2 a {
  text-decoration: none;
  color: #1f6feb;
}
.bio {
  margin: 10px 0;
  color: #586069;
}
ul {
  list-style: none;
  padding: 0;
  display: flex;
  gap: 15px;
}
.error {
  margin-top: 30px;
  color: #cf222e;
}

@media (max-width: 450px) {
  .container {
    width: 400px;
    margin-left: 54px;
    height: 130px;
  }
  h1 {
    font-size: 23px;
  }
  h2 {
    font-size: 21px;
  }
  .bio {
    font-size: 14px;
    text-wrap: wrap;
    max-width: 265px;
  }
  ul {
    font-size: 7px;
    position: relative;
    left: -87px;
    top: 20px;
    padding-bottom: 28px;
  }
  .user-card {
    margin-top: 100px;
  }
}

@media (max-width: 375px) {
  .container {
    width: 400px;
    margin-left: 68px;
    height: 130px;
  }
  h1 {
    font-size: 23px;
  }
  h2 {
    font-size: 21px;
  }
  .bio {
    font-size: 14px;
    text-wrap: wrap;
    max-width: 265px;
  }
  ul {
    font-size: 7px;
    position: relative;
    left: -87px;
    top: 20px;
    padding-bottom: 28px;
  }
  .user-card {
    margin-top: 50px;
  }
  .error {
    margin-top: 45px;
  }
}
</style>
