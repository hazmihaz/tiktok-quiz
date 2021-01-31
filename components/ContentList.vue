<template>
    <div class="scroll-container" ref="container">
        <div
            class="scroll-child"
            v-for="(item, i) in items"
            :key="i"
            :ref="`child`"
            :data-index="i"
        >
            <Content v-bind="item" @done="nextPage" />
        </div>
    </div>
</template>

<script>
import content from '@/content'

export default {
    components: {
        Content: () => import('@/components/Content'),
    },

    data: () => ({
        items: content.map((el) => ({
            ...el,
            active: false,
        })),
        observer: null,
    }),

    methods: {
        nextPage() {
            this.$refs.container.scrollBy({
                top: window.innerHeight,
                left: 0,
                behavior: 'smooth',
            })
        },

        createIntersectionObserver() {
            let options = {
                root: document.querySelector('#scrollArea'),
                rootMargin: '0px',
                threshold: 0.1,
            }
            this.observer = new IntersectionObserver(
                this.handleIntersection,
                options
            )
            this.$refs.child.forEach((target) => {
                this.observer.observe(target)
            })
        },

        handleIntersection(entries) {
            entries.map((entry, i) => {
                if (entry.isIntersecting) {
                    this.setInactiveAll()
                    const i = entry.target.getAttribute('data-index')
                    console.log('set active', i)
                    this.setActive(i)
                }
            })
        },

        setActive(i) {
            this.items[i].active = true
        },

        setInactiveAll() {
            this.items.forEach((el) => (el.active = false))
        },
    },

    mounted() {
        this.createIntersectionObserver()
    },
}
</script>

<style scoped>
.scroll-container {
    height: 100%;
    overflow-y: auto;
    scroll-snap-type: y mandatory;
}
.scroll-child {
    height: 100%;
    scroll-snap-align: start;
}
</style>
