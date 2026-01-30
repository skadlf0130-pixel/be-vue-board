<script setup>
/* reactive의 역할  -객체를 “반응형 상태”로 만들어서, 값이 바뀌면 화면(Vue 템플릿)도 자동으로 같이 바뀌게 해주는 역할 */
import { onMounted, reactive } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import httpService from '@/services/httpService';

//주소 이동시 사용
const route = useRoute();
const router = useRouter();

const state = reactive({
    mode: '등록',
    board: {
        id: 0,
        title: '',
        contents: ''
    }
});

const submit = async () => {
    //제목이 입력되어 있지 않으면 "제목을 입력해 주세요!" alert
    //내용이 입력되어 있지 않으면 "내용을 입력해 주세요!" alert
    //alert가 뜨면 메소드 종료!
    if(!state.board.title) {
        alert('제목을 입력해 주세요!');
        return;
    } else if(!state.board.contents) {
        alert('내용을 입력해 주세요!');
        return;
    }

    const result = await (state.board.id ? httpService.update(state.board) : httpService.save(state.board));
    if(!result) {
        alert(`글 ${state.mode}에 실패하였습니다.`);
        return;
    }
    const path = state.board.id ? `/detail/${state.board.id}` : '/';
    router.push(path)
    
}

onMounted(async () => {
    if(route.params.id) {
        state.mode = '수정';
        const id = route.params.id;
        state.board = await httpService.findById(id);
    }
});

</script>

<!-- 프라미스(Promise) 객체 :지금은 결과가 없지만,
미래에 성공 또는 실패 결과를 약속하는 객체

async / await 한 줄 정의
비동기 코드를 동기 코드처럼 보이게 해주는 문법

async : 이 함수는 Promise를 반환한다
await : Promise가 끝날 때까지 기다린다
await는 async 함수 안에서만 가능
에러 처리는 try / catch
-->
<!-- v-model : 양방향이벤트 -->
<template>
<h3>네모네모 스펀지밥{{ state.mode }}</h3>
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