<script setup>
import { reactive, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router'; //path variable데이터를 받을 때 필요객체
import httpService from '@/services/httpService';

const route = useRoute();
const router = useRouter();

const state = reactive({
    data: { //이 칸을 비워두어도 작동은 되지만 어떤값이 들어오는지 눈으로 확인할 수 있도록 기재해주는것이 좋음.
        id: 0,
        title: '',
        contents: '',
        createdAt: ''
    }
});

onMounted(async () => {
    console.log('typeof route.params.id: ', typeof route.params.id); //String타입
    console.log('route.params.id: ', route.params.id);
    const id = route.params.id;
    state.data = await httpService.findById(id);
    //findById 통신하고 결과를 state.data에 담아주세요.
})

const dodelete = async () => {
    if(!confirm('삭제하시겠습니까?') ) {
        return;
    }

    const params = { id:state.data.id }
    console.log(params);
    const result = await httpService.delete(params);
    console.log("Dodelete-result: " + result);

    if(result === 1) {
    router.push({
        path: '/'
    });
    // 단순하게 화면이동만 할 때는 {}안쓰고 ('/')만 해도됨
    }
}

// const update = () => {
//     const id = state.data.id;
//     router.push({
//         path: `/mod/${id}`
//     })
// }
</script>

<template>
<h3>Detail</h3>
<div>번호: {{ state.data.id }}</div>
<div>제목: {{ state.data.title }}</div>
<div>작성일: {{ state.data.createdAt }}</div>
<div>내용: {{ state.data.contents }}</div>
<div>
    <button @click="dodelete">삭제</button>
    <router-link :to="`/mod/${state.data.id}`">
        <button>수정</button>
    </router-link>
    <!-- <button @click="update">수정</button> -->
</div>
</template>

<style scoped>

</style>