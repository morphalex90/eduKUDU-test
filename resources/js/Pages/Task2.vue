<script setup lang="ts">
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, Link, useForm, useDebounceFn } from '@inertiajs/vue3';
import InputLabel from '@/Components/InputLabel.vue';
import { ref, watch, onMounted } from 'vue';
import { debounce } from 'lodash'
import less from 'less';

const form = useForm({
    html: '<div>Test</div><i>This is dynamic text and style</i>',
    less: 'body {margin: 0px;} div {background-color: red; color: #FFF; padding: 10px; margin-bottom: 10px} i {color: #FFF}',
});

const preview = ref(null);

const compileLess = async (lessInput) => {
    try {
        const output = await less.render(lessInput);
        return output.css;
    } catch (error) {
        console.error('Less compilation error:', error);
        return '';
    }
};

const updatePreview = async () => {
    const css = await compileLess(form.less);
    const previewDocument = preview.value.contentDocument

    previewDocument.open()
    previewDocument.write(`
      <html>
        <head>
          <style>${css}</style>
        </head>
        <body>${form.html}</body>
      </html>
    `);
    previewDocument.close();
};

const debouncedFn = debounce(() => {
    updatePreview()
}, 1000)

onMounted(() => updatePreview());

watch([form], debouncedFn, { immediate: true });
</script>

<template>

    <Head title="Task 2" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">Task 2</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg p-10">
                    <div class="flex gap-10">
                        <div class="w-50">
                            <form>
                                <div class="mb-5">
                                    <InputLabel for="html" value="HTML" />
                                    <textarea class="mt-1 block w-full" v-model="form.html" />
                                </div>

                                <InputLabel for="less" value="Less" />
                                <textarea class="mt-1 block w-full" v-model="form.less" />
                            </form>
                        </div>
                        <iframe ref="preview" class="preview"></iframe>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
