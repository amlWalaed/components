<script setup>
import InputLabel from "./InputLabel.vue";
const props = defineProps({
    showPlaceholder: Boolean,
    iconWrapperWidth: Number,
});
</script>
<template>
    <InputLabel
        :class="[
            showPlaceholder
                ? 'peer-placeholder-shown:text-transparent'
                : 'peer-placeholder-shown:text-gray-400',
        ]"
        class="peer-placeholder-shown:fl__position peer-focus:!inline-start-3 inline-start-3 absolute text-left transition-all ease-in-out duration-300 pointer-events-none origin-top-left flex items-center text-primary peer-focus:text-primary peer-focus:scale-90 scale-90 peer-placeholder-shown:scale-100 peer-focus:text-xs text-xs peer-placeholder-shown:text-sm -translate-y-1/2 peer-focus:top-0 peer-placeholder-shown:top-1/2 top-0 rounded bg-white peer-focus:bg-white peer-placeholder-shown:bg-transparent px-1.5 peer-focus:px-1.5 peer-placeholder-shown:p-0"
    />
</template>

<style>
.peer:placeholder-shown ~ .peer-placeholder-shown\:fl__position {
    inset-inline-start: calc(v-bind(iconWrapperWidth) * 1rem + theme(inset.3));
}
</style>
