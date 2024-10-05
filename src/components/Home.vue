<template>
    <v-container class="fill-height" >
        <div class="align-centerfill-height mx-auto" max-width="600" >
            <v-img class="mx-auto" width="200" cover src="/src/assets/logo_pikachu.png"/>
            <v-card class="mx-auto" max-width="500" style="background-color: #F9F9F9; box-shadow: unset;">
                <v-card-item >
                    <v-card-title class="d-flex justify-center">
                        Welcome to Pokédex
                    </v-card-title>
                    <div class="py-8" style="text-align: center;">
                        The digital encyclopedia created by Professor Oak is an invaluable tool to Trainers in the Pokémon world.
                    </div>
                    <div>
                        <v-btn
                        class="mx-auto d-flex justify-center mb-2 text-body-2"
                        color="#F22539"
                        rounded
                        to="/pokelist"
                        style="width: fit-content;"
                        >
                        Get started
                        </v-btn>
                    </div>
                </v-card-item>
            </v-card>
        </div>
    </v-container>
</template>
  
<script setup lang="ts">
//
</script>
