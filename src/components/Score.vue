<template>
    <div class="scoreboard">
        <p>Accuracy: {{ this.computeAccuracy }}%</p>
        <p v-if="finished">Score: {{ this.score }} WPM</p>
        <p v-if="!finished">Timer: {{ this.time }} Secs</p>
    </div>
</template>
<script>
export default {
    props: {
        textLength: {required: true}
    },
    data() {
        return {
            errors: 0,
            correct: 0,
            score: 0,
            finished: false,
            time: 60,
            timer: null,
        }
    },
    created() {
        //__error made defined in TextInput.vue__
        Event.$on("error-made", () => {
            this.errors += 1;
        });

        //__correct defined in TextInput.vue__
        Event.$on("correct", () => {
            this.correct += 1; 
        });

        //__time-out defined in TextInput.vue__
        //When the 60 seconds is finished calculate the score and set finished var to 
        // true then stop the countdown timer 
        Event.$on("time-out", toObject => {
            let score = (toObject.phrase.length / 5) - toObject.uncorrectedErrors;
            this.score = (score > 0 ? +score.toFixed(2) : 0);
            this.finished = true;
            clearInterval(this.timer);
        });
        
        //__init defined in App.vue__
        //initialize all data variables
        Event.$on("init", () => {
            this.errors = 0;
            this.correct = 0;
            this.score = 0;
            this.finished = false;
            this.time = 60;
            this.uncorrected_errors = 0;
            clearInterval(this.timer)
        });
        
        //__start-timer is defined in TextInput.vue__
        //start a countdown timer that will be displayed to the user
        Event.$on("start-timer", () => {
            this.timer  =  setInterval(() => {
                this.time -= 1;
            }, 1000)  
        });
    },
    computed: {
        //compute the accuracy in real time of what the user has typed so far.
        computeAccuracy() {
            let accuracy = (this.correct / (this.correct + this.errors)) * 100;
            if (isNaN(accuracy)) {
                return 0;
            } else {
                return +accuracy.toFixed(2);
            }
        }
    }
}
</script>

<style>
.scoreboard {
    color: #CCCCCC;
}
</style>
