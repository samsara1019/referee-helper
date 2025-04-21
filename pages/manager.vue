<!-- pages/manager.vue -->
<template>
  <div class="manager-container p-4">
    <h1 class="text-2xl font-bold mb-6">퀴즈 관리자 페이지</h1>

    <div v-if="loading" class="text-center py-8">
      <p>데이터를 불러오는 중...</p>
    </div>

    <div v-else-if="error" class="bg-red-100 p-4 rounded">
      <p class="text-red-600">{{ error }}</p>
    </div>

    <div v-else-if="!isAdmin" class="bg-yellow-100 p-4 rounded">
      <p class="text-yellow-600">관리자 권한이 필요합니다.</p>
    </div>

    <div v-else>
      <div class="mb-4 flex justify-between">
        <input v-model="searchTerm" type="text" placeholder="검색어를 입력하세요" class="border p-2 rounded" />
      </div>

      <div class="overflow-x-auto">
        <table class="min-w-full bg-white border">
          <thead>
            <tr>
              <th class="border p-2">출제</th>
              <th class="border p-2">질문</th>
              <th class="border p-2">옵션</th>
              <th class="border p-2">정답</th>
              <th class="border p-2">작업</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="quiz in filteredQuizzes" :key="quiz.id" class="border-b">
              <td class="border p-2">{{ quiz.question_creator }}</td>
              <td class="border p-2">{{ quiz.question }}</td>
              <td class="border p-2">
                <div v-for="(option, index) in quiz.options" :key="index">{{ index + 1 }}. {{ option }}</div>
              </td>
              <td class="border p-2">{{ quiz.answer + 1 }} ({{ quiz.options[quiz.answer] }})</td>
              <td class="border p-2">
                <button @click="editQuiz(quiz)" class="bg-blue-500 text-white px-3 py-1 rounded mr-2">수정</button>
                <button @click="confirmDelete(quiz.id)" class="bg-red-500 text-white px-3 py-1 rounded">삭제</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- 수정 모달 -->
    <div v-if="showEditModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
      <div class="bg-white p-4 rounded w-full max-w-lg">
        <h2 class="text-xl font-bold mb-4">퀴즈 수정</h2>

        <div class="mb-4">
          <label class="block mb-1">질문:</label>
          <textarea v-model="editingQuiz.question" class="border p-2 w-full" rows="3"></textarea>
        </div>

        <div class="mb-4">
          <label class="block mb-1">옵션:</label>
          <div v-for="(option, index) in editingQuiz.options" :key="index" class="flex mb-2">
            <div class="mr-2 p-2 bg-gray-100 w-10 text-center">{{ index + 1 }}</div>
            <input v-model="editingQuiz.options[index]" class="border p-2 flex-grow" />
            <button @click="removeOption(index)" class="bg-red-500 text-white px-2 ml-2">X</button>
          </div>
          <button @click="addOption" class="bg-green-500 text-white px-3 py-1 rounded">옵션 추가</button>
        </div>

        <div class="mb-4">
          <label class="block mb-1">정답:</label>
          <select v-model="editingQuiz.answer" class="border p-2 w-full">
            <option v-for="(option, index) in editingQuiz.options" :key="index" :value="index">{{ index + 1 }}. {{ option }}</option>
          </select>
        </div>

        <div class="flex justify-end">
          <button @click="closeEditModal" class="bg-gray-500 text-white px-3 py-1 rounded mr-2">취소</button>
          <button @click="saveQuiz" class="bg-blue-500 text-white px-3 py-1 rounded">저장</button>
        </div>
      </div>
    </div>

    <!-- 삭제 확인 모달 -->
    <div v-if="showDeleteModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
      <div class="bg-white p-4 rounded w-full max-w-md">
        <h2 class="text-xl font-bold mb-4">삭제 확인</h2>
        <p class="mb-4">이 퀴즈를 정말 삭제하시겠습니까? 이 작업은 되돌릴 수 없습니다.</p>

        <div class="flex justify-end">
          <button @click="showDeleteModal = false" class="bg-gray-500 text-white px-3 py-1 rounded mr-2">취소</button>
          <button @click="deleteQuiz" class="bg-red-500 text-white px-3 py-1 rounded">삭제</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { useSupabaseClient } from '#imports';
import { useRouter } from 'vue-router';

// 상태 변수
const quizzes = ref([]);
const loading = ref(true);
const error = ref(null);
const isAdmin = ref(true);
const searchTerm = ref('');
const showEditModal = ref(false);
const showDeleteModal = ref(false);
const editingQuiz = ref({});
const deleteQuizId = ref(null);

const supabase = useSupabaseClient();
const router = useRouter();

