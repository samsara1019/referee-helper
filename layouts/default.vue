<template>
  <div class="min-h-screen bg-gradient-to-br from-indigo-100 to-blue-50">
    <!-- 헤더 부분 -->
    <header class="bg-white shadow-md p-4">
      <div class="container mx-auto flex flex-col md:flex-row items-center justify-between">
        <!-- 로고와 앱 이름 -->
        <NuxtLink to="/" class="flex items-center mb-4 md:mb-0">
          <div class="text-2xl font-bold text-blue-600">Referee Helper</div>
        </NuxtLink>

        <!-- 네비게이션 링크 -->
        <nav class="flex items-center space-x-6">
          <NuxtLink
            to="/quiz"
            class="text-gray-700 hover:text-blue-600 font-medium transition flex items-center"
            :class="{ 'text-blue-600': $route.path === '/quiz' }"
          >
            <span class="mr-1">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"
                />
              </svg>
            </span>
            문제풀기
          </NuxtLink>

          <NuxtLink
            to="/quiz/records"
            class="text-gray-700 hover:text-blue-600 font-medium transition flex items-center"
            :class="{ 'text-blue-600': $route.path === '/quiz-history' }"
          >
            <span class="mr-1">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
                />
              </svg>
            </span>
            기록보기
          </NuxtLink>

          <!-- 로그인/로그아웃 버튼 -->
          <template v-if="user">
            <div class="flex items-center space-x-4">
              <div class="hidden md:block text-sm font-medium text-gray-600">{{ userName }}님</div>
              <button @click="signOut" class="bg-red-500 hover:bg-red-600 text-white text-sm px-3 py-1.5 rounded-lg transition">로그아웃</button>
            </div>
          </template>
          <template v-else>
            <button @click="login" class="bg-blue-500 hover:bg-blue-600 text-white text-sm px-3 py-1.5 rounded-lg transition">로그인</button>
          </template>
        </nav>
      </div>
    </header>

    <!-- 모바일용 사용자 정보 (로그인 시) -->
    <div v-if="user" class="md:hidden bg-blue-50 py-2 px-4 text-center text-sm font-medium text-gray-600 border-b border-blue-100">
      안녕하세요, {{ userName }}님
    </div>

    <!-- 페이지 콘텐츠가 여기에 들어갑니다 -->
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import { useRouter, useRoute } from 'vue-router';

const router = useRouter();
const route = useRoute();
const supabase = useSupabaseClient();
const user = useSupabaseUser();

const userName = computed(() => {
  return user.value?.user_metadata?.name || user.value?.email || '게스트';
});

function login() {
  router.push('/login');
}

async function signOut() {
  try {
    await supabase.auth.signOut();
    router.push('/login');
  } catch (error) {
    console.error('로그아웃 중 오류 발생:', error);
  }
}
</script>

<style scoped>
/* 활성 링크 애니메이션 */
.router-link-active,
.router-link-exact-active {
  font-weight: 600;
  position: relative;
}

.router-link-active::after,
.router-link-exact-active::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: currentColor;
  transition: transform 0.3s ease;
}

/* 호버 효과 */
a {
  position: relative;
}

a::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: currentColor;
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

a:hover::after {
  transform: scaleX(1);
}
</style>
