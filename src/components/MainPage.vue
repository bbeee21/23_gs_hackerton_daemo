<template>
<div class="caintaner">
    <!-- heading -->
    <div class="logo">
        <img src="https://upload.wikimedia.org/wikipedia/commons/3/38/GS_logo_%28South_Korean_company%29.svg" alt="GS 로고" />
        <br>
    </div>
    <br>
    <h1 class="text-center">커피찌꺼기 배출 현황</h1>
    <br>
    <!-- input -->
    <div>
        <!-- <input type="text" v-model="task" placeholder="Enter here.." /> -->
        <!-- <button class="add-btn" @click="SubmitTask">ADD</button>
        <br><br>
        <button class="simulation" @click="makeSimulation">Simulation</button> -->
    </div>
    <!-- table -->
    <table class="table" border={3}>
        <thead>
            <tr>
                <th>매장번호</th>
                <th>매장정보</th>
                <th>주소</th>
                <th>배출현황</th>
                <th>관제화면</th>
                <!-- <th>Delete</th>
                <th>Edit</th> -->
                

            </tr>
        </thead>
        <tbody>
            <tr v-for="(task,index) in tasks" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{task.id}}</td>
                <td>{{task.name}}</td>
                <td 
                    v-bind:style="getStyle(task.number)"
                    :class="{ 'blink': task.number >= 80 }">
                    {{ task.number }}
                </td>
                <td>
                    <button
                        class="btn btn-primary"
                        @click="showRandomImage"
                        data-toggle="modal" data-target="#exampleModal"    
                    >
                        <i class="bi bi-camera"></i>
                    </button>
                </td>
                
                <!-- <td>
                    <button class="btn btn-danger del-btn" @click="deleteTask(index)">Delete</button>
                </td>
                <td>
                    <button class="btn btn-warning edit-btn" @click="EditTask(index)">Edit</button>
                </td> -->
            </tr>

        </tbody>
    </table>

</div>
</template>

<script>
export default {
    
    name: "TodoApp",
    props: {
        msg: String,
    },
    data() {
        return {
            task: "",
            editTask: null,
            tasks: [{
                    name: "서울 강남구 테헤란로77길7(삼성2동 143-28, 동원빌딩)",
                    id: "GS25강남동원점",
                    number: 2,
                },
                {
                    name: "서울 강남구 압구정로54길16, 1층 (신사동 662-18)",
                    id: "GS25강남로데오점",
                    number: 70,
                },
                {
                    name: "서울 강남구 밤고개로21길25, 상가동 지상1층 103호, 104호 (자곡동 361)",
                    id: "GS25강남세곡점",
                    number: 30,
                },
                {
                    name: "서울 강남구 헌릉로569길21-30, 104호 (세곡동 615, 드림하이오피스텔)",
                    id: "GS25강남드림하이점",
                    number: 5,
                },
                {
                    name: "서울 강남구 역삼로15길20 (역삼동 749-5)",
                    id: "GS25강남사랑점",
                    number: 20,
                },
                {
                    name: "서울 강남구 테헤란로4길46, 109호(역삼1동 826-37, 쌍용플래티넘)",
                    id: "GS25강남쌍용점",
                    number: 1,
                },
                {
                    name: "서울 강남구 강남대로102길40, 1층 (역삼동 618-3)",
                    id: "GS25강남아트점",
                    number: 35,
                },
                {
                    name: "서울 강남구 테헤란로115, 1층(역삼1동 649-10, 서림빌딩)",
                    id: "GS25강남타운점",
                    number: 3,
                },
                {
                    name: "서울 강남구 개포로20길12(개포4동 1232)",
                    id: "GS25개포럭키점",
                    number: 8,
                },
            ],
            images: [
                "https://picsum.photos/300/200",
                "https://picsum.photos/seed/picsum/300/200",
                "https://picsum.photos/id/237/300/200",
            ],
            // 선택된 이미지
            selectedImage: "",
    
        }
    },
    methods: {
        // delete task
        deleteTask(index) {
            this.tasks.splice(index, 1)
        },

        // edit task
        EditTask(index) {
            this.task = this.tasks[index].name,
                this.editTask = index
        },

        // Add Task
        SubmitTask() {
            if (this.task.length === 0) {
                return;
            }
            if (this.editTask != null) {
                this.tasks[this.editTask].name = this.task;
                this.editTask = null

            } else {
                this.tasks.push({
                    name: this.task,
                    id: Date.now() + 4,
                })
            }
        },
        
    // 숫자 업데이트와 배경색 변경 함수 추가
        updateNumbers() {
        for (let i = 0; i < this.tasks.length; i++) {
            let task = this.tasks[i];
            if (task.number < 100) {
            // 100보다 작을 때만 숫자 증가
            task.number += Math.floor(Math.random() * 5) + 1;
            if (task.number >= 80 && task.number <= 100) {
                task.color = "red";
            } else if (task.number >= 100) {
                task.number = 100;
                task.color = "black";
                /// 100 이상인 경우 5초 후에 0으로 돌아가도록 설정
                setTimeout(() => {
                    task.number = 0;
                    task.color = "";
                }, 5000);
                }
            }
        }
        },
        getStyle(number) {
        let style = {};
        if (number <= 50) {
            style.backgroundColor = "";
            style.color = "black";
        } else if (number > 50 && number < 70) {
            style.backgroundColor = "lightgreen"
            style.color = "white"
        } else if (number >= 70 && number <= 90) {
            style.backgroundColor = "rgb(255,160,122)"
            style.color = "white"
        } else if (number <= 100) {
            style.backgroundColor = "rgb(255,0,0)"
            style.color = "white"
        } else {
            style.backgroundColor = ""
        }

          // 숫자가 100을 넘을 경우 최대값 100으로 설정

        
        return style;

        },

        
        // 랜덤 이미지 선택
        selectImage() {
        const index = Math.floor(Math.random() * this.images.length);
        this.selectedImage = this.images[index];
        },

    },
    mounted() {
    setInterval(this.updateNumbers, 1000); // 1초마다 숫자 업데이트
    },

}
</script>

<style scoped>
@keyframes blink-effect {
    50% {
        opacity: 0;
    }
}
td, th {
    white-space: nowrap;
}
.blink {
    animation: blink-effect 1s step-end infinite;
}

.logo {
  display: inline-block; /* h1 태그 옆에 이미지를 배치하기 위한 인라인 블록 처리 */
  margin-right: 20px; /* h1 태그와 이미지 간의 간격 조정 */
  margin-bottom: 20px;
}
.logo img {
  height: 50px; /* 로고 이미지의 크기 조정 */
  vertical-align: middle; /* h1 태그의 텍스트와 수직 정렬 */
}

.caintaner {
    /* width: 600px; */
    margin: auto;

}

table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
    margin-top: 20px;
}

td,
th {
    border: 1px solid #dddddd;
    text-align: center;
    padding: 8px;
}

tr:nth-child(even) {
    background-color: #dddddd;
}
/* 
.add-btn {
    border: none;
    width: 100px;
    height: 30px;
    padding: 2px;
    background-color: teal;
    color: white;
}

.del-btn {
    border: none;
    background-color: red;
    color: white;
}

.edit-btn {
    border: none;
    background-color: #77b631;
    color: white;

} */

</style>
