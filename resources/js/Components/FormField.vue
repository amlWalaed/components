<script setup>
import InputLabel from "@/Components/InputLabel.vue";
import InputError from "@/Components/InputError.vue";
import InputHelper from "@/Components/InputHelper.vue";
import { v4 as uuid } from "uuid";
import { computed, provide } from "vue";

const props = defineProps({
    id: {
        type: String,
        default: () => `field-${uuid()}`,
    },
    label: String,
    required: Boolean,
    error: String,
    help: String,
    row: Boolean,
});

const ariaDescribedBy = computed(() => {
    return !!props.help ? `help-${uuid()}` : null;
});

provide(
    "field",
    computed(() => {
        return {
            ...props,
            invalid: !!props.error,
            ariaDescribedBy: ariaDescribedBy.value,
        };
    })
);
</script>

<template>
    <div class="flex flex-col gap-1">
        <div
            class="flex gap-3"
            :class="
                row ? 'max-sm:flex-col  flex-row sm:items-center ' : 'flex-col'
            "
        >
            <InputLabel
                class="!grow-0"
                v-if="props.label"
                :for="props.id"
                :required="props.required"
            >
                {{ props.label }}
            </InputLabel>
            <div>
                <slot v-bind="props" />
            </div>
        </div>

        <InputError v-if="props.error" :message="error"> </InputError>

        <InputHelper
            v-if="props.help"
            :id="ariaDescribedBy"
            :message="help"
        ></InputHelper>
    </div>
</template>
