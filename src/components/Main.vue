<template>
  <div class="main">
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
        <ButtonStart @start="startTimer"/>
        <ButtonRestart @restart="stop"/>
      </div>
  </div>
</template>

<script>

import SettingSessionTime from './SettingSessionTime'
import SettingBreakTime from './SettingBreakTime'
import ButtonStart from './ButtonStart'
import ButtonRestart from './ButtonRestart'

let countdownTimer = null

export default {
    components: {
        SettingSessionTime,
        SettingBreakTime,
        ButtonStart,
        ButtonRestart
    },
    data(){
        return{
            currentMode: null,
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
                this.startTimer()
            }else if(this.countdown == 0){
                this.startBreak()
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
        startTimer() {
            clearInterval(countdownTimer)
            this.currentMode = "Work"
            this.timer(this.workTime)
        },
        startBreak(){
            clearInterval(countdownTimer)
            this.currentMode = "Break"
            this.timer(this.breakTime)
        },
        stop(){
            clearInterval(countdownTimer)
            this.currentMode = null
            this.countdown = this.workTime
            this.$emit('resetCycle')
        },
        timer(duration){
            countdownTimer = setInterval( () => {
                
                if (duration == 0){
                    clearInterval(countdownTimer)
                }
                // decrement the timer by one second
                this.countdown = duration
                duration -= 1
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