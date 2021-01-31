<template>
    <div class="quiz">
        <div class="quiz-wrap pt-4 pb-2 px-4">
            <div
                class="quiz-question text-white font-bold mb-5 text-xl text-center"
            >
                {{ questionStr }}
            </div>
            <div class="quiz-choices">
                <button
                    v-for="(choice, i) in choices"
                    :key="i"
                    :class="btnClass(choice)"
                    @click="doAnswer(choice)"
                >
                    {{ choice }}
                </button>
            </div>
        </div>

        <audio v-if="correct" autoplay src="/correct.ogg" class="hidden" />
    </div>
</template>

<script>
export default {
    props: {
        question: String,
        answer: String,
        choices: {
            type: Array,
            default: () => [],
        },
    },

    data: () => ({
        incorrects: [],
        correct: false,

        timeoutId: null,
    }),

    computed: {
        btnClass() {
            return (choice) => {
                if (this.correct) {
                    if (choice === this.answer) {
                        return `btn btn-correct`
                    }
                }

                if (this.incorrects.includes(choice)) {
                    return `btn btn-incorrect`
                }

                return `btn`
            }
        },

        questionStr() {
            return this.correct
                ? this.question.replace('___', this.answer)
                : this.question
        },
    },

    methods: {
        doAnswer(choice) {
            if (choice === this.answer) {
                this.handleCorrectAnswer(choice)
            } else {
                this.handleWrongAnswer(choice)
            }
        },

        handleCorrectAnswer() {
            if (this.correct) return
            this.correct = true
            this.handleDone()
        },

        handleWrongAnswer(choice) {
            this.incorrects.push(choice)
        },

        handleDone() {
            this.timeoutId = setTimeout(() => {
                this.$emit('done')
            }, 1000)
        },
    },

    beforeDestroy() {
        clearTimeout(this.timeoutId)
    },
}
</script>

<style scoped lang="postcss">
.quiz-wrap {
    background: rgba(17, 24, 39, 0.8);
}
.quiz-choices {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    /* @apply -mx-2; */
}
.btn {
    @apply inline-block mx-2 mb-3 py-3 px-5 rounded-lg border-gray-900 bg-gray-900 text-gray-200 font-bold text-lg;
    transition: all 0.2s;
}
.btn-incorrect {
    @apply text-red-600;
}
.btn-correct {
    @apply bg-green-600;
}
</style>
