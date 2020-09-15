<template>
    <div>
        <form @submit="convert">
            <fieldset>
                <legend>Money to Money</legend>
                <div id="fields">
                    <input type="text" v-model="money" placeholder="Amount to Convert"><br>
                        <div id="selectors">
                            <select id="dropDown1" v-model="convertFrom">
                                <option value="Convert From" convertFrom>Convert From</option>
                                <option v-for="rate in rates" v-bind:key="rate.id">
                                    {{ rate.abb }}
                                </option> 
                            </select>
                            
                            <select id="dropDown2" v-model="convertTo">
                                <option value="Convert to" convertTo>Convert to</option>
                                <option v-for="rate in rates" v-bind:key="rate.id">
                                    {{ rate.abb }}
                                </option> 
                            </select>
                        </div>
                </div>
                <div id="buttons">
                    <input type ="submit" value="Convert" class="btn">
                    <input type ="reset" @click="clearOut" value="Reset" class="btn">
                </div>
            </fieldset>
            
        </form>
    </div>
    
    
</template>

<script>

export default {
    name: "Convert",
    props: ["rates"],
    data: ()=> {
        return {
            money : "",
            convertFrom: "Convert From",
            convertTo : "Convert to"
        }
    },
    methods: {
        convert(e) {
            e.preventDefault();
            if (!isNaN(this.money) && (this.money.length > 0) && (this.convertTo != "Convert to") && (this.convertFrom != "Convert From")) {
                console.log("Money: " + this.money);
                console.log("From: " + this.convertFrom);
                console.log("To: " + this.convertTo);
                this.$emit('new-convert', this.money, this.convertFrom, this.convertTo);
            }
        },
        clearOut() {
            this.money = "";
            this.convertTo = "Convert to";
            this.convertFrom = "Convert From";
            this.$emit('clear-out');
            // console.log("Reset stuff:");
            // console.log("CT: " + this.convertTo);
            // console.log("CF: " + this.convertFrom);
            // console.log("M: " + this.money);
        }
    } 
}
</script>

<style scoped>
    legend {
        font-style: italic;
        font-weight: bold;
        text-align: left;
    }
    form {
        width: 40%;
        margin: 2.5em auto;
    }

    #fields input, input {
        margin: .25em 0;
    }
    #buttons {
        margin: 0 auto;
        width: 25%;
    }
    #buttons input {
        margin-right: .4em;
    }
    #selectors {
        width: 90%;
        margin: .5em auto;
    }
    #dropDown1 {
        margin-right: 1em;
    }
</style>