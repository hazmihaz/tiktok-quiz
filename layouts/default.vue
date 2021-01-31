<template>
    <div>
        <Nuxt />
    </div>
</template>

<style lang="postcss">
.main-container {
    min-height: 100vh;
    background: #181c22;

    @screen sm {
        display: grid;
        grid-template-columns: 1fr minmax(auto, 360px) 1fr;
        grid-template-rows: 1fr minmax(auto, 640px) 1fr;
        padding: 32px;
    }
}
</style>
