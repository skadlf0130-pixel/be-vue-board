<script setup>
import { reactive, onMounted } from 'vue';
import httpService from '@/services/httpService';
import { useRouter } from 'vue-router';


const state = reactive({
    list: []
});

const router = useRouter();

onMounted(async()=>{
    const result = await httpService.findAll();
    state.list = result;
});

const moveToDetail = id => {
    router.push(`/detail/${id}`)
    console.log ('moveToDetail-id:',id)
}

</script>

<template>
<h3> 게시판 리스트 </h3>
<table>
    <tr>
        <th>번호</th>
        <th>제목</th>
        <th>작성일</th>        
    </tr>


    <tr v-for="item in state.list" :key="item.id" @click="moveToDetail(item.id)">

        <td>{{ item.id }} </td>
        <td>{{ item.title }}</td> 
        <td>{{ item.createdAt }}</td>
    </tr>
</table>
</template>

<style scoped>
table {border-collapse: collapse;}
table,th,td {border: 1px solid #605d5d ;}
td {cursor: pointer;}
tr:hover{ background-color: rgb(223, 238, 249);}
th {background-color: #ccc;}
</style>