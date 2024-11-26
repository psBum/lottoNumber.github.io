<template>
  <div>
    <h1>로또 번호 뽑기</h1>
    <div>
      <p>고정 번호: {{ fixedNumbers.join(', ') }}</p>
      <p>추첨된 번호: {{ generatedNumbers.join(', ') }}</p>
    </div>
    <button @click="generateLottoNumbers">로또 번호 추첨</button>

    <!-- 모달 -->
    <div v-if="isModalVisible" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <h2>당첨 번호</h2>
        <p>{{ generatedNumbers.join(', ') }}</p>
        <button @click="closeModal">닫기</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'LottoApp',
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

    // 모달 표시 여부
    const isModalVisible = ref(false)

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

      // 모달 표시
      isModalVisible.value = true
    }

    // 모달 닫기 함수
    const closeModal = () => {
      isModalVisible.value = false
    }

    return {
      fixedNumbers,
      generatedNumbers,
      generateLottoNumbers,
      isModalVisible,
      closeModal,
    }
  },
}
</script>

<style scoped>
h1 {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.modal-content button {
  margin-top: 20px;
  padding: 8px 16px;
  font-size: 14px;
}
</style>
