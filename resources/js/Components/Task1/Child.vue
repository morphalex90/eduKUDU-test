<script setup lang="ts">
import { ref, defineProps, defineEmits } from 'vue';

const props = defineProps(['bool', 'text'])

const localBool = ref(props.bool);
const localText = ref(props.text);

const emit = defineEmits(['update-bool', 'update-text']);

const updateParentBool = () => {
    emit('update-bool', {
        bool: localBool.value,
    });
};

const updateParentText = () => {
    emit('update-text', {
        text: localText.value,
    });
};

const toggleActive = () => {
    localBool.value = !localBool.value;
    updateParentBool();
};

</script>

<template>
    <div style="background-color: yellow;" class="p-5 mt-5 rounded-md">
        <div>Child</div>
        <div>Bool: {{ localBool }}</div>
        <div>Text: {{ localText }}</div>

        <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mb-1" ref="bool"
            @click="toggleActive" type="button">Update bool</button>

        <input type="text" class="mt-1 block w-full" v-model="localText" @input="updateParentText" />
    </div>
</template>
