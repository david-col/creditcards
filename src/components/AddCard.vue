<template>
    <div class="addCard">
        <div class="containerAddCard">
            <h2>Crear tarjeta de crédito</h2>
            <form v-on:submit.prevent="creditCardInput" method="POST">
                <input type="text" v-model="credit_card.name" placeholder="nombre de tarjeta"> 
                <br>
                <input type="number" v-model="credit_card.number" placeholder="Número de tarjeta">
                <br>
                <input type="text" v-model="credit_card.franchise" placeholder="franquicia">
                <br>
                <input type="text" v-model="credit_card.bankname" placeholder="nombre del banco">
                <br>
                <input type="text" v-model="credit_card.id_user" placeholder="nombre del banco">
                <br>
                <button type="submit">Agregar</button>
            </form>
        </div>
    </div>
</template>

<script>
    import gql from 'graphql-tag';
    export default {
        name: "AddCard",
        data: function(){
            return {
                credit_card: {
                    name:"",
                    number: 0,
                    franchise: "",
                    bankname: "",
                    id_user: 0
                }
            }
        },
        methods: {
            validateJustCharacters: function(stringToValidate){
                const regex = /^[a-zA-Z]+$/;
                return regex.test(stringToValidate);
            },
            processSignUp: async function(){
                await this.$apollo.mutate(
                    {
                        mutation: gql`
                            mutation createCreditCard(creditCardInput: CreditCardInput): CreditCard
                            }
                        `,
                        variables:{
                            userInput: this.credit_card,
                        }
                    }
                )
                .then((result) => {
                    let dataAddCard = {
                        name       : this.user.username,
                        number     : this.credit_card.number,
                        franchise  : this.credit_card.franchise,
                        bankname   : this.credit_card.bankname ,
                        id_user    : this.credit_card.id_user 
                    };
                    this.$emit("completedAddCard", dataAddCard);
                })
                .catch((error) => {
                    console.log(error);
                    alert("ERROR. Fallo al añadir. Intente de nuevo.")
                })
            }
        }
    }
</script>


<style>
.addCard {
  margin: 0;
  padding: 0%;
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.addCars h2 {
  font-size: 50px;
  color: #163f6a;
}
.containerAddCard button{
        width: 55%;
        height: 35px;
        color: white;
        background: purple;
        border: 1px solid #E5E7E9;
        border-radius: 5px;
        padding: 10px 25px;
        margin: 5px 0;
        float: right;
    }
    .containerAddCard button:hover{
        color: #E5E7E9;
        background: rgb(20, 180, 220);
        border: 1px solid #283747;
    }
.addCard span {
  color: #163f6a;
  font-weight: bold;
}
</style>