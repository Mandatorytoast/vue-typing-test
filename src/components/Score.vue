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
        Event.$on("error-made", () => {
            this.errors += 1;
        });

        Event.$on("correct", () => {
            this.correct += 1; 
        });

        Event.$on("time-out", toObject => {
            let score = (toObject.phrase.length / 5) - toObject.uncorrectedErrors;
            this.score = (score > 0 ? +score.toFixed(2) : 0);
            this.finished = true;
            clearInterval(this.timer);
        });

        Event.$on("init", () => {
            this.errors = 0;
            this.correct = 0;
            this.score = 0;
            this.finished = false;
            this.time = 60;
            this.uncorrected_errors = 0;
            clearInterval(this.timer)
        });

        Event.$on("start-timer", () => {
            this.timer  =  setInterval(() => {
                this.time -= 1;
            }, 1000)  
        });
    },
    methods: {
        
    }, 
    computed: {
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
