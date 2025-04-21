<template>
  <div class="min-h-screen bg-gradient-to-br from-indigo-100 to-blue-50 p-6 flex flex-col items-center justify-center">
    <div v-if="loading" class="flex flex-col items-center justify-center space-y-4">
      <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-500"></div>
      <p class="text-gray-600">퀴즈를 불러오는 중...</p>
    </div>

    <div v-else-if="error" class="text-center space-y-4">
      <p class="text-red-500 text-lg">{{ error }}</p>
      <button @click="goToBack" class="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-xl shadow">돌아가기</button>
    </div>

    <div v-else-if="step === 'quiz'" class="w-full max-w-lg space-y-8 animate-slide-in">
      <div class="bg-white rounded-xl shadow p-6">
        <div class="flex justify-between items-center mb-4">
          <h2 class="text-xl font-semibold text-gray-800">다시 풀기: 문제 {{ currentIndex + 1 }} / {{ quizzes.length }}</h2>
          <button @click="goToCalendar" class="text-gray-500 hover:text-gray-700 text-sm">취소</button>
        </div>
        <!-- 출제자 표시 -->
        <p class="text-sm text-gray-500 italic">{{ currentQuiz.question_creator }} 님이 출제하신 문제입니다</p>
        <p class="mt-3 text-lg text-gray-700 whitespace-pre-line">{{ currentQuiz.question }}</p>
      </div>

      <div class="space-y-4">
        <button
          v-for="(option, i) in currentQuiz.options"
          :key="i"
          :class="[
            selectedAnswer !== null ? getOptionClass(i) : 'bg-white hover:bg-gray-100',
            'transition w-full p-4 border rounded-xl text-left shadow-sm',
          ]"
          @click="submitAnswer(i)"
          :disabled="selectedAnswer !== null"
        >
          {{ option }}
        </button>
      </div>

      <transition name="fade">
        <div v-if="selectedAnswer !== null" class="mt-6 text-sm text-gray-600">
          <p v-if="selectedAnswer === currentQuiz.answer" class="text-green-600 font-semibold">🎉 정답입니다!</p>
          <p v-else class="text-red-600 font-semibold">❌ 틀렸습니다! {{ currentQuiz.explanation }}</p>
          <button @click="nextQuestion" class="mt-4 bg-blue-500 hover:bg-blue-600 transition text-white px-4 py-2 rounded-xl shadow">
            다음 문제
          </button>
        </div>
      </transition>
    </div>

    <div v-else-if="step === 'result'" class="text-center space-y-6 animate-fade-in">
      <h2 class="text-3xl font-bold text-gray-800">🎯 퀴즈 완료!</h2>
      <p class="text-xl text-gray-700">총 {{ score }}점 / {{ quizzes.length }}점</p>
      <div class="space-y-4 md:space-y-0 md:space-x-4 flex flex-col md:flex-row justify-center">
        <button @click="loadMoreQuizzes" class="bg-yellow-500 hover:bg-yellow-600 transition text-white px-6 py-3 rounded-xl shadow">더 풀기</button>
        <button @click="goToCalendar" class="bg-blue-500 hover:bg-blue-600 transition text-white px-6 py-3 rounded-xl shadow">
          캘린더로 돌아가기
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';

const route = useRoute();
const router = useRouter();
const supabase = useSupabaseClient();
const user = useSupabaseUser();

// 상태 정의
const loading = ref(true);
const error = ref(null);
const step = ref('quiz');
const quizzes = ref([]);
const currentIndex = ref(0);
const selectedAnswer = ref(null);
const score = ref(0);

// 현재 퀴즈 가져오기
const currentQuiz = computed(() => {
  if (!quizzes.value || quizzes.value.length === 0) return {};
  return quizzes.value[currentIndex.value];
});

// 다시 풀기 대상 퀴즈 ID 가져오기
onMounted(async () => {
  try {
    // 1. 쿼리 파라미터에서 퀴즈 ID 가져오기
    const retryQuizId = route.query.retry;

    if (!retryQuizId) {
      throw new Error('다시 풀기 대상 퀴즈 ID가 없습니다.');
    }

    if (!user.value) {
      throw new Error('로그인이 필요합니다.');
    }

    // 단일 퀴즈만 다시 풀 경우
    if (!Array.isArray(retryQuizId)) {
      await loadQuiz(retryQuizId);
    }
    // 여러 퀴즈를 다시 풀 경우
    else {
      await loadMultipleQuizzes(retryQuizId);
    }

    loading.value = false;
  } catch (err) {
    loading.value = false;
    error.value = err.message || '퀴즈를 불러오는데 실패했습니다.';
  }
});

