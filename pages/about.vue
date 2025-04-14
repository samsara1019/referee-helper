<template>
  <div class="about-page">
    <!-- 헤더는 기존 메인 페이지와 동일하게 사용한다고 가정 -->

    <!-- 페이지 제목 배너 -->
    <section class="pt-32 pb-12 bg-gradient-to-r from-blue-600 to-blue-800 text-white">
      <div class="container mx-auto px-4 text-center">
        <h1 class="text-4xl font-bold mb-4">About Referee Helper</h1>
        <p class="text-xl max-w-3xl mx-auto">심판의 이론 지식을 강화하고, 실전 경기에서 더 나은 판정을 내릴 수 있도록 도와드립니다.</p>
      </div>
    </section>

    <!-- 운영자 소개 섹션 -->
    <section class="py-16 bg-white">
      <div class="container mx-auto px-4">
        <div class="grid md:grid-cols-2 gap-12 items-center">
          <div>
            <h2 class="text-3xl font-bold mb-6">운영자 소개</h2>
            <div class="bg-blue-50 p-6 rounded-lg mb-6 border-l-4 border-blue-500">
              <p class="italic text-gray-700">
                "심판으로서 성장하기 위해서는 끊임없는 학습과 연습이 필요합니다. 제가 처음 배우며 겪었던 어려움을 다른 분들은 좀 더 쉽게 극복할 수
                있기를 바랍니다."
              </p>
            </div>
            <p class="text-gray-700 mb-4">
              안녕하세요, 저는 대한축구협회(KFA) 5급 심판 자격증을 보유한 Referee Helper의 운영자입니다. 심판 활동을 하며 항상 규칙을 잘 숙지하고
              있어야 한다는 중요성을 깨달았고, 시험을 준비하는 과정에서 체계적으로 문제를 풀어볼 수 있는 플랫폼이 필요하다고 느꼈습니다.
            </p>
            <p class="text-gray-700 mb-4">
              이론 지식을 강화하고, 최신 규정 변경사항을 항상 학습하고 싶은 마음에서 Referee Helper를 개발하게 되었으며, 제가 겪었던 어려움을 다른
              분들은 조금 더 쉽게 극복하길 바라는 마음으로 서비스를 운영하고 있습니다.
            </p>
            <p class="text-gray-700">
              심판으로서의 경험과 이론 지식을 공유하며, 더 많은 분들이 축구 심판의 세계에 관심을 갖고 함께 성장할 수 있기를 희망합니다.
            </p>
          </div>
          <div class="order-first md:order-last">
            <!-- <img src="/api/placeholder/500/400" alt="운영자 이미지" class="rounded-lg shadow-xl mx-auto" /> -->
          </div>
        </div>
      </div>
    </section>

    <!-- 서비스 미션 섹션 -->
    <section class="py-16 bg-gray-50">
      <div class="container mx-auto px-4 text-center">
        <h2 class="text-3xl font-bold mb-12">우리의 미션</h2>

        <div class="grid md:grid-cols-3 gap-8">
          <div class="bg-white p-6 rounded-lg shadow-md hover:shadow-lg hover:-translate-y-1 transition-all duration-300">
            <div class="w-16 h-16 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-6">
              <span class="material-icons text-3xl text-blue-600">school</span>
            </div>
            <h3 class="text-xl font-bold mb-4">초급 심판 지원</h3>
            <p class="text-gray-600">
              심판 활동을 시작한 초급 심판들이 실전에서 자신감을 갖고 판정할 수 있도록 이론 지식을 강화하는 데 도움을 드립니다.
            </p>
          </div>

          <div class="bg-white p-6 rounded-lg shadow-md hover:shadow-lg hover:-translate-y-1 transition-all duration-300">
            <div class="w-16 h-16 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-6">
              <span class="material-icons text-3xl text-blue-600">verified</span>
            </div>
            <h3 class="text-xl font-bold mb-4">자격증 준비 지원</h3>
            <p class="text-gray-600">
              KFA 심판 자격증 취득을 준비하는 분들이 필기시험을 효과적으로 준비할 수 있도록 다양한 문제와 학습 자료를 제공합니다.
            </p>
          </div>

          <div class="bg-white p-6 rounded-lg shadow-md hover:shadow-lg hover:-translate-y-1 transition-all duration-300">
            <div class="w-16 h-16 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-6">
              <span class="material-icons text-3xl text-blue-600">update</span>
            </div>
            <h3 class="text-xl font-bold mb-4">최신 규정 업데이트</h3>
            <p class="text-gray-600">
              IFAB의 최신 규칙 변경사항을 반영한 문제와 학습 자료를 제공하여 심판들이 항상 최신 규정을 숙지할 수 있도록 돕습니다.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- 서비스 소개 섹션 -->
    <section class="py-16 bg-white">
      <div class="container mx-auto px-4">
        <h2 class="text-3xl font-bold text-center mb-12">Referee Helper의 특징</h2>

        <div class="grid md:grid-cols-2 gap-10">
          <div class="flex gap-4">
            <div class="flex-shrink-0">
              <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
                <span class="material-icons text-blue-600">quiz</span>
              </div>
            </div>
            <div>
              <h3 class="text-xl font-bold mb-2">24/25 IFAB 기준 문제</h3>
              <p class="text-gray-600">
                최신 국제축구평의회(IFAB) 기준에 맞춰 업데이트된 문제들을 풀어볼 수 있습니다. 규칙 변경사항을 반영한 문제들로 항상 최신 규정을 학습할
                수 있습니다.
              </p>
            </div>
          </div>

          <div class="flex gap-4">
            <div class="flex-shrink-0">
              <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
                <span class="material-icons text-blue-600">category</span>
              </div>
            </div>
            <div>
              <h3 class="text-xl font-bold mb-2">카테고리별 문제 분류</h3>
              <p class="text-gray-600">
                경기 규칙의 17개 조항과 주요 주제별로 문제를 분류하여 특정 영역에 집중해서 학습할 수 있도록 구성되어 있습니다.
              </p>
            </div>
          </div>

          <div class="flex gap-4">
            <div class="flex-shrink-0">
              <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
                <span class="material-icons text-blue-600">history</span>
              </div>
            </div>
            <div>
              <h3 class="text-xl font-bold mb-2">학습 기록 관리</h3>
              <p class="text-gray-600">풀어본 문제를 기록하고 통계를 확인할 수 있어 자신의 강점과 약점을 파악하고 효율적으로 학습할 수 있습니다.</p>
            </div>
          </div>

          <div class="flex gap-4">
            <div class="flex-shrink-0">
              <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
                <span class="material-icons text-blue-600">replay</span>
              </div>
            </div>
            <div>
              <h3 class="text-xl font-bold mb-2">오답 복습 기능</h3>
              <p class="text-gray-600">틀린 문제를 따로 모아 복습할 수 있어 취약한 부분을 집중적으로 보완할 수 있습니다.</p>
            </div>
          </div>

          <div class="flex gap-4">
            <div class="flex-shrink-0">
              <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
                <span class="material-icons text-blue-600">speed</span>
              </div>
            </div>
            <div>
              <h3 class="text-xl font-bold mb-2">모의고사 기능</h3>
              <p class="text-gray-600">
                실제 시험과 유사한 형태의 모의고사를 통해 시험 감각을 기를 수 있습니다. 시간 제한 설정으로 실전처럼 연습할 수 있습니다.
              </p>
            </div>
          </div>

          <div class="flex gap-4">
            <div class="flex-shrink-0">
              <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
                <span class="material-icons text-blue-600">menu_book</span>
              </div>
            </div>
            <div>
              <h3 class="text-xl font-bold mb-2">상세한 해설 제공</h3>
              <p class="text-gray-600">문제마다 규칙서 근거와 함께 상세한 해설을 제공하여 단순 암기가 아닌 깊이 있는 이해를 도와드립니다.</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 대상 사용자 섹션 -->
    <section class="py-16 bg-gray-50">
      <div class="container mx-auto px-4">
        <h2 class="text-3xl font-bold text-center mb-12">이런 분들께 추천합니다</h2>

        <div class="grid md:grid-cols-3 gap-8">
          <div class="bg-white p-8 rounded-lg shadow-md">
            <div class="w-20 h-20 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-6">
              <span class="material-icons text-4xl text-blue-600">sports</span>
            </div>
            <h3 class="text-xl font-bold mb-4 text-center">심판 자격증 준비생</h3>
            <ul class="space-y-3">
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>KFA 심판 자격증 필기시험을 준비 중인 분</span>
              </li>
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>다양한 문제를 통해 시험 감각을 키우고 싶은 분</span>
              </li>
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>경기 규칙에 대한 체계적인 학습이 필요한 분</span>
              </li>
            </ul>
          </div>

          <div class="bg-white p-8 rounded-lg shadow-md">
            <div class="w-20 h-20 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-6">
              <span class="material-icons text-4xl text-blue-600">emoji_events</span>
            </div>
            <h3 class="text-xl font-bold mb-4 text-center">초급 심판</h3>
            <ul class="space-y-3">
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>5급, 4급 심판으로 활동 중인 분</span>
              </li>
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>이론 지식을 강화하여 실전에서 자신감을 키우고 싶은 분</span>
              </li>
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>상위 급수 승급을 준비 중인 분</span>
              </li>
            </ul>
          </div>

          <div class="bg-white p-8 rounded-lg shadow-md">
            <div class="w-20 h-20 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-6">
              <span class="material-icons text-4xl text-blue-600">groups</span>
            </div>
            <h3 class="text-xl font-bold mb-4 text-center">축구 지도자 및 관계자</h3>
            <ul class="space-y-3">
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>축구 지도자로서 규칙을 정확히 알고 싶은 분</span>
              </li>
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>동호회나 학교 팀의 심판을 맡고 있는 분</span>
              </li>
              <li class="flex items-start">
                <span class="material-icons text-green-500 mr-2 mt-1">check_circle</span>
                <span>최신 규칙 변경사항을 쉽게 학습하고 싶은 분</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </section>

    <!-- 자격증 정보 섹션 -->
    <section class="py-16 bg-white">
      <div class="container mx-auto px-4">
        <h2 class="text-3xl font-bold text-center mb-12">KFA 심판 자격증 안내</h2>

        <div class="max-w-4xl mx-auto bg-gray-50 rounded-lg p-8 shadow-md">
          <p class="text-gray-700 mb-6">
            대한축구협회(KFA)에서는 축구 심판 자격증을 1급부터 5급까지 부여하고 있습니다. 각 급수별로 필요한 자격과 시험 과정이 다르며, 특히 처음
            시작하는 5급 심판의 경우 필기시험과 실기시험을 통과해야 합니다.
          </p>

          <h3 class="text-xl font-bold mb-4">5급 심판 자격증 취득 과정</h3>
          <div class="grid md:grid-cols-4 gap-4 mb-8">
            <div class="bg-white p-4 rounded-lg shadow-sm border border-gray-200">
              <div class="flex justify-center items-center mb-3">
                <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
                  <span class="font-bold text-blue-600">1</span>
                </div>
              </div>
              <p class="text-center text-gray-700">교육 신청</p>
            </div>
            <div class="bg-white p-4 rounded-lg shadow-sm border border-gray-200">
              <div class="flex justify-center items-center mb-3">
                <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
                  <span class="font-bold text-blue-600">2</span>
                </div>
              </div>
              <p class="text-center text-gray-700">이론 교육</p>
            </div>
            <div class="bg-white p-4 rounded-lg shadow-sm border border-gray-200">
              <div class="flex justify-center items-center mb-3">
                <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
                  <span class="font-bold text-blue-600">3</span>
                </div>
              </div>
              <p class="text-center text-gray-700">필기시험</p>
            </div>
            <div class="bg-white p-4 rounded-lg shadow-sm border border-gray-200">
              <div class="flex justify-center items-center mb-3">
                <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
                  <span class="font-bold text-blue-600">4</span>
                </div>
              </div>
              <p class="text-center text-gray-700">실기시험</p>
            </div>
          </div>

          <h3 class="text-xl font-bold mb-4">필기시험 준비 팁</h3>
          <ul class="space-y-2 mb-6">
            <li class="flex items-start">
              <span class="material-icons text-blue-600 mr-2">arrow_right</span>
              <span>IFAB 경기 규칙서를 반복적으로 학습하세요.</span>
            </li>
            <li class="flex items-start">
              <span class="material-icons text-blue-600 mr-2">arrow_right</span>
              <span>Referee Helper의 문제 풀이를 통해 자주 출제되는 유형을 파악하세요.</span>
            </li>
            <li class="flex items-start">
              <span class="material-icons text-blue-600 mr-2">arrow_right</span>
              <span>규칙의 내용뿐만 아니라 근거 조항도 함께 기억하세요.</span>
            </li>
            <li class="flex items-start">
              <span class="material-icons text-blue-600 mr-2">arrow_right</span>
              <span>모의고사를 통해 실전 감각을 키우세요.</span>
            </li>
          </ul>

          <div class="bg-blue-50 p-6 rounded-lg border-l-4 border-blue-500">
            <p class="text-gray-700">
              <span class="font-bold">알림:</span> KFA 심판 자격증 취득에 관한 자세한 정보는 대한축구협회 공식 웹사이트에서 확인하실 수 있습니다.
              Referee Helper는 특히 필기시험 준비에 도움을 드리기 위해 만들어졌습니다.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA 섹션 -->
    <section class="py-16 bg-gradient-to-r from-blue-600 to-blue-800 text-white">
      <div class="container mx-auto px-4 text-center">
        <h2 class="text-3xl font-bold mb-6">지금 바로 시작하세요</h2>
        <p class="text-xl mb-8 max-w-3xl mx-auto">Referee Helper와 함께 축구 규칙을 체계적으로 학습하고 더 나은 심판으로 성장해보세요.</p>
        <div class="flex flex-wrap justify-center gap-4">
          <NuxtLink to="/quiz" class="px-8 py-4 bg-red-500 hover:bg-red-600 text-white font-bold rounded-lg text-lg transition-colors duration-200">
            문제 풀기
          </NuxtLink>
          <NuxtLink
            to="/login"
            class="px-8 py-4 bg-transparent border-2 border-white hover:bg-white/10 text-white font-bold rounded-lg text-lg transition-colors duration-200"
          >
            회원가입
          </NuxtLink>
        </div>
      </div>
    </section>

    <!-- 연락처 정보 -->
    <section class="py-16 bg-gray-50">
      <div class="container mx-auto px-4 text-center">
        <h2 class="text-3xl font-bold mb-8">문의하기</h2>
        <p class="text-gray-700 mb-6 max-w-2xl mx-auto">
          서비스 이용에 관한 질문이나 제안사항이 있으시면 언제든지 연락주세요. 더 나은 서비스를 위해 여러분의 의견을 기다립니다.
        </p>
        <div class="inline-flex items-center justify-center bg-white px-6 py-3 rounded-full shadow-md">
          <span class="material-icons text-blue-600 mr-2">email</span>
          <span class="text-gray-700">contact@referee-helper.com</span>
        </div>
      </div>
    </section>

    <!-- 푸터는 기존 메인 페이지와 동일하게 사용한다고 가정 -->
  </div>
</template>

<script setup>
import { onMounted } from 'vue';

// 필요한 경우 페이지 특정 로직을 여기에 추가

onMounted(() => {
  // 페이지 로드 시 스크롤을 맨 위로 이동
  window.scrollTo(0, 0);
});
</script>

<style scoped>
/* Material Icons 폰트 임포트 */
@import url('https://fonts.googleapis.com/icon?family=Material+Icons');
/* Pretendard 폰트 임포트 */
@import url('https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css');
</style>
