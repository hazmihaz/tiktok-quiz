<template>
    <div class="w-full h-full">
        <video ref="video" loop class="w-full h-full object-cover">
            <source :src="src" type="video/mp4" />
        </video>
        <div
            class="absolute w-full h-full top-0 left-0 flex justify-center items-center"
        >
            <div v-if="!playing" class="opacity-50">
                <svg
                    width="64"
                    height="64"
                    viewBox="0 0 36 36"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        d="M27.75 18.799L12 27.8923C11 28.4697 9.75 27.748 9.75 26.5933L9.75 8.40673C9.75 7.25203 11 6.53034 12 7.10769L27.75 16.201C28.75 16.7783 28.75 18.2217 27.75 18.799Z"
                        fill="white"
                        stroke="white"
                    />
                </svg>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        src: String,
        playing: Boolean,
    },

    computed: {
        video() {
            return this.$refs.video
        },
    },

    data: () => ({
        ready: false,
    }),

    watch: {
        playing: {
            handler(val) {
                if (val) {
                    this.play()
                } else {
                    this.pause()
                }
            },
        },
    },

    methods: {
        initListeners() {
            this.video.addEventListener('canplay', (event) => {
                if (!this.ready) {
                    this.$emit('ready')
                    this.ready = true
                }
            })
        },

        play() {
            this.video.play()
        },

        pause() {
            this.video.pause()
        },

        reset() {
            this.video.currentTime = 0
        },
    },

    mounted() {
        this.initListeners()
    },
}
</script>
