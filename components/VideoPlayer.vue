<template>
    <div class="w-full h-full">
        <video ref="video" loop class="w-full h-full object-cover">
            <source :src="src" type="video/mp4" />
        </video>
        <div
            class="absolute w-full h-full top-0 left-0 flex justify-center items-center"
        ></div>
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
