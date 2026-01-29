<script setup>
import { reactive, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import httpService from '@/services/httpService';

//Path Variable 값 가져올 때
const route = useRoute();

//주소이동할 때 사용한다.
const router = useRouter();

//reactive사용 이유 => 수정한값이 화면에도 보여지게 하기위해서 사용
const state = reactive({
  board: {
      id: 0,
      title: '',
      contents: ''
  }
});

const submit = async () => {
    //제목이 입력되어 있지 않으면 "제목을 입력해 주세요!" alert
    //내용이 입력되어 있지 않으면 "내용을 입력해 주세요!" alert
    //메소드 종료
    if(!state.board.title) { //빈문자열일때 라는 뜻
        alert('제목을 입력해 주세요!');
        return;
    } else if(!state.board.contents) {
        alert('내용을 입력해 주세요!')
        return;
    }

    const result = await (state.board.id ? httpService.update(state.board) : httpService.save(state.board));
    if(!result) {
      alert(`글 ${state.mode}에 실패하였습니다.`);
      return;
    }
    const path = state.board.id ? `/detail/${state.board.id}` : '/';
    router.push(path)

    // if(state.board.id === 0) {
    //   const result = await httpService.save(state.board);
    //   console.log('result:', result);

    //   //result가 성공이면 제목, 내용 적혀있는거 모두 삭제해 주세요.

    //   if(result === 1) { //데이터 타입과 내용이 모두 같다는 뜻은 === 사용.
    //     state.board.title = '',
    //     state.board.contents = '';
    //     alert('등록에 성공하였습니다.');
    //     router.push({
    //         path: '/'
    //     });
    //   } else {
    //       alert('등록에 실패하였습니다.');
    //   }
    // } else { //수정
    //     const result = await httpService.update(state.board);
    //     if(result) {
    //         router.push(`/detail/${state.board.id}`) //디테일 화면으로 이동!!!
    //     }
    // }
}

onMounted(async () => {
    if(route.params.id) {
      state.mode = '수정'
      const id = route.params.id;
      state.board = await httpService.findById(id);
    }
});

</script>

<template>
<h3>글쓰기</h3>
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