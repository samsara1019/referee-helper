<template>
  <div class="min-h-screen flex flex-col justify-center items-center bg-gray-100">
    <div class="bg-white p-6 rounded-lg shadow-md space-y-4 w-80">
      <h1 class="text-xl font-semibold text-center">로그인</h1>

      <!-- 이메일 로그인 -->
      <input v-model="email" type="email" placeholder="이메일" class="w-full p-2 border rounded" />
      <input v-model="password" type="password" placeholder="비밀번호" class="w-full p-2 border rounded" />

      <button @click="login" class="w-full bg-blue-600 text-white py-2 rounded hover:bg-blue-700">로그인</button>
      <button @click="signup" class="w-full bg-gray-300 text-gray-700 py-2 rounded hover:bg-gray-400">회원가입</button>

      <!-- 구분선 -->
      <div class="flex items-center my-2">
        <div class="flex-grow border-t border-gray-300"></div>
        <span class="mx-2 text-sm text-gray-500">또는</span>
        <div class="flex-grow border-t border-gray-300"></div>
      </div>

      <!-- 카카오 로그인 버튼 -->
      <button @click="loginWithKakao" class="w-full">
        <img src="/images/kakao-login.png" alt="카카오 로그인" class="w-full rounded" />
      </button>
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue';
const supabase = useSupabaseClient();
const email = ref('');
const password = ref('');

// 로그인
const login = async () => {
  const { error } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });
  if (error) {
    alert('로그인 실패: ' + error.message);
  } else {
    navigateTo('/');
  }
};

// 회원가입
const signup = async () => {
  const { error } = await supabase.auth.signUp({
    email: email.value,
    password: password.value,
  });
  if (error) {
    alert('회원가입 실패: ' + error.message);
  } else {
    alert('회원가입 성공! 이메일을 확인해주세요.');
  }
};

// 카카오 로그인
const loginWithKakao = async () => {
  const { error } = await supabase.auth.signInWithOAuth({
    provider: 'kakao',
    options: {
      redirectTo: 'http://localhost:3002/', // 메인 페이지로 리다이렉트
    },
  });
  if (error) {
    alert('카카오 로그인 실패: ' + error.message);
  }
};
</script>