// 필터링된 퀴즈 목록
const filteredQuizzes = computed(() => {
  if (!searchTerm.value) return quizzes.value;

  const term = searchTerm.value.toLowerCase();
  return quizzes.value.filter(
    (quiz) => quiz.question.toLowerCase().includes(term) || quiz.options.some((option) => option.toLowerCase().includes(term)),
  );
});

// 관리자 권한 확인
async function checkAdminStatus() {
  try {
    const {
      data: { user },
    } = await supabase.auth.getUser();

    if (!user) {
      navigateTo('/login'); // Nuxt의 navigateTo 사용
      return;
    }

    // 관리자 권한 확인 (사용자의 실제 구현에 맞게 수정 필요)
    const { data, error: profileError } = await supabase.from('profiles').select('is_admin').eq('id', user.id).single();

    if (profileError) {
      console.error('권한 확인 오류:', profileError);
      isAdmin.value = false;
      return;
    }

    isAdmin.value = data?.is_admin || false;
  } catch (err) {
    console.error('권한 확인 중 오류 발생:', err);
    isAdmin.value = false;
  }
}

// 모든 퀴즈 데이터 가져오기
async function fetchQuizzes() {
  try {
    loading.value = true;
    const { data, error: fetchError } = await supabase.from('quizzes').select('*').order('id');

    if (fetchError) throw fetchError;

    quizzes.value = data;
  } catch (err) {
    console.error('퀴즈 데이터 로딩 오류:', err);
    error.value = '데이터를 불러오는데 문제가 발생했습니다.';
  } finally {
    loading.value = false;
  }
}

// 퀴즈 수정 모달 열기
function editQuiz(quiz) {
  editingQuiz.value = JSON.parse(JSON.stringify(quiz)); // 깊은 복사
  showEditModal.value = true;
}

// 모달 닫기
function closeEditModal() {
  showEditModal.value = false;
  editingQuiz.value = {};
}

// 옵션 추가
function addOption() {
  editingQuiz.value.options.push('');
}

// 옵션 제거
function removeOption(index) {
  editingQuiz.value.options.splice(index, 1);

  // 만약 삭제된 옵션의 인덱스가 정답이었다면 정답 재설정
  if (editingQuiz.value.answer >= editingQuiz.value.options.length) {
    editingQuiz.value.answer = 0; // 첫 번째 옵션으로 기본 설정
  }

  // 인덱스가 삭제된 옵션보다 큰 경우 정답 인덱스 조정
  if (editingQuiz.value.answer > index) {
    editingQuiz.value.answer = Number(editingQuiz.value.answer) - 1;
  }
}

// 퀴즈 저장
async function saveQuiz() {
  try {
    loading.value = true;

    // 빈 옵션 필터링
    editingQuiz.value.options = editingQuiz.value.options.filter((option) => option.trim() !== '');

    // 옵션이 없으면 저장하지 않음
    if (editingQuiz.value.options.length === 0) {
      alert('최소 한 개 이상의 옵션이 필요합니다.');
      return;
    }

    // 정답 인덱스가 유효한지 확인
    if (editingQuiz.value.answer >= editingQuiz.value.options.length) {
      editingQuiz.value.answer = 0;
    }

    // 수정된 퀴즈 저장
    const { error: updateError } = await supabase
      .from('quizzes')
      .update({
        question: editingQuiz.value.question,
        options: editingQuiz.value.options,
        answer: Number(editingQuiz.value.answer), // 숫자로 변환하여 저장
      })
      .eq('id', editingQuiz.value.id);

    if (updateError) throw updateError;

    // 목록 새로고침
    await fetchQuizzes();
    closeEditModal();
  } catch (err) {
    console.error('퀴즈 저장 오류:', err);
    alert('퀴즈 저장 중 오류가 발생했습니다.');
  } finally {
    loading.value = false;
  }
}

// 삭제 확인 모달 표시
function confirmDelete(id) {
  deleteQuizId.value = id;
  showDeleteModal.value = true;
}

// 퀴즈 삭제
async function deleteQuiz() {
  try {
    loading.value = true;

    const { error: deleteError } = await supabase.from('quizzes').delete().eq('id', deleteQuizId.value);

    if (deleteError) throw deleteError;

    // 목록 새로고침
    await fetchQuizzes();
    showDeleteModal.value = false;
  } catch (err) {
    console.error('퀴즈 삭제 오류:', err);
    alert('퀴즈 삭제 중 오류가 발생했습니다.');
  } finally {
    loading.value = false;
  }
}

// 컴포넌트 마운트 시 데이터 로드
onMounted(async () => {
  // await checkAdminStatus();
  if (isAdmin.value) {
    await fetchQuizzes();
  }
});
</script>

<style scoped>
.manager-container {
  max-width: 1200px;
  margin: 0 auto;
}
</style>
