<template>
  <div class="referee-helper">
    <!-- 히어로 섹션 -->
    <section class="pt-32 pb-20 bg-gradient-to-r from-blue-600 to-blue-800 text-white">
      <div class="container mx-auto px-4">
        <div class="grid md:grid-cols-2 gap-8 items-center">
          <div>
            <h2 class="text-4xl md:text-5xl font-bold mb-6">심판의 판정 능력,<br />한 단계 더 높이세요</h2>
            <p class="text-xl mb-8">24/25 IFAB 기준의 문제로 실전 감각을 키우고<br />더 정확한 판정을 내릴 수 있습니다.</p>
            <div class="flex flex-wrap gap-4">
              <NuxtLink to="/quiz/all" class="px-6 py-3 bg-red-500 hover:bg-red-600 text-white font-bold rounded-lg transition-colors duration-200">
                지금 시작하기
              </NuxtLink>
              <NuxtLink
                to="/about"
                class="px-6 py-3 bg-transparent border-2 border-white text-white font-bold rounded-lg hover:bg-white/10 transition-colors duration-200"
              >
                서비스 소개
              </NuxtLink>
            </div>
          </div>
          <div class="mt-8 md:mt-0">
            <!-- <img src="/api/placeholder/550/400" alt="심판 이미지" class="rounded-lg shadow-xl mx-auto" /> -->
          </div>
        </div>
      </div>
    </section>

    <!-- 기능 소개 섹션 -->
    <section class="py-16 bg-white">
      <div class="container mx-auto px-4">
        <h2 class="text-3xl font-bold text-center mb-12">주요 기능</h2>

        <div class="grid md:grid-cols-3 gap-8">
          <div
            class="p-6 bg-white rounded-lg shadow-md hover:shadow-lg hover:-translate-y-1 transition-all duration-300"
            v-motion
            :initial="{ opacity: 0, y: 50 }"
            :visible="{ opacity: 1, y: 0 }"
          >
            <div class="flex justify-center mb-4">
              <span class="material-icons text-4xl text-blue-600">quiz</span>
            </div>
            <h3 class="text-xl font-bold mb-3 text-center">최신 규정 기반 문제</h3>
            <p class="text-gray-600 text-center">24/25 IFAB 기준으로 업데이트된 최신 문제로 실전 감각을 유지하세요.</p>
          </div>

          <div
            class="p-6 bg-white rounded-lg shadow-md hover:shadow-lg hover:-translate-y-1 transition-all duration-300"
            v-motion
            :initial="{ opacity: 0, y: 50 }"
            :visible="{ opacity: 1, y: 0, transition: { delay: 200 } }"
          >
            <div class="flex justify-center mb-4">
              <span class="material-icons text-4xl text-blue-600">history</span>
            </div>
            <h3 class="text-xl font-bold mb-3 text-center">풀어본 문제 모아보기</h3>
            <p class="text-gray-600 text-center">이미 풀어본 문제들을 한 눈에 확인하고 학습 진행상황을 파악하세요.</p>
          </div>

          <div
            class="p-6 bg-white rounded-lg shadow-md hover:shadow-lg hover:-translate-y-1 transition-all duration-300"
            v-motion
            :initial="{ opacity: 0, y: 50 }"
            :visible="{ opacity: 1, y: 0, transition: { delay: 400 } }"
          >
            <div class="flex justify-center mb-4">
              <span class="material-icons text-4xl text-blue-600">replay</span>
            </div>
            <h3 class="text-xl font-bold mb-3 text-center">다시 풀어보기</h3>
            <p class="text-gray-600 text-center">약점 분야의 문제를 반복 학습하여 완벽한 이론 지식을 쌓아보세요.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- 통계 섹션 -->
    <section class="py-16 bg-gray-50">
      <div class="container mx-auto px-4">
        <h2 class="text-3xl font-bold text-center mb-12">학습 효과</h2>

        <div class="grid md:grid-cols-4 gap-8 text-center">
          <div class="p-6 bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow duration-300" ref="statCard1">
            <div class="text-4xl font-bold text-blue-600 mb-2">{{ stats.questions }}+</div>
            <p class="text-gray-600">문제 수</p>
          </div>

          <div class="p-6 bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow duration-300" ref="statCard2">
            <div class="text-4xl font-bold text-blue-600 mb-2">{{ stats.categories }}</div>
            <p class="text-gray-600">카테고리</p>
          </div>

          <div class="p-6 bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow duration-300" ref="statCard3">
            <div class="text-4xl font-bold text-blue-600 mb-2">{{ stats.improvement }}%</div>
            <p class="text-gray-600">평균 점수 향상</p>
          </div>

          <div class="p-6 bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow duration-300" ref="statCard4">
            <div class="text-4xl font-bold text-blue-600 mb-2">{{ stats.users }}+</div>
            <p class="text-gray-600">사용자</p>
          </div>
        </div>
      </div>
    </section>

    <!-- 추천 학습 코스 섹션 -->
    <section class="py-16 bg-white">
      <div class="container mx-auto px-4">
        <h2 class="text-3xl font-bold text-center mb-12">추천 학습 코스</h2>

        <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div
            class="bg-white rounded-lg shadow-md overflow-hidden border border-gray-200 hover:shadow-lg hover:-translate-y-1 transition-all duration-300"
            v-for="(course, index) in courses"
            :key="index"
            v-motion
            :initial="{ opacity: 0, scale: 0.9 }"
            :visible="{ opacity: 1, scale: 1, transition: { delay: index * 100 } }"
          >
            <!-- <img :src="`/api/placeholder/400/${250 + index * 10}`" :alt="course.title" class="w-full h-48 object-cover" /> -->
            <div class="p-6">
              <div class="flex items-center justify-between mb-3">
                <span class="bg-blue-100 text-blue-800 text-xs px-2 py-1 rounded-full">{{ course.category }}</span>
                <span class="text-gray-500 text-sm">{{ course.questions }}문제</span>
              </div>
              <h3 class="text-xl font-bold mb-2">{{ course.title }}</h3>
              <p class="text-gray-600 mb-4">{{ course.description }}</p>
              <NuxtLink :to="course.link" class="text-blue-600 font-semibold hover:text-blue-800 transition-colors"> 학습 시작하기 → </NuxtLink>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 사용자 후기 섹션 -->
    <section class="py-16 bg-gray-50">
      <div class="container mx-auto px-4">
        <h2 class="text-3xl font-bold text-center mb-12">사용자 후기</h2>

        <div class="relative">
          <div class="flex transition-transform duration-500" :style="{ transform: `translateX(-${activeTestimonial * 100}%)` }">
            <div v-for="(testimonial, index) in testimonials" :key="index" class="w-full flex-shrink-0 px-4">
              <div class="p-8 bg-white rounded-lg shadow-md">
                <div class="flex items-center mb-4">
                  <div class="w-12 h-12 rounded-full bg-gray-300 flex items-center justify-center mr-4">
                    <span class="text-xl font-bold text-gray-600">{{ testimonial.name.charAt(0) }}</span>
                  </div>
                  <div>
                    <h4 class="font-bold">{{ testimonial.name }}</h4>
                    <p class="text-gray-600 text-sm">{{ testimonial.role }}</p>
                  </div>
                </div>
                <p class="text-gray-700 italic mb-4">"{{ testimonial.content }}"</p>
                <div class="flex text-yellow-400">
                  <span v-for="star in 5" :key="star" class="material-icons text-lg">
                    {{ star <= testimonial.rating ? 'star' : 'star_border' }}
                  </span>
                </div>
              </div>
            </div>
          </div>

          <button
            @click="prevTestimonial"
            class="absolute top-1/2 left-2 -translate-y-1/2 bg-white rounded-full w-10 h-10 flex items-center justify-center shadow-md z-10 hover:bg-gray-100 transition-colors"
          >
            <span class="material-icons">chevron_left</span>
          </button>

          <button
            @click="nextTestimonial"
            class="absolute top-1/2 right-2 -translate-y-1/2 bg-white rounded-full w-10 h-10 flex items-center justify-center shadow-md z-10 hover:bg-gray-100 transition-colors"
          >
            <span class="material-icons">chevron_right</span>
          </button>

          <div class="flex justify-center mt-6 space-x-2">
            <button
              v-for="(_, index) in testimonials"
              :key="index"
              @click="activeTestimonial = index"
              class="w-3 h-3 rounded-full transition-colors"
              :class="index === activeTestimonial ? 'bg-blue-600' : 'bg-gray-300'"
            ></button>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA 섹션 -->
    <section class="py-20 bg-gradient-to-r from-blue-600 to-blue-800 text-white">
      <div class="container mx-auto px-4 text-center">
        <h2 class="text-3xl md:text-4xl font-bold mb-6">지금 바로 실력을 향상시켜 보세요</h2>
        <p class="text-xl mb-8 max-w-3xl mx-auto">
          정확한 판정은 경기의 질을 높이고, 선수들의 신뢰를 얻는 첫 걸음입니다. Referee Helper와 함께 더 나은 심판이 되어보세요.
        </p>
        <NuxtLink to="/login" class="px-8 py-4 bg-red-500 hover:bg-red-600 text-white font-bold rounded-lg text-lg transition-colors duration-200">
          무료로 시작하기
        </NuxtLink>
      </div>
    </section>

    <!-- 푸터 -->
    <footer class="bg-gray-800 text-white py-10">
      <div class="container mx-auto px-4">
        <div class="grid md:grid-cols-4 gap-8">
          <div>
            <h3 class="text-xl font-bold mb-4">Referee Helper</h3>
            <p class="text-gray-400">더 나은 판정을 위한 심판 교육 플랫폼</p>
          </div>

          <div>
            <h4 class="font-bold mb-4">바로가기</h4>
            <ul class="space-y-2">
              <li><NuxtLink to="/" class="text-gray-400 hover:text-white transition-colors">홈</NuxtLink></li>
              <li><NuxtLink to="/quiz/all" class="text-gray-400 hover:text-white transition-colors">문제 풀이</NuxtLink></li>
              <!-- <li><NuxtLink to="/quiz/records" class="text-gray-400 hover:text-white transition-colors">학습 자료</NuxtLink></li> -->
              <li><NuxtLink to="/quiz/records" class="text-gray-400 hover:text-white transition-colors">내 기록</NuxtLink></li>
            </ul>
          </div>

          <div>
            <h4 class="font-bold mb-4">고객 지원</h4>
            <ul class="space-y-2">
              <li><NuxtLink to="/faq" class="text-gray-400 hover:text-white transition-colors">자주 묻는 질문</NuxtLink></li>
              <li><NuxtLink to="/contact" class="text-gray-400 hover:text-white transition-colors">문의하기</NuxtLink></li>
              <li><NuxtLink to="/terms" class="text-gray-400 hover:text-white transition-colors">이용약관</NuxtLink></li>
              <li><NuxtLink to="/privacy" class="text-gray-400 hover:text-white transition-colors">개인정보처리방침</NuxtLink></li>
            </ul>
          </div>

          <div>
            <h4 class="font-bold mb-4">뉴스레터 구독</h4>
            <p class="text-gray-400 mb-4">최신 규정 업데이트와 팁을 받아보세요.</p>
            <form @submit.prevent="subscribeNewsletter" class="flex">
              <input
                type="email"
                v-model="newsletterEmail"
                placeholder="이메일 주소"
                class="bg-gray-700 text-white px-4 py-2 rounded-l-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500"
                required
              />
              <button type="submit" class="bg-blue-600 hover:bg-blue-700 px-4 py-2 rounded-r-lg transition-colors">구독</button>
            </form>
          </div>
        </div>

        <div class="border-t border-gray-700 mt-8 pt-8 text-center text-gray-400">
          <p>&copy; {{ new Date().getFullYear() }} Referee Helper. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

