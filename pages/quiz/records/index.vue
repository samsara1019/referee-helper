<template>
  <div class="min-h-screen bg-gradient-to-br from-indigo-100 to-blue-50 p-6">
    <div class="max-w-4xl mx-auto">
      <!-- 헤더 -->
      <div class="text-center mb-8">
        <h1 class="text-3xl font-bold text-gray-800">나의 퀴즈 기록</h1>
        <p class="text-gray-600 mt-2">날짜를 선택하여 풀었던 문제를 확인해보세요</p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <!-- 캘린더 섹션 -->
        <div class="bg-white rounded-xl shadow-md p-4 md:col-span-1">
          <div class="mb-4 flex justify-between items-center">
            <button @click="prevMonth" class="text-gray-600 hover:text-gray-900">
              <span class="text-xl">←</span>
            </button>
            <h2 class="text-xl font-semibold text-gray-800">{{ currentMonthName }} {{ currentYear }}</h2>
            <button @click="nextMonth" class="text-gray-600 hover:text-gray-900">
              <span class="text-xl">→</span>
            </button>
          </div>

          <!-- 요일 헤더 -->
          <div class="grid grid-cols-7 text-center font-medium text-gray-500 mb-2">
            <div v-for="day in ['일', '월', '화', '수', '목', '금', '토']" :key="day" class="p-2">
              {{ day }}
            </div>
          </div>

          <!-- 날짜 그리드 -->
          <div class="grid grid-cols-7 gap-1">
            <div
              v-for="(day, index) in calendarDays"
              :key="index"
              @click="selectDate(day)"
              :class="[
                'p-2 text-center rounded-lg cursor-pointer transition',
                day.month !== currentMonth ? 'text-gray-300' : 'text-gray-700',
                day.date === selectedDate ? 'bg-blue-500 text-white' : 'hover:bg-gray-100',
                day.hasQuizzes ? 'font-bold' : '',
                day.isPast ? '' : 'opacity-50',
              ]"
            >
              {{ day.day }}
              <div v-if="day.hasQuizzes" class="h-1 w-1 bg-green-500 rounded-full mx-auto mt-1"></div>
            </div>
          </div>
        </div>

        <!-- 퀴즈 목록 섹션 -->
        <div class="bg-white rounded-xl shadow-md p-4 md:col-span-2">
          <div v-if="!selectedDate" class="text-center py-12">
            <p class="text-gray-500">날짜를 선택하여 퀴즈 기록을 확인하세요</p>
          </div>
          <div v-else>
            <h2 class="text-xl font-semibold text-gray-800 mb-4">{{ formatSelectedDate }} 퀴즈 기록</h2>

            <div v-if="loadingQuizzes" class="flex justify-center py-6">
              <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-500"></div>
            </div>

            <div v-else-if="dayQuizzes.length === 0" class="text-center py-6">
              <p class="text-gray-500">해당 날짜에 풀었던 퀴즈 기록이 없습니다</p>
            </div>

            <div v-else class="space-y-6">
              <div v-for="(record, index) in dayQuizzes" :key="index" class="border rounded-lg p-4 shadow-sm hover:shadow-md transition">
                <div class="flex items-start justify-between">
                  <div>
                    <p class="font-medium text-sm text-gray-500">{{ record.quiz.question_creator }} 님이 출제한 문제</p>
                    <p class="text-lg font-medium text-gray-800 mt-1">{{ record.quiz.question }}</p>
                  </div>
                  <div
                    :class="record.is_correct ? 'bg-green-100 text-green-700' : 'bg-red-100 text-red-700'"
                    class="py-1 px-3 rounded-full text-xs font-medium whitespace-nowrap"
                  >
                    {{ record.is_correct ? '정답' : '오답' }}
                  </div>
                </div>

                <div class="mt-4 grid grid-cols-1 gap-2">
                  <div
                    v-for="(option, i) in record.quiz.options"
                    :key="i"
                    :class="['px-3 py-2 rounded border transition', getAnswerClass(i, record.selected_answer, record.quiz.answer)]"
                  >
                    {{ option }}
                  </div>
                </div>

                <div v-if="!record.is_correct" class="mt-4 p-3 bg-gray-50 rounded-lg">
                  <p class="text-sm text-gray-700"><span class="font-medium">해설:</span> {{ record.quiz.explanation }}</p>
                </div>

                <div class="mt-4 flex justify-end">
                  <button @click="retryQuiz(record.quiz.id)" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg text-sm">
                    다시 풀기
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const supabase = useSupabaseClient();
const user = useSupabaseUser();

// 날짜 관련 상태
const currentDate = ref(new Date());
const currentMonth = ref(currentDate.value.getMonth());
const currentYear = ref(currentDate.value.getFullYear());
const selectedDate = ref(null);
const quizDates = ref([]);

// 퀴즈 관련 상태
const dayQuizzes = ref([]);
const loadingQuizzes = ref(false);

// 캘린더 관련 계산된 속성
const currentMonthName = computed(() => {
  return new Date(currentYear.value, currentMonth.value).toLocaleString('ko-KR', { month: 'long' });
});

const formatSelectedDate = computed(() => {
  if (!selectedDate.value) return '';
  return new Date(selectedDate.value).toLocaleDateString('ko-KR', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  });
});

