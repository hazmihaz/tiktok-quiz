<template>
    <div class="relative w-full h-full bg-red-500">
        <div class="h-full" @click="togglePlay">
            <VideoPlayer
                ref="player"
                :src="src"
                :playing="videoIsPlaying"
                @ready="onVideoReady"
                @no-autoplay="onVideoNoAutoplay"
            />
        </div>

        <div class="absolute bottom-0 w-full">
            <Quiz
                :question="question"
                :answer="answer"
                :choices="choices"
                @done="done"
            />
        </div>

        <div class="decors absolute right-0 top-0">
            <div>
                <svg
                    width="36"
                    height="36"
                    viewBox="0 0 36 36"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        d="M6.79688 14.2422C6.79688 19.2344 10.9805 24.1445 17.5898 28.3633C17.8359 28.5156 18.1875 28.6797 18.4336 28.6797C18.6797 28.6797 19.0312 28.5156 19.2891 28.3633C25.8867 24.1445 30.0703 19.2344 30.0703 14.2422C30.0703 10.0938 27.2227 7.16406 23.4258 7.16406C21.2578 7.16406 19.5 8.19531 18.4336 9.77734C17.3906 8.20703 15.6094 7.16406 13.4414 7.16406C9.64453 7.16406 6.79688 10.0938 6.79688 14.2422ZM8.68359 14.2422C8.68359 11.125 10.6992 9.05078 13.418 9.05078C15.6211 9.05078 16.8867 10.4219 17.6367 11.5938C17.9531 12.0625 18.1523 12.1914 18.4336 12.1914C18.7148 12.1914 18.8906 12.0508 19.2305 11.5938C20.0391 10.4453 21.2578 9.05078 23.4492 9.05078C26.168 9.05078 28.1836 11.125 28.1836 14.2422C28.1836 18.6016 23.5781 23.3008 18.6797 26.5586C18.5625 26.6406 18.4805 26.6992 18.4336 26.6992C18.3867 26.6992 18.3047 26.6406 18.1992 26.5586C13.2891 23.3008 8.68359 18.6016 8.68359 14.2422Z"
                        fill="#ffffff"
                    />
                </svg>
            </div>
            <div>
                <svg
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M17 3H7C5.9 3 5 3.9 5 5V21L12 18L19 21V5C19 3.9 18.1 3 17 3ZM17 18L12 15.82L7 18V5H17V18Z"
                        fill="#ffffff"
                    />
                </svg>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    components: {
        VideoPlayer: () => import('@/components/VideoPlayer'),
        Quiz: () => import('@/components/Quiz'),
    },

    props: {
        src: String,
        question: String,
        answer: String,
        choices: {
            type: Array,
            default: () => [],
        },
        active: Boolean,
    },

    data: () => ({
        videoIsPlaying: false,
    }),

    computed: {
        player() {
            return this.$refs.player
        },
    },

    watch: {
        active: {
            handler(val) {
                this.videoIsPlaying = val
            },
        },
    },

    methods: {
        togglePlay() {
            this.videoIsPlaying = !this.videoIsPlaying
        },

        onVideoReady() {
            if (this.active) this.videoIsPlaying = true
        },

        onVideoNoAutoplay() {
            this.videoIsPlaying = false
        },

        done() {
            this.player.pause()
            this.$emit('done')
        },
    },
}
</script>

<style lang="postcss" scoped>
.decors {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 12px;
}
.decors > div {
    margin-bottom: 8px;
}
</style>
