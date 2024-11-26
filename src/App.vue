<template>
  <div>
    <h1>로또 번호 뽑기</h1>
    <div>
      <button @click="generateLottoNumbers">로또 번호 추첨</button>
    </div>

    <!-- 모달 -->
    <div v-if="isModalVisible" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <h2>고정 번호</h2>
        <p>고정 번호: {{ fixedNumbers.join(', ') }}</p>

        <h2>당첨 번호</h2>
        <p>추첨된 번호: {{ generatedNumbers.join(', ') }}</p>
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
/* 전체 컨테이너 스타일 */
.container {
  font-family: 'Arial', sans-serif;
  text-align: center;
  padding: 40px;
  background: #f7f7f7;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin: 50px auto;
}

h1 {
  font-size: 28px;
  color: #333;
  margin-bottom: 20px;
}

.lotto-info p {
  font-size: 18px;
  color: #555;
  margin: 10px 0;
}

.generate-btn {
  background-color: #ff8c00;
  color: white;
  border: none;
  padding: 15px 30px;
  font-size: 18px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.generate-btn:hover {
  background-color: #e07b00;
}

/* 모달 오버레이 */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

/* 모달 콘텐츠 스타일 */
.modal-content {
  background-color: #fff;
  padding: 40px;
  border-radius: 10px;
  text-align: center;
  width: 300px;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.1);
}

h2 {
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
}

.winning-numbers {
  font-size: 22px;
  color: #ff8c00;
  font-weight: bold;
  margin-bottom: 20px;
}

.close-btn {
  background-color: #ff8c00;
  color: white;
  border: none;
  padding: 12px 24px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.close-btn:hover {
  background-color: #e07b00;
}
</style>