const calendarDays = computed(() => {
  const days = [];
  const firstDay = new Date(currentYear.value, currentMonth.value, 1);
  const lastDay = new Date(currentYear.value, currentMonth.value + 1, 0);

  // 이전 달의 마지막 날짜들
  const prevMonthLastDate = new Date(currentYear.value, currentMonth.value, 0).getDate();
  const firstDayOfWeek = firstDay.getDay();

  for (let i = firstDayOfWeek - 1; i >= 0; i--) {
    const prevMonth = currentMonth.value === 0 ? 11 : currentMonth.value - 1;
    const prevYear = currentMonth.value === 0 ? currentYear.value - 1 : currentYear.value;
    const day = prevMonthLastDate - i;
    const dateString = `${prevYear}-${String(prevMonth + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;

    days.push({
      day,
      month: prevMonth,
      dateString,
      hasQuizzes: quizDates.value.includes(dateString),
      isPast: new Date(dateString) <= new Date(),
    });
  }

  // 현재 달의 날짜들
  for (let i = 1; i <= lastDay.getDate(); i++) {
    const dateString = `${currentYear.value}-${String(currentMonth.value + 1).padStart(2, '0')}-${String(i).padStart(2, '0')}`;

    days.push({
      day: i,
      month: currentMonth.value,
      dateString,
      hasQuizzes: quizDates.value.includes(dateString),
      isPast: new Date(dateString) <= new Date(),
    });
  }

  // 다음 달의 시작 날짜들
  const remainingDays = 42 - days.length; // 6 주 표시
  for (let i = 1; i <= remainingDays; i++) {
    const nextMonth = currentMonth.value === 11 ? 0 : currentMonth.value + 1;
    const nextYear = currentMonth.value === 11 ? currentYear.value + 1 : currentYear.value;
    const dateString = `${nextYear}-${String(nextMonth + 1).padStart(2, '0')}-${String(i).padStart(2, '0')}`;

    days.push({
      day: i,
      month: nextMonth,
      dateString,
      hasQuizzes: quizDates.value.includes(dateString),
      isPast: new Date(dateString) <= new Date(),
    });
  }

  return days;
});

// 캘린더 이동 함수
function prevMonth() {
  if (currentMonth.value === 0) {
    currentMonth.value = 11;
    currentYear.value--;
  } else {
    currentMonth.value--;
  }
  fetchQuizDates();
}

function nextMonth() {
  if (currentMonth.value === 11) {
    currentMonth.value = 0;
    currentYear.value++;
  } else {
    currentMonth.value++;
  }
  fetchQuizDates();
}

// 날짜 선택 함수
function selectDate(day) {
  if (!day.isPast) return; // 미래 날짜는 선택 불가
  selectedDate.value = day.dateString;
  fetchDayQuizzes(day.dateString);
}

// 데이터 가져오기 함수
async function fetchQuizDates() {
  if (!user.value) return;

  const startOfMonth = `${currentYear.value}-${String(currentMonth.value + 1).padStart(2, '0')}-01`;
  const endOfMonth = new Date(currentYear.value, currentMonth.value + 1, 0);
  const endDate = `${currentYear.value}-${String(currentMonth.value + 1).padStart(2, '0')}-${String(endOfMonth.getDate()).padStart(2, '0')}`;

  const { data } = await supabase
    .from('quiz_records')
    .select('date')
    .eq('user_id', user.value.id)
    .gte('date', startOfMonth)
    .lte('date', endDate)
    .order('date');

  if (data) {
    // 중복 날짜 제거
    quizDates.value = [...new Set(data.map((record) => record.date))];
  }
}

async function fetchDayQuizzes(date) {
  if (!user.value) return;

  loadingQuizzes.value = true;

  const { data, error } = await supabase
    .from('quiz_records')
    .select(
      `
      *,
      quiz:quiz_id (
        id,
        question,
        options,
        answer,
        explanation,
        question_creator
      )
    `,
    )
    .eq('user_id', user.value.id)
    .eq('date', date)
    .order('created_at');

  loadingQuizzes.value = false;

  if (error) {
    console.error('Error fetching quizzes:', error);
    return;
  }

  dayQuizzes.value = data;
}

// 퀴즈 다시 풀기
function retryQuiz(quizId) {
  // 이 함수는 선택된 퀴즈를 다시 풀 수 있는 페이지로 이동합니다
  // 실제 구현에서는 퀴즈 ID를 포함한 URL로 이동하거나 상태를 설정할 수 있습니다
  router.push({
    path: '/quiz/retry',
    query: { retry: quizId },
  });
}

// 옵션 클래스 결정
function getAnswerClass(optionIndex, selectedAnswer, correctAnswer) {
  if (optionIndex === correctAnswer) {
    return 'bg-green-50 border-green-500 text-green-800';
  }
  if (optionIndex === selectedAnswer && selectedAnswer !== correctAnswer) {
    return 'bg-red-50 border-red-500 text-red-800';
  }
  return 'bg-white border-gray-200 text-gray-700';
}

onMounted(async () => {
  if (user.value) {
    await fetchQuizDates();
    // 기본적으로 오늘 날짜 선택
    const today = new Date().toISOString().split('T')[0];
    selectedDate.value = today;
    fetchDayQuizzes(today);
  }
});
</script>

<style scoped>
/* 추가 스타일이 필요한 경우 여기에 추가 */
</style>
