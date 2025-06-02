<script setup lang="ts">
import {ref, watch} from "vue";
import XIcon from "../icons/XIcon.vue";
import CheckIcon from "../icons/CheckIcon.vue";
import {useFetch} from "../../store/useFetch.ts";
import ErrorPopup from "../ErrorPopup.vue";

const name = ref()
const participants = ref()
const type = ref()
const accessibility = ref()
const link = ref()
const cost = ref()
const showErrorPopup = ref(false);
const errorMessage = ref('');

const {
  fetchData
,error,data} = useFetch()
const emit = defineEmits(['update'])

const addActivity = () => {
  const activity = {
    activity: name.value,
    participants: parseInt(participants.value),
    type: type.value,
    accessibility: parseInt(accessibility.value),
    link: link.value,
    cost: cost.value
  }
  fetchData({
    url: `http://localhost:3000/activities`, method: 'POST', body: activity
  })
  // store.postActivity(activity)
  emit('update')
  name.value=null
  participants.value=null
  type.value= null
  accessibility.value=null
  link.value=null
  cost.value=null
}

const cancel = () => {
  name.value=null
  participants.value=null
  type.value= null
  accessibility.value=null
  link.value=null
  cost.value=null
}
watch(() => data.value, (newData) => {
  if (error.value) {
    errorMessage.value = error.value as string
    showErrorPopup.value = true
    setTimeout(() => {
      showErrorPopup.value = false
    }, 3000)
    return
  }
  if (newData) {
    errorMessage.value = data.value as string
    showErrorPopup.value = true
    setTimeout(() => {
      showErrorPopup.value = false
    }, 3000)
  }
})

</script>

<template>
  <div>
    <Transition name="slide-fade">
      <ErrorPopup :show="showErrorPopup"
                  :message="errorMessage"
                  @close="showErrorPopup = false"/>
    </Transition>
    <div class="activity_container">
      <h2>Add activity</h2>
      <div class="activity_details">
        <div class="activity_detail">
          <span>Name of activity: </span>
          <input type="text" v-model="name" placeholder="Enter activity name"/>
        </div>
        <div class="activity_detail"><span>Number of participants: </span>
          <input type="text" v-model="participants" placeholder="Enter activity participants"/>
        </div>
        <div class="activity_detail"><span>Type of activity: </span>
          <input type="text" v-model="type" placeholder="Enter activity type"/>
        </div>
        <div class="activity_detail">
          <span>Accessibility: </span>
          <input type="text" v-model="accessibility" placeholder="Enter activity accessibility"/>
        </div>
        <div class="activity_detail"><span>Link: </span>
          <input type="text" v-model="link" placeholder="Enter activity link"/>
        </div>
        <div class="activity_detail">
          <span>Cost: </span>
          <input type="text" v-model="cost" placeholder="Enter activity price"/>
        </div>
      </div>
      <div class="modify">
        <button @click="">
          <CheckIcon @click="addActivity"/>
        </button>
        <button @click="cancel">
          <XIcon/>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.activity_container {
  width: 500px;
  padding: 32px;
  border: 1px solid #646cff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 24px;
}
button{
  background-color: transparent;
  &:hover{
    border: none;
  }
}
.activity_details {
  display: flex;
  flex-direction: column;
  gap: 12px;
  place-items: center;
  @media screen and (max-width: 480px) {
    width: 80vw;
  }
}

.activity_detail {
  width: 400px;
  display: flex;
  justify-content: space-between;
  gap: 8px;
  flex-wrap: wrap;
  word-break: break-all;
}

p {
  color: #646cff;
  font-weight: 500;
}

input {
  border: 1px solid #646cff;
  background-color: transparent;
  width: initial;
  border-radius: initial;
  padding: 8px 8px;
}

.input-1 {
  font-size: 25px;
}

.modify {
  display: flex;
  gap: 24px;
}

.fav {
  cursor: pointer;
  padding-top: 48px;
  height: 32px;
  width: 32px;
  stroke: #d0d0d0;
  transition: 0.3s all ease;

  &:hover {
    stroke: #535bf2;
  }
}

.fav_filled {
  stroke: #646cff;
  fill: #646cff;

  &:hover {
    stroke: #535bf2;
    fill: #535bf2;
  }
}
</style>
