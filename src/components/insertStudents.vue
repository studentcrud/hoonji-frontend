<template>
    <div v-show="$store.state.modalOn" class="modal_wrapper">
        <div @click="toggle" class="modal_bg"></div>
        <div class="modal">
            <h1>학생 정보 신규 등록</h1>
            <input type="text" v-model="name" placeholder="이름">
            <input type="text" v-model="age" placeholder="나이">
            <input type="text" v-model="address" placeholder="주소">
            <select v-model="subject">
                <option value="" selected hidden>학과를 선택하세요</option>
                <option v-for="item in this.$store.state.subjects" :key="item.id" :value="item.subjectName">{{item.subjectName}}</option>
            </select>
            <select v-model="professor">
                <option value="" selected hidden>지도교수를 선택하세요</option>
                <option v-show="subject == item.subjectName" v-for="item in this.$store.state.professors" :key="item.id" :value="item.professorName">{{item.subjectName}} : {{item.professorName}} 교수</option>
            </select>
            <button @click="newData" class="colorbtn">등록</button>
        </div>
    </div>
</template>

<script>

import axios from 'axios';
axios.defaults.baseURL = 'http://172.16.28.167:8084';

export default{
    name: 'insertStudents',
    data(){
        return{
            name: "",
            age: "",
            address: "",
            subject: "",
            professor: ""
        }
    },
    methods:{
        toggle(){
            this.$store.commit('toggle');
        },
        newData(){
            axios.post('api/students', {
                studentName : this.name,
                studentAge : this.age,
                studentAddress : this.address,
                professorName : this.professor,
                subjectName : this.subject
            })
            .then((res)=>{
                console.log(res);
                this.$router.go();
            })
            .catch((err)=>{
                console.log(err);
            })
        }
    }
}
</script>