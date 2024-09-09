<script setup lang="ts">
import { reactive } from 'vue';
import Child from '@/Components/Task1/Child.vue'

const data = reactive({
    bool: false,
    text: 'This is changable text',
});

const updateBool = ({ bool }) => {
    data.bool = bool;
};

const updateText = ({ text }) => {
    data.text = text;
};

</script>

<template>
    <div style="background-color: red;" class="p-5">
        <div>Parent</div>
        <div>bool: {{ data.bool }}</div>
        <div>text: {{ data.text }}</div>
        <Child :bool="data.bool" :text="data.text" @update-bool="updateBool" @update-text="updateText" />
    </div>
</template>
