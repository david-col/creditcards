<template>
    <div class="container-table-cards">
            <table>
                <tr>
                    <th>Id tarjeta</th>
                    <th>Nombre de tarjeta</th>
                    <th>Número de tarjeta</th>
                    <th>Franquicia</th>
                    <th>Nombre de banco</th>
                    <th>id de usuario</th>
                    <th>Eliminar</th>
                </tr>
                
                <tr>
                    <td>{{  }}</td>
                    <td>{{  }}</td>
                    <td>{{  }}</td>
                    <td>{{  }}</td>
                    <td>{{  }}</td>
                    <td>{{  }}</td>
                    <td><i v-on:click="deleteCard()">Eliminar</i></td>
                </tr>
   
            </table>
    </div>
</template>

<script>
    import gql        from 'graphql-tag';
    import jwt_decode from 'jwt-decode';
    export default{
        name: "CreditCards",
        data: function(){
            return {
                userId: jwt_decode( localStorage.getItem("tokenRefresh") ).user_id,
                name: localStorage.getItem("name") || "none",
                number: localStorage.getItem("number") || "none",
                franchise: localStorage.getItem("franchise") || "none",
                bankname: localStorage.getItem("bankname") || "none",
                id_user: localStorage.getItem("id_user") || "none"
                }
                
            },
            methods: {
            deleteCreditCard: async function(id_){
                console.log(id_);
                await this.$apollo.mutate(
                    {
                        mutation: gql`
                            mutation deleteCreditCard( creditCardId: Int!): String
                        `,
                        variables: {
                            userId: id_
                        }
                    }
                )
                .then((result) => {
                    console.log(result);
                })
                .catch((error) => {
                    console.log(error);
                })
                this.$apollo.queries.creditCardByUserId.refetch();
            },
            reloadFilter: function(){
                this.$apollo.queries.creditCardsByUserId.refetch();
            }
        },
        apollo: {
            creditCardsByUserId: {
                query: gql`
                    id 
                    card_name
                    card_number
                    card_franchise
                    bank_name
                    id_user
                `,
                variables() {
                    return {
                        username: this.creditCard,
                    };
                }
            }
        },
        created: function(){
            this.$apollo.queries.creditCardsByUserId.refetch();
            
        }
}
</script>


<style>
table, th, td {
  	border:1px solid black; 
}
.container-table-cards {
  width: 30%;
  height: 50%;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.container-table-cards button{
   color: #263747;
   background: #283747;
   border: 1px solid #e5e7e9;

   border-radius: 35px;
   padding: 10px 20px;
   font-family: monospace;
}
.container-table-cards button:hover{
  color: #e5e7e9;
  background: crimson;
  border: 1px solid #283747;
}

.cards button {
  width: 100%;
  height: 40px;
  color: #e5e7e9;
  background: #173f6a;
  border: 1px solid #e5e7e9;
  border-radius: 5px;
  padding: 10px 25px;
  margin: 5px 0 25px 0;
  align-content: center;
}
.cards button:hover {
  color: #e5e7e9;
  background: crimson;
  border: 1px solid #283747;
}
</style>