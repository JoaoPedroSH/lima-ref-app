<script setup>
import Axios from '@/service/Axios';
import { ref, onMounted } from 'vue';
import { loadingOpen, loadingClose } from '../../../utils/computeds';
import { useToast } from 'primevue/usetoast';

const toast = useToast();

const data = ref([]);

const getCountStatusByService = async () => {
    loadingOpen();
    try {
        const response = await Axios.get('/panel_control/status_payment_by_service');
        data.value = response.data;

        loadingClose();
    } catch (error) {
        toast.add({ severity: 'error', summary: 'Erro', detail: 'Erro ao buscar dados', life: 5000 });
        console.error(error);
        loadingClose();
    }
};

onMounted(() => {
    getCountStatusByService();
});
</script>
<template>
    <Toast />
    <div class="card">
        <div class="flex justify-content-between align-items-center mb-5">
            <h6>QUANTIDADE POR STATUS DE PAGAMENTO</h6>
        </div>
        <ul class="list-none p-0 m-0">
            <li v-for="status in data.values" :key="status.index" class="flex flex-column md:flex-row md:align-items-center md:justify-content-between mb-4">
                <div>
                    <span class="block text-500 font-medium mb-1 md:mb-0">{{ status.description }}</span>
                </div>
                <div class="mt-2 md:mt-0 flex align-items-center">
                    <div class="surface-100 border-round overflow-hidden w-10rem lg:w-6rem" style="height: 8px">
                        <div class="h-full" :style="'background-color:' + status.color + '; width:' + Math.floor((status.count / data.length) * 100) + '%'"></div>
                    </div>
                    <span class="ml-3 font-medium" :style="'color:' + status.color">{{ status.count }}</span>
                </div>
            </li>
        </ul>
    </div>
</template>
