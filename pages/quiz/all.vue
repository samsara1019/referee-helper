<template>
  <div class="min-h-screen bg-gradient-to-br from-indigo-100 to-blue-50 p-6">
    <div class="max-w-4xl mx-auto">
      <!-- 헤더 섹션 -->
      <div class="mb-8 text-center">
        <h1 class="text-3xl font-bold text-gray-800">전체 문제 보기</h1>
        <p class="text-lg text-gray-600 mt-2">{{ userName }}님의 퀴즈 기록</p>
        <p class="text-gray-600 mt-2" v-if="!isLogin">로그인을 하셔야만 문제를 풀 수 있고 전체 문제를 확인할 수 있습니다.</p>

        <div class="mt-4 bg-white rounded-xl shadow p-4">
          <div class="flex justify-between items-center">
            <div>
              <p class="text-sm text-gray-500">총 문제 수</p>
              <p class="text-xl font-bold">{{ quizzes.length }}문제</p>
            </div>
            <div>
              <p class="text-sm text-gray-500">푼 문제 수</p>
              <p class="text-xl font-bold">{{ solvedQuizzes.length }}문제</p>
            </div>
            <div>
              <p class="text-sm text-gray-500">정답률</p>
              <p class="text-xl font-bold text-green-600">{{ correctRate }}%</p>
            </div>
          </div>
        </div>
      </div>

      <!-- 검색 및 필터링 -->
      <div class="mb-6 flex flex-wrap gap-4">
        <input v-model="searchQuery" type="text" placeholder="문제 검색" class="px-4 py-2 border rounded-lg flex-grow" />

        <select v-model="filterOption" class="px-4 py-2 border rounded-lg bg-white">
          <option value="all">모든 문제</option>
          <option value="solved">푼 문제만</option>
          <option value="unsolved">안 푼 문제만</option>
          <option value="correct">맞은 문제만</option>
          <option value="incorrect">틀린 문제만</option>
        </select>
      </div>

      <!-- 로딩 상태 -->
      <div v-if="pending" class="text-center py-10">
        <p class="text-gray-600">문제를 불러오는 중입니다...</p>
      </div>

      <!-- 문제 목록 -->
      <div v-else class="space-y-6">
        <div v-if="filteredQuizzes.length === 0" class="text-center py-10">
          <p class="text-gray-600">표시할 문제가 없습니다.</p>
        </div>

        <div v-for="quiz in filteredQuizzes" :key="quiz.id" class="bg-white rounded-xl shadow overflow-hidden transition hover:shadow-md">
          <div class="p-6">
            <!-- 문제 정보 헤더 -->
            <div class="flex justify-between items-start mb-3">
              <div>
                <p class="text-sm text-gray-500 italic">출제자: {{ quiz.question_creator || '알 수 없음' }}</p>
                <h3 class="text-xl font-semibold text-gray-800">{{ quiz.question }}</h3>
              </div>
              <div class="flex space-x-2">
                <span
                  v-if="getQuizStatus(quiz).solved"
                  :class="[getQuizStatus(quiz).correct ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800', 'px-2 py-1 text-xs rounded-full']"
                >
                  {{ getQuizStatus(quiz).correct ? '정답' : '오답' }}
                </span>
                <span v-if="getQuizStatus(quiz).solved" class="bg-blue-100 text-blue-800 px-2 py-1 text-xs rounded-full">
                  {{ formatDate(getQuizRecord(quiz.id)?.created_at) }}
                </span>
              </div>
            </div>

            <!-- 문제 옵션 -->
            <div class="space-y-2 mt-4">
              <div
                v-for="(option, i) in quiz.options"
                :key="i"
                :class="[getQuizStatus(quiz).solved ? getOptionClassForHistory(quiz, i) : 'bg-gray-50', 'p-3 border rounded-lg transition']"
              >
                {{ i }}. {{ option }}
              </div>
            </div>

            <!-- 설명 (오답인 경우만 표시) -->
            <div
              v-if="getQuizStatus(quiz).solved && !getQuizStatus(quiz).correct && quiz.explanation"
              class="mt-4 p-3 bg-yellow-50 text-yellow-800 rounded-lg"
            >
              <p class="text-sm"><strong>설명:</strong> {{ quiz.explanation }}</p>
            </div>

            <!-- 풀지 않은 문제는 버튼 표시 -->
            <div v-if="!getQuizStatus(quiz).solved" class="mt-4 flex justify-end">
              <button @click="goToQuiz(quiz.id)" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg text-sm">이 문제 풀기</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// Supabase 클라이언트
const supabase = useSupabaseClient();
const user = useSupabaseUser();
const router = useRouter();

