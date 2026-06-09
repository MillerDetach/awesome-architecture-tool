<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps<{
  question: string
  options: string[]
  answer: number
  explanation?: string
}>()

const picked = ref<number | null>(null)
const answered = ref(false)

function choose(i: number) {
  if (answered.value) return
  picked.value = i
  answered.value = true
}
function reset() {
  picked.value = null
  answered.value = false
}
</script>

<template>
  <div class="quiz">
    <div class="quiz-q"><span class="quiz-icon">🤔</span>{{ question }}</div>
    <ul class="quiz-opts">
      <li
        v-for="(opt, i) in options"
        :key="i"
        :class="[
          'quiz-opt',
          answered && i === answer ? 'correct' : '',
          answered && i === picked && i !== answer ? 'wrong' : '',
        ]"
        @click="choose(i)"
      >
        <span class="quiz-mark">{{ String.fromCharCode(65 + i) }}</span>
        <span class="quiz-text">{{ opt }}</span>
        <span v-if="answered && i === answer" class="quiz-badge ok">✓</span>
        <span v-else-if="answered && i === picked" class="quiz-badge no">✗</span>
      </li>
    </ul>
    <div v-if="answered" class="quiz-exp">
      <strong>{{ picked === answer ? '✅ 答对了!' : '❌ 再想想——正确答案是 ' + String.fromCharCode(65 + answer) }}</strong>
      <p v-if="explanation">{{ explanation }}</p>
      <button class="quiz-reset" @click="reset">重做这题</button>
    </div>
  </div>
</template>

<style scoped>
.quiz {
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  padding: 18px 20px;
  margin: 24px 0;
  background: var(--vp-c-bg-soft);
}
.quiz-q { font-weight: 600; margin-bottom: 14px; line-height: 1.6; }
.quiz-icon { margin-right: 8px; }
.quiz-opts { list-style: none; padding: 0; margin: 0; }
.quiz-opt {
  display: flex; align-items: center; gap: 10px;
  padding: 10px 14px; margin: 8px 0;
  border: 1px solid var(--vp-c-divider); border-radius: 8px;
  cursor: pointer; transition: all 0.2s;
}
.quiz-opt:hover { border-color: var(--vp-c-brand-1); background: var(--vp-c-brand-soft); }
.quiz-opt.correct { border-color: #3c8772; background: rgba(60, 135, 114, 0.14); }
.quiz-opt.wrong { border-color: #d65c5c; background: rgba(214, 92, 92, 0.12); }
.quiz-mark {
  display: inline-flex; align-items: center; justify-content: center;
  width: 24px; height: 24px; border-radius: 50%;
  background: var(--vp-c-default-soft); font-size: 13px; font-weight: 600; flex-shrink: 0;
}
.quiz-text { flex: 1; }
.quiz-badge { font-weight: 700; }
.quiz-badge.ok { color: #3c8772; }
.quiz-badge.no { color: #d65c5c; }
.quiz-exp {
  margin-top: 14px; padding-top: 12px;
  border-top: 1px dashed var(--vp-c-divider); font-size: 14px;
}
.quiz-exp p { margin: 8px 0 0; color: var(--vp-c-text-2); line-height: 1.6; }
.quiz-reset {
  margin-top: 12px; font-size: 13px; padding: 5px 14px;
  border: 1px solid var(--vp-c-divider); border-radius: 6px;
  background: var(--vp-c-bg); cursor: pointer; transition: 0.2s;
}
.quiz-reset:hover { border-color: var(--vp-c-brand-1); }
</style>