// 단일 퀴즈 불러오기
async function loadQuiz(quizId) {
  const { data, error: fetchError } = await supabase.from('quizzes').select('*').eq('id', quizId).single();

  if (fetchError) {
    throw new Error('퀴즈를 불러오는데 실패했습니다.');
  }

  if (!data) {
    throw new Error('존재하지 않는 퀴즈입니다.');
  }

  quizzes.value = [data];
}

// 여러 퀴즈 불러오기
async function loadMultipleQuizzes(quizIds) {
  const { data, error: fetchError } = await supabase.from('quizzes').select('*').in('id', quizIds);

  if (fetchError) {
    throw new Error('퀴즈를 불러오는데 실패했습니다.');
  }

  if (!data || data.length === 0) {
    throw new Error('존재하지 않는 퀴즈입니다.');
  }

  // 랜덤하게 순서 섞기
  quizzes.value = data.sort(() => 0.5 - Math.random());
}

// 더 풀기 - 새로운 퀴즈 불러오기
async function loadMoreQuizzes() {
  try {
    loading.value = true;

    // 최근에 풀었던 퀴즈 제외하고 5개 랜덤 선택
    const { data: correctRecords } = await supabase.from('quiz_records').select('quiz_id').eq('user_id', user.value.id).eq('is_correct', true);

    // 맞은 문제 ID 추출
    const correctQuizIds = correctRecords ? correctRecords.map((record) => record.quiz_id) : [];

    // 이미 맞은 문제를 제외한 모든 문제 가져오기
    let query = supabase.from('quizzes').select('*');

    // 맞은 문제가 있으면 해당 문제 제외
    if (correctQuizIds.length > 0) {
      query = query.not('id', 'in', `(${correctQuizIds.join(',')})`);
    }

    const { data } = await query.limit(5);

    if (!data || data.length === 0) {
      error.value = '더 이상 풀 문제가 없습니다.';
      loading.value = false;
      return;
    }

    // 새로운 문제 설정
    quizzes.value = data;
    currentIndex.value = 0;
    selectedAnswer.value = null;
    score.value = 0;
    step.value = 'quiz';
    loading.value = false;
  } catch (err) {
    loading.value = false;
    error.value = err.message || '퀴즈를 불러오는데 실패했습니다.';
  }
}

// 답변 제출 처리
async function submitAnswer(index) {
  selectedAnswer.value = index;
  const isCorrect = index === currentQuiz.value.answer;

  if (isCorrect) {
    score.value++;
  }

  // 기록 저장
  if (user.value) {
    await supabase.from('quiz_records').insert({
      user_id: user.value.id,
      quiz_id: currentQuiz.value.id,
      selected_answer: index,
      is_correct: isCorrect,
      created_at: new Date().toISOString(),
      date: new Date().toISOString().split('T')[0],
    });
  }
}

// 다음 문제로 이동
function nextQuestion() {
  if (currentIndex.value < quizzes.value.length - 1) {
    currentIndex.value++;
    selectedAnswer.value = null;
  } else {
    step.value = 'result';
  }
}

// 캘린더 페이지로 이동
function goToCalendar() {
  router.push('/quiz/records');
}

function goToBack() {
  router.go(-1);
}

// 옵션 클래스 설정
function getOptionClass(i) {
  if (selectedAnswer.value === i && i === currentQuiz.value.answer) return 'bg-green-200 border-green-500';
  if (selectedAnswer.value === i) return 'bg-red-200 border-red-500';
  if (selectedAnswer.value !== null && i === currentQuiz.value.answer) return 'bg-green-50 border-green-300';
  return 'bg-white border-gray-200';
}
</script>

<style scoped>
@keyframes fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
.animate-fade-in {
  animation: fade-in 0.8s ease-in-out;
}

@keyframes slide-in {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
.animate-slide-in {
  animation: slide-in 0.6s ease-out;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
