<script setup>
import { reactive } from 'vue';
import { useRouter } from 'vue-router';
import httpService from '@/services/httpService';

//주소 이동시 사용
const router = useRouter();

const state = reactive({
    board : {
    title : '',
    contents : ''
    }
});

const submit = async () =>{
    // 제목이 입력되어 있지 않으면 "제목을 입력해 주세요!" alert
    // 내용이 입력되어 있지 않으면 "내용을 입력해 주세요!" alert
    //alert가 뜨면 메소드 종료!
    if(state.board.title ==''){
        alert("제목을 입력해 주세요!");
        return;
    } else if(state.board.contents==''){
        alert("내용을 입력해 주세요!");
        return;
    } 
    const result = await httpService.save(state.board);
    console.log('result: ' , result);

    //result가 성공이면 제목 내용 적혀있는거 모두 삭제.
    if(result === '성공'){
        state.board = '';
        router.push({
            path:'/'
        })
    }
}
</script>

<template>
<h3>네모네모 스펀지밥</h3>
<div>
    <label>제목: <input type="text" v-model="state.board.title"></label>
</div>
<div>
    <label>내용: <textarea v-model="state.board.contents"></textarea></label>
</div>
<div>
    <button @click="submit">저장</button>
</div>

</template>

<style scoped>

</style>