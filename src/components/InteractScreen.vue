<template>
    <div class="screen">
        <div class="flex-container" :style="{ 'max-width':maxWidth + 'px'}">
            <card-flip 
                v-for="(card, index) in cardsContext" 
                :key="index" 
                :ref="`card-${index}`"
                :imgBackFaceUrl="`images/${card}.png`"
                :card="{ index: index, value: card }"
                :rules="rules"
                @onFliped="checkRule($event)"
            />
        </div>
    </div>
</template>

<script>
import CardFlip from "./Card.vue";

export default {
    props: {
        cardsContext: {
            type: Array,
            default: function() {
                return [];
            }
        }
    },
    components: {
        CardFlip,
    },
    data() {
        return {
            rules: [],
            count: 0,
            maxWidth: Math.sqrt(this.cardsContext.length) * 100 + 150
        }
    },
    methods: {
        checkRule(card) {
            if(this.rules.length == 2) return false;
            this.rules.push(card);
            let i = 0;
            for(i ; i<this.rules.length;i++) {
                this.$refs[`card-${this.rules[i].index}`][0].onDisabledFlip();
            }

            if(this.rules.length == 2 && this.rules[0].value == this.rules[1].value) {
                console.log('right . . .');
                this.$refs[`card-${this.rules[0].index}`][0].onDisabledFlip();
                this.$refs[`card-${this.rules[1].index}`][0].onDisabledFlip();

                this.rules = [];
                this.count++;
                if(this.count == this.cardsContext.length / 2) {
                    setTimeout(()=>{
                        this.$emit("onFinish");
                    }, 920);
                }
            }
            if(this.rules.length == 2 && this.rules[0].value != this.rules[1].value) {
                console.log('wrong . . .');
                setTimeout(() => {
                    this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                    this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();

                    this.rules = [];
                }, 800)
                this.$refs[`card-${this.rules[0].index}`][0].onEnabledFlip();
                this.$refs[`card-${this.rules[1].index}`][0].onEnabledFlip();
            } else return false;
        }
    },
}
</script>

<style scoped>
.screen {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    min-height: 100vh;

}
.flex-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    padding: 2%;
}
</style>