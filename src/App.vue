<template>
  <div>
    <h1>로또 번호 뽑기</h1>
    <div>
      <p>고정 번호: {{ fixedNumbers.join(', ') }}</p>
      <p>추첨된 번호: {{ generatedNumbers.join(', ') }}</p>
    </div>
    <button @click="generateLottoNumbers">로또 번호 추첨</button>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'Lotto',
  setup() {
    // 고정 번호 (예: 5번, 12번)
    const fixedNumbers = ref([5, 12])

    // 고정 번호를 제외한 나머지 번호 (1부터 45까지)
    const availableNumbers = ref(
      Array.from({ length: 45 }, (_, i) => i + 1).filter(
        (num) => !fixedNumbers.value.includes(num),
      ),
    )

    // 랜덤으로 뽑힌 번호
    const generatedNumbers = ref([])

    // 로또 번호 추첨 함수
    const generateLottoNumbers = () => {
      // 고정 번호 2개를 제외한 4개 번호를 랜덤으로 추첨
      const remainingNumbers = [...availableNumbers.value]
      const randomNumbers = []

      for (let i = 0; i < 4; i++) {
        const randomIndex = Math.floor(Math.random() * remainingNumbers.length)
        randomNumbers.push(remainingNumbers.splice(randomIndex, 1)[0])
      }

      // 고정 번호와 추첨된 번호 합치기
      generatedNumbers.value = [...fixedNumbers.value, ...randomNumbers]
      generatedNumbers.value.sort((a, b) => a - b) // 번호 오름차순 정렬
    }

    return {
      fixedNumbers,
      generatedNumbers,
      generateLottoNumbers,
    }
  },
}
</script>
