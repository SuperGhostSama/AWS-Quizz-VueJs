<script setup>
import {ref,computed} from 'vue';

const questions =ref([
  {
    question: "Why is AWS more economical than traditional data centers for applications with varying compute workloads?",
    answer : 2,
    option : [
    "Amazon EC2 costs are billed on a monthly basis",
    "Users retain full administrative access to their Amazon EC2 instances.",
    "Amazon EC2 instances can be launched on demand when needed",
    "Users can permanently run enough instances to handle peak workloads.",
    ],
    selected : null
  },
  {
    question: "Which AWS service would simplify the migration of a database to AWS?",
    answer : 1,
    option : [
    "AWS Storage Gateway",
    "AWS Database Migration Service (AWS DMS)",
    "Amazon EC2",
    "Amazon AppStream 2.0",
    ],
    selected : null
  },
  {
    question: " Which AWS offering enables users to find, buy, and immediately start using software solutions in their AWS environment?",
    answer : 3,
    option : [
    "AWS Config",
    "AWS OpsWorks",
    "AWS SDK",
    "AWS Marketplace",
    ],
    selected : null
  },
  {
    question:"Which AWS networking service enables a company to create a virtual network within AWS?",
    answer : 3,
    option : [
    "AWS Config",
    "Amazon Route 53",
    "AWS Direct Connect",
    "Amazon Virtual Private Cloud (Amazon VPC)",
    ],
    selected : null
  },
  {
    question: "Which of the following is an AWS responsibility under the AWS shared responsibility model?",
    answer : 1,
    option : [
    "Configuring third-party applications",
    "Maintaining physical hardware",
    "Securing application access and data",
    "Managing guest operating systems",
    ],
    selected : null
  },
  {
    question: "Which component of the AWS global infrastructure does Amazon CloudFront use to ensure low-latency delivery?",
    answer : 1,
    option : [
    "AWS Regions",
    "Edge locations",
    "Availability Zones",
    "Virtual Private Cloud (VPC)",
    ],
    selected : null
  },
  {
    question: "How would a system administrator add an additional layer of login security to a user's AWS Management Console?",
    answer : 2,
    option : [
    "Use Amazon Cloud Directory",
    "Audit AWS Identity and Access Management (IAM) roles",
    "Enable multi-factor authentication",
    "Enable AWS CloudTrail",
    ],
    selected : null
  },
  {
    question: "Which service can identify the user that made the API call when an Amazon EC2 instance is terminated?",
    answer : 1,
    option : [
    "AWS Trusted Advisor",
    "AWS CloudTrail",
    "AWS X-Ray",
    "AWS Identity and Access Management (AWS IAM)",
    ],
    selected : null
  },
  {
    question: "Which service would be used to send alerts based on Amazon CloudWatch alarms?",
    answer : 0,
    option : [
    "Amazon Simple Notification Service (Amazon SNS)",
    "AWS CloudTrail",
    "AWS Trusted Advisor",
    "Amazon Route 53",
    ],
    selected : null
  },
  {
    question: "Where can a user find information about prohibited actions on the AWS infrastructure?",
    answer : 3,
    option : [
    "AWS Trusted Advisor",
    "AWS Identity and Access Management (IAM)",
    "AWS Billing Console",
    "AWS Acceptable Use Policy",
    ],
    selected : null
  },
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score =computed(() => {
  let value= 0
  questions.value.map(q => {
    if(q.selected==q.answer){
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed ( () =>{
  let question = questions.value [currentQuestion.value]
  question.index = currentQuestion.value
  return question 
} )

const SetAnswer = e => {
  questions.value[currentQuestion.value].selected = e.target.value
  e.target.value = null
}

const NextQuestion =() => {
  if(currentQuestion.value < questions.value.length - 1){
    currentQuestion.value++
    return
  }
  quizCompleted.value = true
}
</script>

<template>
  <main class="app">
    <h1>
      The AWS Quizz
    </h1>
    
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question"> {{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }}/{{ questions.length }} </span>
      </div>

      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.option" 
        :key="index" 
        :class="`option ${
          getCurrentQuestion.selected == index
          ? index == getCurrentQuestion.answer
          ?'correct' : 'wrong' : ''
        }${
          getCurrentQuestion.selected != null &&
          index != getCurrentQuestion.selected
                ? 'disabled'
                : ''
        }`">
          <input type="radio" 
          :name="getCurrentQuestion.index"
          :value="index"
          v-model="getCurrentQuestion.selected" 
          :disabled="getCurrentQuestion.selected"
          @change="SetAnswer">
          <span>{{ option }}</span>
        </label>
      </div>
      <button
        @click="NextQuestion"
        :disabled="!getCurrentQuestion.selected">
        {{ 
          getCurrentQuestion.index == questions.length -1
            ? 'finish'
          :getCurrentQuestion.selected == null
            ? 'Select an option'
            : 'Next question'
        }}
      </button>
    </section>

    <section v-else>
      <h2>You Have Finished The Quizz</h2>
      <p>Your Score is {{ score }}/{{ questions.length }}</p>
    </section>
    
  </main>
</template>

<style>
*{
  margin :0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body{
  background-color: #ffffff;
  color: #000000;
}
.app{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
}
h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}
.quiz{
  background-color: #a0a0a0;
  padding: 1rem;
  width: 100%;
  max-width: 600px;

}
.quiz-info{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;

}

.quiz-info .question{
  color : rgb(0, 0, 0);
  font-size: 1.5rem;
}
.quiz-info .score{
  color: rgb(255, 255, 255);
  font-size: 1.25;
}
.options{
  margin-bottom: 1rem;
}
.option{
  padding: 1rem;
  display: block;
  background-color: #7979a1;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;

}
.option.hover{
  background-color: #c29573;
}

.option.correct{
  background-color: #2cce7d;
}
.option.wrong{
  background-color: #ff5a5f;
}
.option:last-of-type{
  margin-bottom: 0;
}

.option.disabled{
  opacity: 0.5;
}
.option.input{
  display: none;

}
button{
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 1rem 2rem;
  margin-left: 12rem;
  background-color: #22ff00;
  color: #070707;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
}

button:disabled{
  opacity: 0.5;
}
h2{
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p{
  color: #1dc104;
  font-size: 1.5rem;
  text-align: center;
}
</style>
