<template>
    <div>
        <form class="credit-card" action="">
            <div class="credit-card__block">
                <label ref="cardValue" for="1">Card number</label>
                <input :key="updateKey" v-model="cardNumber" id="1" type="number" />
            </div>
            <div class="credit-card__bottom-body">
                <div class="credit-card__block">
                    <label for="2">Expiry date</label>
                    <input
                        ref="cardDate"
                        :key="updateKey"
                        v-model="expiryDate"
                        placeholder="MM / YY"
                        id="2"
                        type="text"
                    />
                </div>
                <div class="credit-card__block">
                    <label for="3">Secure code</label>
                    <input maxlength="4" v-model="secureCode" id="3" type="number" />
                </div>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    name: 'CreditCardDetails',
    data() {
        return {
            updateKey: 0,
            cardNumberValue: null,
            expiryDateValue: null,
            secureCode: null,
        }
    },
    computed: {
        cardNumber: {
            get() {
                return this.cardNumberValue
            },
            set(newVal) {
                // this.cardNumberValue = newVal.replace(/\D/g, '')
                newVal = newVal.slice(0, 19)
                newVal = newVal.replace(/\D/g, '')
                this.updateAndFocus('cardValue')
                this.cardNumberValue = newVal.replace(/(\d{4}(?=.+))/g, '$1 ')
            },
        },
        expiryDate: {
            get() {
                return this.expiryDateValue
            },
            set(newVal) {
                this.updateAndFocus('cardDate')
                newVal = newVal.replace(/\D/g, '').slice(0, 4)
                if (!/\//.test(newVal)) newVal = newVal.replace(/\d{2}/, '$&/')
                this.expiryDateValue = newVal
            },
        },
    },
    methods: {
        updateAndFocus(refName) {
            this.$nextTick(() => {
                this.updateKey++
                this.$nextTick(() => {
                    this.$refs[refName].focus()
                })
            })
        },
    },
}
</script>

<style lang="scss" scoped>
.credit-card {
    width: 400px;
    padding: 5px;
    background-color: #ded9d9;
    display: grid;
    gap: 10px;
    // .credit-card__block
    &__block {
        display: grid;
        grid-template-columns: auto;
        gap: 10px;
        label {
            font-size: 18px;
            text-transform: uppercase;
        }
        input {
            border: none;
            padding: 15px 10px;
            //display: inline-block;
            &:focus {
                outline: none;
                background-color: #edecce;
            }
        }
    }
    // .credit-card__bottom-body
    &__bottom-body {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 10px;
    }
}
</style>
