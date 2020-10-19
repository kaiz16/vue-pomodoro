<template>
  <div class="main">
      <p v-if="paused && countdown">Paused</p>
      <p>{{currentMode == "Break" ? 'Take a short break' : currentMode}}</p>
      <h1>
          <!-- eslint-disable -->
          {{minutes < 10 ? '0' + minutes : minutes}}:{{seconds < 10 ? '0' + seconds : seconds}} 
           <!-- eslint-enable -->
        </h1>
      <div class="settings">
        <p>Session Length</p>
        <SettingSessionTime 
            :workTime="workTime" 
            @increment="workTime += 1 * 60"
            @decrement="workTime -= 1 * 60"
        />
        <p>Break Length</p>
        <SettingBreakTime :breakTime="breakTime"
            @increment="breakTime += 1 * 60"
            @decrement="breakTime -= 1 * 60"
        />
      </div>
      <div class="actions">

        <ButtonAction @click="startOrPauseTimer" 
            :iconClass="paused ? 'mdi-play-circle-outline' : 'mdi-pause'"/>
        <ButtonRestart @restart="stop"/>
      </div>
  </div>
</template>

<script>

import SettingSessionTime from './SettingSessionTime'
import SettingBreakTime from './SettingBreakTime'
import ButtonAction from './ButtonAction'
import ButtonRestart from './ButtonRestart'
let countdownTimer = null

export default {
    components: {
        SettingSessionTime,
        SettingBreakTime,
        ButtonAction,
        ButtonRestart
    },
    data(){
        return{
            currentMode: null,
            paused: true,
            // in seconds, multiply it by 60 for a minute
            workTime: 10 * 60,
            breakTime: 5 * 60,
            countdown: null,
        }
    },
    watch: {
        countdown: function(){
            // when it reaches to 0. 0 is false in javascript
            if (this.countdown == 0 && this.currentMode == "Break"){
                this.$emit('incrementCycle')
                this.startSession('Work')
            }else if(this.countdown == 0){
                this.startSession('Break')
            }
        },
    },
    computed: {
        minutes(){
            return parseInt(this.countdown / 60, 10)
        },
        seconds(){
            return parseInt(this.countdown % 60, 10)
        }
    },
    methods: {
        startOrPauseTimer() {
            this.paused = !this.paused
            this.startSession(this.currentMode || 'Work')
        },
        startSession(mode){
            clearInterval(countdownTimer)
            if (this.paused){
                return
            }
            this.currentMode = mode
            // if the countdown is available, it's paused, so just continue
            if (this.countdown){
                this.timer()
                return
            }
            if (mode == 'Work'){
                this.countdown = this.workTime
            }else if(mode == 'Break'){
                this.countdown = this.breakTime
            }
            this.timer()
        },
        stop(){
            clearInterval(countdownTimer)
            this.currentMode = null
            this.countdown = null
            this.paused = true
            this.$emit('resetCycle')
        },
        timer(){
            countdownTimer = setInterval( () => {
                
                if (this.countdown == 0){
                    clearInterval(countdownTimer)
                }
                // decrement the timer by one second
                --this.countdown;
            }, 1000);
        }
    }
}
</script>

<style>

.main{
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    border: 1px solid #b10046;
    border-radius: 20px;
    width: 20%;
    color: #fff;
    padding: 10px;
}

.settings{
    display: flex;
    justify-content: space-evenly;
    flex-direction: column;
}

.actions{
    display: flex;
    justify-content: center;
    flex-direction: row;
}
</style>