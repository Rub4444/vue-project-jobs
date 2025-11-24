<script setup>
    import {RouterLink} from 'vue-router';
    import JobListing from '@/components/JobListing.vue';
    import { reactive, defineProps, onMounted } from 'vue';
    import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
    import axios from 'axios';
    
    defineProps({
        limit: {
            type: Number,
            default: null
        },
        showButton: {
            type: Boolean,
            default: false
        }
    });

    const state = reactive({
        jobs: [],
        isLoading: true
    });

    onMounted(async () => {
        try {
            const response = await axios.get('/api/jobs');
            state.jobs = response.data;
        } catch (error) {
            console.error('Error fetching jobs:', error);
        } finally {
            state.isLoading = false;
        }
    });
</script>

<template>
    <section class="py-10 px-4 bg-blue-50">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold mb-6 text-green-500">
                Browse Jobs
            </h2>

            <div v-if="state.isLoading" class="flex justify-center my-10">
                <PulseLoader :loading="state.isLoading" color="#22c55e" />
            </div>

            <div v-else class="grid grid-cols-1 gap-6 md:grid-cols-3">
                <JobListing v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job" />
            </div>
        </div>
    </section>

    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
        <RouterLink
        to="/jobs"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >View All Jobs</RouterLink
        >
    </section>
</template>