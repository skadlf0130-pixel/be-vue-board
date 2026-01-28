<script setup>
import httpService from '@/services/httpService';
import { reactive, onMounted } from 'vue';
import { useRoute } from 'vue-router';

// router는 페이지 이동을 담당하는 객체이고,
// route는 현재 라우트의 경로·파라미터 정보를 담고 있는 객체

const route = useRoute();
const state = reactive({
    data:{
        id: 0,
        title: '',
        contents:'',
        createdAt:''
    }
});

onMounted(() =>{
    console.log('route.params.id: ',route.params.id)
    
})

onMounted(async() =>{
    const id = route.params.id;
    const result = await httpService.findById(id);
    state.data = result;

})
</script>

<template>
<h3>Detail</h3>
<div>글번호:{{ state.data.id }}</div>
<div>제목:{{ state.data.title }}</div>
<div>작성일:{{ state.data.createdAt }}</div>
<div>내용:{{ state.data.contents }}</div>
</template>

<style scoped>

</style>