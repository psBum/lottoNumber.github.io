<template>
  <div class="about">
    <button @click="lottoNumber">로또 번호 생성</button>
    <div v-if="lottoNumbers.length > 0">
      <h2>생성된 번호</h2>
      <p>{{ lottoNumbers.join(', ') }}</p>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    // 로또 번호를 저장할 배열
    const lottoNumbers = ref([])

    // 로또 번호 생성 함수
    const lottoNumber = () => {
      let numbers = new Set()

      // 6개의 고유한 숫자 생성 (1부터 45까지)
      while (numbers.size < 6) {
        const randomNumber = Math.floor(Math.random() * 45) + 1
        numbers.add(randomNumber) // Set은 중복을 허용하지 않음
      }

      // Set을 배열로 변환하여 lottoNumbers에 저장
      lottoNumbers.value = Array.from(numbers)
    }

    // 반환된 값은 템플릿에서 사용할 수 있도록 설정
    return {
      lottoNumbers,
      lottoNumber,
    }
  },
}
</script>

<style scoped>
#app {
  text-align: center;
  font-family: Arial, sans-serif;
  margin-top: 50px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
}

button:hover {
  background-color: #45a049;
}

h1 {
  font-size: 24px;
}

h2 {
  font-size: 20px;
}

p {
  font-size: 18px;
  font-weight: bold;
}
</style>
