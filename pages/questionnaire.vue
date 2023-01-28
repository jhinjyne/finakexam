<template>
    <div>
        <div class="container">
            <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" 
            rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" 
            crossorigin="anonymous">
    
    <modal v-if="showModal" @close="showModal = false">
        <div class="addQuestion">
            <h3 slot="header">{{ modalMode === 'add' ? 'Add a question' : 'Edit the selected question' }}</h3>
            <div slot="body">
            <label>Question:</label><br>
            <input id="questinput" style="border: 2px solid black; margin-left: 10px;"  type="text" v-model="currentQuestion.text" />
            <br><br>
            <label style="margin-left: 10px; margin-top: 10px;">Answer:</label><br>
            <input id="answerinput" style="border: 2px solid black; margin-left: 5px;" type="text" v-model="currentQuestion.answer" />
            <br>
            <br>
            <label style="margin-left: 10px; margin-top: 10px;">Choices:</label>
            <br>
            <textarea id="choicestext" v-model="currentQuestion.choices" style="height: 100px; width: 40%; margin-top: 20px;"></textarea>
            </div><br>
            <div slot="footer">
            <button id="btnsave" class="btn btn-success" @click="saveQuestion">Save</button>&nbsp;&nbsp;
            <button id="btnexit" class="btn btn-danger" @click="showModal = false">Exit</button>
            </div>
        </div>
    </modal>
    <div style="text-align: center;">
    <h1>Final Exam Questionnaire</h1><br>
</div>

<div class="buttons-container" style="display: flex; width:11%; margin: 0 auto;">
    <button id="addquestion" class="btn btn-primary" @click="addQuestion()">Add Question</button>
</div> 

<br>
    <table class="table">
        <thead class="thead-light">
        <tr>
            <th>No.</th>
            <th>Question</th>
            <th>Answer</th>
            <th style="width: 150px">Options</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(question, index) in questions" :key="index">
            <td style="padding-top: 30px">{{ index+1 }}</td>
            <td>{{ question.text }}<br><br>{{ question.choices }}</td>
            <td style="width: 300px; padding-top: 30px">
            <button v-if="!question.showAnswer" @click="question.showAnswer = true" id="btnShowAnswer">Show Answer</button>
            <button v-else @click="question.showAnswer = false" id="btnHideAnswer">{{ question.answer }}</button>
            </td>
            <td style="padding-top: 20px">
            <button class="btn" @click="editQuestion(index)" id="btnAlter">Alter</button>      
            <button class="btn" @click="deleteQuestion(index)" id="btnDel">Delete</button>
            </td>
        </tr>
        </tbody>
    </table>

    
    </div>
</div>
</template>
<style>
    #btnShowAnswer {
        position: absolute;
        background-color: #3f51b5;
        color: #fff;
        border:none; 
        border-radius:10px; 
        padding:15px;
        min-height:30px; 
        min-width: 120px;
    }
    #btnAlter {
        background-color: #4caf50;
        color: #fff;
        border:none; 
    }
    #btnDel {
        background-color: #f44336;
        color: #fff;
        border:none;
    }
    body {
        color: #333;
    }
    .addQuestion {
        text-align: center;
        padding: 30px;
        margin-left: 10%;
        margin-right: 10%;
        background-color: #e3f2fd;
        color: #333;
        border-radius: 10px;
    }
    .table {
        color: #333;
        background-color: #f2f2f2;
    }
    .table input, textarea {
        border: 2px solid #333;
    }
</style>
<script>

export default {
    name: "QuestionnairePage",
    data() {
    return {
        questions: [],
        currentQuestion: {
        text: '',
        answer: '',
        choices: '',
        showAnswer: false
        },
        showModal: false,
        modalMode: 'add'
    }
    },
    methods: {
    addQuestion() {
        this.currentQuestion = {
        text: '',
        answer: '',
        choices: '',
        showAnswer: false
        }
        this.modalMode = 'add'
        this.showModal = true
    },
    editQuestion(index) {
        this.currentQuestion = {...this.questions[index]}
        this.modalMode = 'edit'
        this.showModal = true
    },
    deleteQuestion(index) {
        this.questions.splice(index, 1)
    },
    saveQuestion() {
        if (this.modalMode === 'add') {
        this.questions.push(this.currentQuestion)
        }
        else if (this.modalMode === 'edit')
        {
            this.questions.splice(this.currentQuestion.text, 1)
            this.questions.push(this.currentQuestion)
        }
        this.showModal = false
    }
    }
    }
</script>