// 스크롤 이벤트 관련 상태
const scrolled = ref(false);

// 통계 데이터
const stats = ref({
  questions: 500,
  categories: 8,
  improvement: 40,
  users: 1200,
});

// 추천 학습 코스 데이터
const courses = ref([
  {
    title: '초급 심판을 위한 기초 과정',
    category: '입문',
    questions: 50,
    description: '경기 규칙의 기본을 배우고 간단한 상황에서의 판정을 연습합니다.',
    link: '/courses/beginner',
  },
  {
    title: '중급 심판 실전 문제',
    category: '중급',
    questions: 75,
    description: '복잡한 경기 상황에서의 올바른 판정을 연습하는 문제들입니다.',
    link: '/courses/intermediate',
  },
  {
    title: '고급 판정 시나리오',
    category: '고급',
    questions: 60,
    description: '미묘한 규칙 적용과 까다로운 상황을 다루는 고난도 문제입니다.',
    link: '/courses/advanced',
  },
  {
    title: '오프사이드 심화 과정',
    category: '특화',
    questions: 40,
    description: '오프사이드 판정에 관련된 다양한 시나리오를 연습합니다.',
    link: '/courses/offside',
  },
  {
    title: '경기 재개 상황별 판정',
    category: '규칙',
    questions: 55,
    description: '경기 재개 시 발생할 수 있는 다양한 상황에 대한 문제입니다.',
    link: '/courses/restart',
  },
  {
    title: '24/25 규칙 변경사항 학습',
    category: '최신',
    questions: 30,
    description: '새롭게 변경된 24/25 시즌 규칙에 대한 적응 연습입니다.',
    link: '/courses/new-rules',
  },
]);

