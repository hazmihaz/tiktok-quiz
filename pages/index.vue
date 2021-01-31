<template>
    <div class="main-container">
        <div class="main">
            <ContentList />
        </div>
    </div>
</template>

<script>
export default {
    components: {
        ContentList: () => import('@/components/ContentList'),
    },
}
</script>

<style scoped lang="postcss">
.main {
    height: 100vh;
    grid-column-start: 2;
    grid-row-start: 2;
    overflow: hidden;
    overflow-y: auto;
    background: #fff;

    @screen sm {
        max-height: 660px;
        border-radius: 24px;
    }
}
</style>
