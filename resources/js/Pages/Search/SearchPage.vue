<script setup>
import {Head, Link, router, usePage} from '@inertiajs/vue3';
import ApplicationLayout from '@/Layouts/ApplicationLayout.vue';
import Sidebar from "@/Common/Sidebar.vue";
import {onMounted, ref} from 'vue';
import Button from "@/Components/Button.vue";

const props = defineProps({
    search: String,
    result: Object,
    numDocumentsInIndex: Number,
    isIndexing: Boolean,
});
const page = usePage();
const search = ref(props.search);
const processSearch = ref(false);

const handleSearch = () => {
    if (search.value) {
        processSearch.value = true;
        router.post('/search', {
                search: search.value
            }, {
                onFinish: (visit) => {
                    processSearch.value = false;
                }
            }
        );
    }
}

</script>

<template>
    <Head title="Новости"/>

    <ApplicationLayout>

        <div class="flex flex-col md:flex-row">
            <div class="flex flex-col lg:flex-row w-full">
                <div class="bg-white p-4 w-full">

                    <div class="flex flex-row justify-between mt-4 mb-4">

                        <div class="font-serif text-4xl">Поиск</div>

                        <div>
                            <div class="text-xs text-right">
                                <p>Элементов в индексе: {{ numDocumentsInIndex }}</p>
                                <p v-if="isIndexing"><span class="text-red-600">идёт индексация базы данных..</span></p>
                                <!--                                <p><a href="/search/status" class="link">Статус поисковой базы</a></p>-->
                            </div>
                        </div>

                    </div>
                    <div class="flex flex-row">
                        <input type="text" autofocus
                               class="w-full flex-1 p-2 border border-gray-300 rounded focus:border-gray-300 focus:outline-none focus:ring-1 focus:ring-gray-300"
                               v-model="search" @keyup.enter="handleSearch"
                               placeholder="Поиск...">
                        <Button class="ml-2" @click="handleSearch">Искать</Button>
                    </div>
                    <div v-if="page.props.errors.search" class="text-red-600 mt-2">{{ page.props.errors.search }}</div>

                    <div v-if="processSearch" class="my-4 italic">выполняется поиск...</div>

                    <div class="search-page mt-8">

                        <div v-for="suttaSearchData in result" class="mb-8">
                            <div class="text-xl">{{ suttaSearchData.name }}</div>
                            <div v-for="textResultData in suttaSearchData.textResults" class="ml-4 mt-4">
                                <Link :href="textResultData.url">
                                    <div v-html="textResultData.html"></div>
                                </Link>
                                <Link :href="textResultData.url" class="link text-sm">
                                    {{ textResultData.fullUrl }}
                                </Link>
                            </div>

                        </div>

                    </div>


                </div>
            </div>

            <div class="md:ml-4 md:w-96">
                <Sidebar/>
            </div>

        </div>


    </ApplicationLayout>
</template>
