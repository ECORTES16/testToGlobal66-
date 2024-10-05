<template>
    <v-container v-if="!loading" class="mt-4" style="   
  padding: 10px;
  display:flex;
      height: 92vh;
      overflow: auto;">
        <v-responsive class="align-centerfill-height mx-auto" max-width="600" >
            <v-text-field
            prepend-inner-icon="mdi-magnify"
            density="compact"
            label="Search"
            variant="solo"
            hide-details
            single-line
            v-model="namePokemon"
            @keyup="infoPokemon(null, true)"/>
            
            <v-list v-if="pokeList" lines="one" class="mt-4 " style="background-color: #F9F9F9;">
                <v-list-item
                    class="my-2  "
                    v-for="(pokemon,index) in pokemons"
                    :key="pokemon.name"
                    style="background-color: white;" 
                >
                <v-row >
                    <v-col @click="infoPokemon(pokemon)" class="h-4 py-6 d-flex flex-row me-auto cursor-pointer text-capitalize">
                        {{  pokemon.name }}
                    </v-col>
                    <v-col class="d-flex flex-row-reverse">
                        <v-btn :color="pokemon.favorite ? `#ECA539`: `#BFBFBF`" icon="mdi-star" variant="text" @click="updateFavorite(pokemon.name)"/>
                    </v-col>
                </v-row>
                </v-list-item>
            </v-list>
            <v-card v-else class="mx-auto mt-6" max-width="500" style="background-color: #F9F9F9; box-shadow: unset;">
                <v-card-item >
                    <v-card-title class="d-flex justify-center mt-6 text-h3">
                        Uh-oh!
                    </v-card-title>

                    <div class="d-flex justify-center mb-2 font-weight-regular" style="text-align: center;">
                        You look lost on your journey!
                    </div>
                    <div>
                        <v-btn
                        class="mx-auto d-flex justify-center mb-2 text-body-2"
                        color="#F22539"
                        rounded
                        to="/"
                        style="width: fit-content;"
                        >
                        Go back home
                        </v-btn>
                    </div>
                </v-card-item>
            </v-card>

    </v-responsive>
        
    </v-container>
    <v-container v-else class="fill-height">
        <v-responsive class="align-centerfill-height mx-auto content-load" max-width="600"  style="overflow: top;">
            <v-img class="mx-auto imgLoading" width="40" cover src="/src/assets/loading.png"/>
        </v-responsive>
    </v-container>
    <div v-if="pokeList">
        <v-row class="mx-0" justify="center" style="width: 100%;">
            <v-col xs="6" md="3" class="v-col v-col-xs-6">
                <v-btn block @click="getData()" :color="btnActiveAll ? `#F22539` : `#BFBFBF`" rounded to="/pokelist" >
                    <v-icon icon="mdi-format-list-bulleted" color="white" />
                    <div class="text-none text-subtitle-1" style="color: white;">
                        All
                    </div>
                </v-btn>                
            </v-col>
            <v-col xs="6" md="3" class="v-col v-col-xs-6">
                <v-btn block @click="switchList()" :color="btnActiveFavorite ? `#F22539` : `#BFBFBF`" rounded to="/pokelist" >
                    <v-icon icon="mdi-star" color="white"/>
                    <div class="text-none text-subtitle-1" style="color: white;">
                        Favorites
                    </div>
                </v-btn>                
            </v-col>
        </v-row>
    </div>
    <template>
        <div class="text-center pa-4">
            <v-dialog v-model="dialog" persistent width="auto" >
                <v-card class="mx-auto" max-width="400">
                    <v-btn
                  icon="mdi-close-outline"
                  color="transparent"
                  style="position: absolute; z-index: 1;right: 1px;"
                  @click=" dialog= false"
                ></v-btn>
                    <div style="position: relative; left: 0; top: 0;">
                        <img :src="pokemonSelected.sprites.other['official-artwork']['front_default']" class='eye'/>
                        <img src="/src/assets/fondo-pokemon.png" class="heaven"/>
                    </div>
                    <v-card-text class="py-0 px-6" style="position: relative;top:-34px;">
                        <v-list class="py-2"><span class="text-none text-subtitle-2">Name: </span>
                            <span class="font-weight-regular text-capitalize">{{pokemonSelected.name}}</span> 
                        </v-list>
                        <v-divider></v-divider>
                        <v-list class="py-2"><span class="text-none text-subtitle-2">Weight: </span>
                            <span class="font-weight-regular text-capitalize">{{pokemonSelected.weight}}</span>
                        </v-list>
                        <v-divider></v-divider>
                        <v-list class="py-2"><span class="text-none text-subtitle-2">Height: </span>
                            <span class="font-weight-regular text-capitalize">{{pokemonSelected.height}}</span>
                        </v-list>
                        <v-divider></v-divider>
                        <v-list class="py-2"><span class="text-none text-subtitle-2">Type: </span>
                            <span class="font-weight-regular text-capitalize">{{pokemonSelected.type}}</span>
                        </v-list> 
                        <v-divider></v-divider>
                    </v-card-text>
                    <v-row class="ma-0">
                        <v-col>                    
                            <v-btn block @click="copyClipboard()" color="#F22539" rounded to="/pokelist" >
                                <div class="text-none text-subtitle-1" style="color: white;">
                                    Share to my friends
                                </div>
                            </v-btn>            
                        </v-col>
                        <v-col class="d-flex flex-row-reverse">
                            <v-icon :color="pokemonSelected.favorite ? `#ECA539`: `#BFBFBF`" size="35" @click="updateFavorite(pokemonSelected.name)" icon="mdi-star" />
                        </v-col>              
                    </v-row>
                </v-card>
            </v-dialog>
        </div>
    </template>
