<template>
    <div>
        <form @submit="convertCurrency">
            <fieldset>
                <div id="fields">
                    <input type="text" v-model="money" name="money" placeholder="Amount to convert"><br>
                    <input type="text" v-model="convertFrom" name="convertFrom" placeholder="Convert from - country"><br>
                    <input type="text" v-model="convertTo" name="convertTo" placeholder="Covert to - country"><br>
                </div>
                <div id="buttons">
                    <input type ="submit" value="Convert" class="btn">
                    <input type ="reset" value="Reset" class="btn">
                </div>
            </fieldset>
            
        </form>
    </div>
</template>

<script>

export default {
    name: "Convert",
    data() {
        return {
            money : '',
            convertFrom : '',
            convertTo : '',
        }
    },
    methods: {
        convertCurrency(e) {
            e.preventDefault();
            const newConversion = {
                money : this.money,
                cFrom : this.convertFrom,
                cTo : this.convertTo
            }
            // Send to parent
            this.$emit('new-conversion', newConversion);

            // money : '';
            // convertFrom : '';
            // convertTo : '';
        }
    } 
}
</script>

<style scoped>
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
</style>