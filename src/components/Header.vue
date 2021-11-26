<template>
    <v-tabs
        fixed-tabs
        background-color="orange"
        dark
        style="position: fixed;z-index: 11;">
        <v-tab> Home </v-tab>
        <v-tab> Weblog </v-tab>
        <v-tab><a href="#cartBox" class="text-decoration-none" style="color: rgba(255, 255, 255, 0.6)"> Shopping Cart </a> </v-tab>
        <v-tab> Contact US </v-tab>
        <v-tab>
            <v-badge color="black"
                     :content="carts.length > 0 ? carts.length : '0'">
                <v-icon dark>
                    mdi-cart
                </v-icon>
            </v-badge>
        </v-tab>
    </v-tabs>
</template>

<script>
export default {
    name: "Header",
    props: ["carts"],
    data: function () {
        return {

        }
    }
}
</script>

<style scoped>

</style>