</template>

<script setup lang="ts">
import { ref } from "vue";

    onMounted(() => {
        getData ()
    })
    const pokemons = ref();
    const loading = ref(true);
    const btnActiveAll = ref(false)
    const btnActiveFavorite = ref(false);
    const dialog = ref(false)
    const pokemonSelected = ref()
    const namePokemon = ref()
    const pokeList = ref(false)
    const timeout = ref()

    async function getData() {
        if(btnActiveAll.value != true){
            loading.value = true;
            const response = await fetch(
                `https://pokeapi.co/api/v2/pokemon?offset=0&limit=40`
            ).then((res) => res.json());
            pokemons.value =  response.results.map((el) => {
                el['favorite']= localStorage.getItem(el.name) != null ? true : false
                return el;
            });
                    
            setTimeout(() => {                
                loading.value = false;
            }, 1000);
            btnActiveAll.value = true
            btnActiveFavorite.value = false
            pokeList.value  = true   
        }
        console.info(pokeList.value)
    }

    async function switchList() {
        if(btnActiveFavorite.value != true){
            loading.value = true;
            pokemons.value = []
            for (var i = 0; i < localStorage.length; i++){
                pokemons.value[i] = JSON.parse(localStorage.getItem(localStorage.key(i)))
            }
            console.info()

            setTimeout(() => {                
                loading.value = false;
            }, 1000);
            btnActiveFavorite.value = true
            btnActiveAll.value = false

            console.info(localStorage)
        }
    }
   
    async function updateFavorite(name){
        let index = pokemons.value.map(function(e) { return e.name; }).indexOf(name);
        
        if(pokemonSelected.value != null){
            pokemonSelected.value.favorite = !pokemons.value[index]['favorite']
        }

        pokemons.value[index]['favorite'] = !pokemons.value[index]['favorite']
        !pokemons.value[index]['favorite']?localStorage.removeItem(pokemons.value[index]['name']):localStorage.setItem(pokemons.value[index]['name'], JSON.stringify(pokemons.value[index]))
    }

     function infoPokemon(pokemon = null, searching = false){
        let pokeName = searching ? namePokemon.value.toLowerCase() : pokemon.name
        console.info(pokeName)
        if(pokeName.length == 0){
            btnActiveAll.value = false
            return getData()
        }


         clearTimeout(timeout.value);
            timeout.value = setTimeout(async function () {
                const response = await fetch(
                    `https://pokeapi.co/api/v2/pokemon/`+pokeName
                )
                .then((res) => res.json())
                .catch(err =>{
                    pokeList.value  = false   
                    return false
                });
                if(!searching){
                    loading.value = true;

                    dialog.value = true
                    pokemonSelected.value = response
                    let typePokemon = []
                    for(const infoType of pokemonSelected.value.types){
                        typePokemon.push(' '+infoType.type.name.charAt(0).toUpperCase()+ infoType.type.name.slice(1))
                    }
                    pokemonSelected.value.type = typePokemon.toString() 
                    pokemonSelected.value.favorite = localStorage.getItem(pokemonSelected.value.name) != null ? true : false
                    loading.value = false;             
        
                }else{
                   if(response){
                       pokemons.value = [{'name':response.name, 'favorite': localStorage.getItem(response.name) != null ? true : false}]
                       btnActiveAll.value = false
                       btnActiveFavorite.value = false
                       namePokemon.value = ''
                       pokeList.value = true
                   }
                }

            }, 1000);



    }

    async function copyClipboard(){
        try {
            console.info(pokemonSelected.value.name)
            await navigator.clipboard.writeText(
                'Name: '+pokemonSelected.value.name.charAt(0).toUpperCase()+ pokemonSelected.value.name.slice(1)+
                ', Weight: '+pokemonSelected.value.weight+
                ', Height: '+pokemonSelected.value.height+
                ', Type: '+pokemonSelected.value.type
            );
        } catch (err) {
            console.error('Error al copiar: ', err);
        }
    }

</script>
<style>
.imgLoading{
    -webkit-animation:spin 2s linear infinite;
    -moz-animation:spin 2s linear infinite;
    animation:spin 2s linear infinite;
}
@-moz-keyframes spin { 
    100% { -moz-transform: rotate(360deg); } 
}
@-webkit-keyframes spin { 
    100% { -webkit-transform: rotate(360deg); } 
}
@keyframes spin { 
    100% { 
        -webkit-transform: rotate(360deg); 
        transform:rotate(360deg); 
    } 
}
.mdi-close-outline{
    color: white;
}
.v-responsive__content {
    overflow-x: auto;
    /* scrollbar-width: none;
    -ms-overflow-style: none; */
}

.content-load > .v-responsive__content{
    overflow-x: unset !important;
}

.eye{
    position: absolute;
    height: 125px;
    width: 125px;
    top: 21px;
    left: 100px;
    z-index: 1;
}

.heaven
{
    height: 200px;
    position: relative;
    top: -42px;
    width: 100%;
}
</style>