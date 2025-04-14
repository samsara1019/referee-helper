<template>
  <div class="min-h-screen bg-gradient-to-br from-indigo-100 to-blue-50 p-6 flex flex-col items-center justify-center">
    <div v-if="step === 'intro'" class="text-center space-y-6 animate-fade-in">
      <h1 class="text-3xl font-bold text-gray-800">안녕하세요! {{ userName }}님</h1>
      <p class="text-lg text-gray-600">오늘의 퀴즈를 시작해볼까요?</p>
      <button @click="startQuiz(false)" class="bg-blue-600 hover:bg-blue-700 transition text-white px-6 py-3 rounded-xl shadow-md">
        퀴즈 시작하기
      </button>
    </div>

    <div v-else-if="step === 'quiz'" class="w-full max-w-lg space-y-8 animate-slide-in">
      <div class="bg-white rounded-xl shadow p-6">
        <h2 class="text-xl font-semibold text-gray-800">문제 {{ currentIndex + 1 }} / {{ questionsToShow.length }}</h2>
        <!-- 출제자 표시 추가 -->
        <p class="text-sm text-gray-500 italic">{{ currentQuestion.question_creator }} 님이 출제하신 문제입니다</p>
        <p class="mt-3 text-lg text-gray-700 whitespace-pre-line">{{ currentQuestion.question }}</p>
      </div>

      <div class="space-y-4">
        <button
          v-for="(option, i) in currentQuestion.options"
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
          <p v-if="selectedAnswer === currentQuestion.answer" class="text-green-600 font-semibold">🎉 정답입니다!</p>
          <p v-else class="text-red-600 font-semibold">❌ 틀렸습니다! {{ currentQuestion.explanation }}</p>
          <button @click="nextQuestion" class="mt-4 bg-blue-500 hover:bg-blue-600 transition text-white px-4 py-2 rounded-xl shadow">
            다음 문제
          </button>
        </div>
      </transition>
    </div>

    <div v-else-if="step === 'result'" class="text-center space-y-6 animate-fade-in">
      <h2 class="text-3xl font-bold text-gray-800">🎯 퀴즈 완료!</h2>
      <p class="text-xl text-gray-700">총 {{ score }}점 / {{ questionsToShow.length }}점</p>
      <div class="space-x-4">
        <button @click="startQuiz(true)" class="bg-yellow-500 hover:bg-yellow-600 transition text-white px-6 py-3 rounded-xl shadow">더 풀기!</button>
        <button @click="resetQuiz" class="bg-green-500 hover:bg-green-600 transition text-white px-6 py-3 rounded-xl shadow">처음으로</button>
      </div>
    </div>
  </div>
</template>

<script setup>
// Supabase 클라이언트
const supabase = useSupabaseClient();

const user = useSupabaseUser();
const userName = user.value?.user_metadata.name || user.value?.email || '심판';
const step = ref('intro');
const currentIndex = ref(0);
const selectedAnswer = ref(null);
const score = ref(0);
const solvedQuizIds = ref(new Set());

// 수정: 사용자의 맞은 문제 ID를 가져와서 제외하기
const { data: quizzes, pending } = await useAsyncData('quizzes', async () => {
  // 1. 사용자가 맞은 문제 ID를 가져옵니다 (로그인된 경우)
  let correctQuizIds = [];
  if (user.value) {
    const { data: correctRecords } = await supabase.from('quiz_records').select('quiz_id').eq('user_id', user.value.id).eq('is_correct', true);

    // 맞은 문제 ID 추출
    if (correctRecords && correctRecords.length > 0) {
      correctQuizIds = correctRecords.map((record) => record.quiz_id);
    }
  }

  // 2. 이미 맞은 문제를 제외한 모든 문제 가져오기
  let query = supabase.from('quizzes').select('*');

  // 맞은 문제가 있으면 해당 문제 제외
  if (correctQuizIds.length > 0) {
    query = query.not('id', 'in', `(${correctQuizIds.join(',')})`);
  }

  const { data, error } = await query;
  if (error) throw error;
  return data;
});

const questionsToShow = ref([]);
const currentQuestion = computed(() => questionsToShow.value[currentIndex.value]);

function startQuiz(isExtra = false) {
  if (isExtra && !user.value) {
    const confirmLogin = window.confirm('로그인하면 더 많은 문제를 풀어볼 수 있어요.\n로그인하시겠습니까?');
    if (confirmLogin) {
      return navigateTo('/login');
    } else {
      return; // 로그인하지 않으면 진행 안 함
    }
  }

  step.value = 'quiz';
  selectedAnswer.value = null;
  currentIndex.value = 0;
  score.value = 0;
  const available = quizzes.value.filter((q) => !solvedQuizIds.value.has(q.id));
  console.log(available);
  const shuffled = available.sort(() => 0.5 - Math.random()).slice(0, 5);
  questionsToShow.value = shuffled;
}

async function submitAnswer(index) {
  selectedAnswer.value = index;
  const isCorrect = index === currentQuestion.value.answer;
  if (isCorrect) {
    score.value++;
  }
  const quizId = currentQuestion.value.id;
  solvedQuizIds.value.add(quizId);

  if (user.value) {
    await supabase.from('quiz_records').insert({
      user_id: user.value.id,
      quiz_id: quizId,
      selected_answer: index,
      is_correct: isCorrect,
      created_at: new Date().toISOString(),
      date: new Date().toISOString().split('T')[0],
    });
  }
}

function nextQuestion() {
  if (currentIndex.value < questionsToShow.value.length - 1) {
    currentIndex.value++;
    selectedAnswer.value = null;
  } else {
    step.value = 'result';
  }
}

function resetQuiz() {
  step.value = 'intro';
  selectedAnswer.value = null;
  currentIndex.value = 0;
}

function getOptionClass(i) {
  if (selectedAnswer.value === i && i === currentQuestion.value.answer) return 'bg-green-200 border-green-500';
  if (selectedAnswer.value === i) return 'bg-red-200 border-red-500';
  if (i === currentQuestion.value.answer) return 'bg-green-50 border-green-300';
  return 'bg-white';
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

.slide-fade-enter-active {
  transition: all 0.6s ease;
}
.slide-fade-enter-from {
  opacity: 0;
  transform: translateY(20px);
}
</style>