// 상태 관리
const userName = computed(() => user.value?.user_metadata.name || user.value?.email || '심판');
const searchQuery = ref('');
const filterOption = ref('all');
const quizRecords = ref([]);
const isLogin = ref(false);

// 퀴즈 데이터 가져오기
const { data: quizzes, pending } = await useAsyncData('all-quizzes', async () => {
  const { data, error } = await supabase.from('quizzes').select('*');
  if (error) throw error;
  return data;
});

// 사용자의 퀴즈 기록 가져오기
const fetchQuizRecords = async () => {
  if (!user.value) return;

  const { data, error } = await supabase.from('quiz_records').select('*').eq('user_id', user.value.id);

  if (error) {
    console.error('퀴즈 기록을 불러오는 중 오류 발생:', error);
    return;
  }

  quizRecords.value = data || [];
};

// 컴포넌트 마운트 시 기록 불러오기
onMounted(() => {
  fetchQuizRecords();
});

// 계산된 속성들
const solvedQuizzes = computed(() => {
  return quizzes.value?.filter((quiz) => quizRecords.value.some((record) => record.quiz_id === quiz.id)) || [];
});

const correctQuizzes = computed(() => {
  return quizzes.value?.filter((quiz) => quizRecords.value.some((record) => record.quiz_id === quiz.id && record.is_correct === true)) || [];
});

const correctRate = computed(() => {
  if (solvedQuizzes.value.length === 0) return '0';
  return ((correctQuizzes.value.length / solvedQuizzes.value.length) * 100).toFixed(1);
});

// 필터링된 퀴즈 목록
const filteredQuizzes = computed(() => {
  if (!quizzes.value) return [];

  let filtered = [...quizzes.value];

  // 검색어 필터링
  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase();
    filtered = filtered.filter(
      (quiz) => quiz.question.toLowerCase().includes(query) || quiz.options.some((option) => option.toLowerCase().includes(query)),
    );
  }

  // 상태 필터링
  switch (filterOption.value) {
    case 'solved':
      filtered = filtered.filter((quiz) => quizRecords.value.some((record) => record.quiz_id === quiz.id));
      break;
    case 'unsolved':
      filtered = filtered.filter((quiz) => !quizRecords.value.some((record) => record.quiz_id === quiz.id));
      break;
    case 'correct':
      filtered = filtered.filter((quiz) => quizRecords.value.some((record) => record.quiz_id === quiz.id && record.is_correct === true));
      break;
    case 'incorrect':
      filtered = filtered.filter((quiz) => quizRecords.value.some((record) => record.quiz_id === quiz.id && record.is_correct === false));
      break;
  }
  if (!isLogin.value) {
    return filtered.slice(0, 5);
  } else {
    return filtered;
  }
});

// 퀴즈 상태 확인 헬퍼 함수
function getQuizStatus(quiz) {
  const record = quizRecords.value.find((r) => r.quiz_id === quiz.id);

  return {
    solved: !!record,
    correct: record?.is_correct || false,
    selectedAnswer: record?.selected_answer,
  };
}

// 퀴즈 레코드 가져오기
function getQuizRecord(quizId) {
  return quizRecords.value.find((r) => r.quiz_id === quizId);
}

// 옵션 클래스 반환 (히스토리 뷰용)
function getOptionClassForHistory(quiz, index) {
  const status = getQuizStatus(quiz);

  if (!status.solved) return 'bg-gray-50';

  if (index === quiz.answer && status.selectedAnswer === index) return 'bg-green-200 border-green-500'; // 선택한 정답

  if (status.selectedAnswer === index) return 'bg-red-200 border-red-500'; // 선택한 오답

  if (index === quiz.answer) return 'bg-green-50 border-green-300'; // 선택하지 않은 정답

  return 'bg-gray-50'; // 나머지 옵션
}

// 날짜 포맷팅
function formatDate(dateString) {
  if (!dateString) return '';

  const date = new Date(dateString);
  return `${date.getFullYear()}.${(date.getMonth() + 1).toString().padStart(2, '0')}.${date.getDate().toString().padStart(2, '0')}`;
}

// 퀴즈 페이지로 이동
function goToQuiz(quizId) {
  // 이 부분은 실제 앱 구조에 맞게 수정하세요
  // 예: 특정 문제만 풀도록 파라미터를 전달
  router.push({
    path: '/quiz/retry',
    query: { retry: quizId },
  });
}

// 사용자가 로그인하지 않았을 때 처리
watch(
  () => user.value,
  (newUser) => {
    if (newUser) {
      isLogin.value = true;
      fetchQuizRecords();
    } else {
      isLogin.value = false;
      quizRecords.value = [];
    }
  },
  { immediate: true },
);
</script>
