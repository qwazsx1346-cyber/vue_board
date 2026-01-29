<script setup>
import { reactive, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import httpService from '@/services/httpService';

const router = useRouter();

const state = reactive({
    list: []
});


onMounted(async () => {
    //httpService페이지에 findAll함수에서 작업하는데 소요되는 시간만큼 받아야하기때문에
    //promise객체로 감싸서 이쪽 아래코드로 준다.
    const result = await httpService.findAll();
    state.list = result;
});

const moveToDetail = id => {
    //router와 id를 이용하여 주소 이동
    console.log('moveToDetail - id: ', id);
    router.push({
            path: `/detail/${id}`
    });
}
</script>

<template>
<h3>게시판 리스트</h3>
    <!-- state.list.length === 0으로 처리해도되지만 어차피 false이니까 앞에 !를 붙여주어도 됨-->
    <div v-if="!state.list.length"> 
        <p>작성된 글이 하나도 없습니다.</p>
    </div>
<table v-else>
    <tr>
        <th>번호</th>
        <th>제목</th>
        <th>작성일</th>
    </tr>
    
    <tr v-for="item in state.list" :key="item.id" @click="moveToDetail(item.id)">
        <td>{{ item.id }}</td>
        <td>{{ item.title }}</td>
        <td>{{ item.createdAt }}</td>
    </tr>
</table>
</template>

<style scoped>
table { border-collapse: collapse; }
table, th, td { border: 1px solid #ccc; }
th, td { padding: 5px;}

td { cursor: pointer; }
tr:hover { background-color: aliceblue; }
tr:first-child { background-color: rgb(128, 223, 218);}

</style>