// 사용자 후기 데이터
const testimonials = ref([
  {
    name: '김민수',
    role: '2급 심판',
    content:
      '퀴즈를 통해 규칙을 복습하면서 실전에서 보다 자신감 있게 판정할 수 있게 되었습니다. 특히 규칙 변경사항을 빠르게 학습할 수 있어서 큰 도움이 됩니다.',
    rating: 5,
  },
  {
    name: '이지훈',
    role: '지역 리그 심판',
    content: '풀어본 문제를 다시 확인하는 기능이 정말 유용합니다. 제가 약한 부분을 집중적으로 공부할 수 있어서 판정 정확도가 많이 향상되었습니다.',
    rating: 4,
  },
  {
    name: '박수진',
    role: '축구 동호회 심판',
    content:
      '다양한 카테고리의 문제들이 있어서 전반적인 규칙 이해에 큰 도움이 됩니다. 동호회 경기를 볼 때도 훨씬 더 정확한 판정을 내릴 수 있게 되었어요.',
    rating: 5,
  },
]);

// 뉴스레터 구독 이메일
const newsletterEmail = ref('');

// 현재 표시중인 후기 인덱스
const activeTestimonial = ref(0);

// 스크롤 이벤트 핸들러
const handleScroll = () => {
  scrolled.value = window.scrollY > 50;
};

// 뉴스레터 구독 함수
const subscribeNewsletter = () => {
  // 실제 구현시 API 호출로 대체
  alert(`${newsletterEmail.value} 주소로 뉴스레터 구독 신청이 완료되었습니다.`);
  newsletterEmail.value = '';
};

// 후기 슬라이드 이동 함수
const nextTestimonial = () => {
  activeTestimonial.value = (activeTestimonial.value + 1) % testimonials.value.length;
};

const prevTestimonial = () => {
  activeTestimonial.value = (activeTestimonial.value - 1 + testimonials.value.length) % testimonials.value.length;
};

// 자동 슬라이드 타이머
let slideTimer;

// 컴포넌트 마운트시 이벤트 리스너 등록
onMounted(() => {
  window.addEventListener('scroll', handleScroll);

  // 5초마다 자동으로 다음 후기로 이동
  slideTimer = setInterval(nextTestimonial, 5000);
});

// 컴포넌트 언마운트시 이벤트 리스너 해제
onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
  clearInterval(slideTimer);
});
</script>

<style scoped>
/* Material Icons 폰트 임포트 */
@import url('https://fonts.googleapis.com/icon?family=Material+Icons');
/* Pretendard 폰트 임포트 */
@import url('https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css');
</style>
