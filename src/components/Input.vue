<template>
    <div class="wrapper">
        <div class="input-container">
            <i class="fa-solid fa-magnifying-glass"></i>
            <input 
                type="text"
                :value="modelValue"
                @input="handleInput"
                @keydown.enter="define()"
                placeholder="Найти пользователя..."
            >
        </div><br>
        <button @click="define()">Найти пользователя</button>
    </div>
</template>

<script setup>
import { onMounted } from 'vue';

defineProps({
    modelValue: String,
    define: Function,
});

const emit = defineEmits(['update:modelValue']);

onMounted(() => {
  console.log('Компонент Input.vue УСПЕШНО ЗАГРУЖЕН.');
});

function handleInput(event) {
  if (event.target) {
    console.log('Значение в инпуте (event.target.value):', event.target.value);
  } else {
    console.error('КРИТИЧЕСКАЯ ОШИБКА: event.target не существует!');
  }
  
  emit('update:modelValue', event.target.value); 
  
}
</script>

<style scoped>
.wrapper {
    display: flex;
    flex-wrap: wrap;
}

.input-container {
    color: #f0f6fc;
    vertical-align: middle;
    background-color: #0d1117;
    background-position: right 8px center;
    border: solid 1px #3d444d;
    border-radius: 6px;
    width: 100%;
    display: flex;
    align-items: baseline;
}

input {
    padding: 5px 25px;
    font-size: 14px;
    line-height: 20px;
    color: #f0f6fc;
    vertical-align: middle;
    background-color: #0d1117;
    background-position: right 8px center;
    border: 0;
    border-radius: 6px;
    width: 100%;
    outline: 0;
}

button {
    width: 100%;
    text-align: center;
    color: #f0f6fc;
    background-color: #212830;
    border: solid 1px #3d444d;
    border-radius: 6px;
    cursor: pointer;
    font-size: 14px;
    vertical-align: middle;
    height: 32px;
    position: relative;
    top: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
}

button:hover {
    background-color: #313b47;
}

i {
    position: relative;
    left: 10px;
    font-size: 0.75rem;
}
</style>
