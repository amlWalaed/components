<script setup>
import { onMounted, ref, inject, computed } from "vue";
import FloatingLabel from "./FloatingLabel.vue";

const props = defineProps({
    type: {
        default: "text",
        type: String,
    },
    as: {
        type: String,
        default: "input",
    },
    id: String,
    modelValue: [String, Number],
    required: Boolean,
    ariaDescribedBy: String,
    endIcon: [String, Object, Function],
    startIcon: [String, Object, Function],
    iconClass: String,
    floatingLabel: String,
    showPlaceholder: Boolean,
});

defineEmits(["update:modelValue"]);

const field = inject("field", props);

const input = ref(null);

onMounted(() => {
    if (input.value.hasAttribute("autofocus")) {
        input.value.focus();
    }
});

defineExpose({ focus: () => input.value.focus() });

const iconPadding = 0.5;
const iconWidth = 1.125;
const iconWrapperWidth = iconPadding * 2 + iconWidth + 0.5;
</script>

<template>
    <div
        :style="{
            '--icon-wrapper-width': iconWrapperWidth + 'rem',
            '--icon-padding': iconPadding + 'rem',
            '--icon-width': iconWidth + 'rem',
        }"
        class="relative text-gray-600 isolate dark:text-gray-300 focus-within:text-primary"
    >
        <component
            :is="as"
            v-bind="$attrs"
            :id="field.id"
            :aria-describedby="field.ariaDescribedBy"
            ref="input"
            :type="type"
            :required="field.required"
            :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)"
            class="block w-full p-3 text-sm transition-colors duration-150 rounded shadow-sm focus:ring-0"
            :class="[
                'text-gray-600 dark:text-gray-400 focus:text-primary-700',
                'bg-gray-50 dark:bg-gray-900',
                field.invalid
                    ? 'border border-red-500  focus:border-primary-500'
                    : 'border-gray-200 dark:border-gray-700 hover:border-gray-300 dark:hover:border-gray-600 focus:border-primary-500 dark:focus:border-primary-500 hover:focus:border-primary-500',
                !!props?.floatingLabel
                    ? 'placeholder-transparent peer placeholder:text-[0px] '
                    : ' placeholder-gray-300 dark:placeholder-gray-600',
                !!props?.showPlaceholder
                    ? '!placeholder-gray-300 dark:!placeholder-gray-600 placeholder:!text-sm'
                    : '',
                {
                    'pis-[var(--icon-wrapper-width)]': !!props?.startIcon,
                    'pie-[var(--icon-wrapper-width)]': !!props?.endIcon,
                },
                {
                    'form-input': props.as === 'input',
                    'form-textarea': props.as === 'textarea',
                },
            ]"
        />

        <FloatingLabel
            v-if="!!props?.floatingLabel"
            :for="field.id"
            :value="floatingLabel"
            :required="field.required"
            :showPlaceholder="props?.showPlaceholder"
            :icon-wrapper-width="!!props.startIcon ? iconWrapperWidth : 0"
        />
        <div
            v-if="props?.startIcon"
            class="absolute inset-y-0 px-[var(--icon-padding)] grid border-e start-0 place-content-center border-gray-200"
        >
            <component
                :is="props.startIcon"
                :class="[
                    'w-[var(--icon-width)] transition-colors duration-150 aspect-square',
                    iconClass,
                ]"
            />
        </div>
        <div
            v-if="props?.endIcon"
            class="absolute inset-y-0 px-[var(--icon-padding)] grid border-s end-0 place-content-center border-gray-200"
        >
            <component
                :is="props.endIcon"
                :class="[
                    'w-[var(--icon-width)] transition-colors duration-150 aspect-square',
                    iconClass,
                ]"
            />
        </div>
    </div>
</template>
