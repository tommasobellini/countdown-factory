<template>
    <div class="countDown" v-if="!state">
        <h1 class="counter">
            {{timer}}
        </h1>
        <div class="image" v-if="state">
            <img src="../assets/logo.png" alt="">
        </div>
    </div>
</template>


<script>
export default {
    name: "CountDownWidget",
    props:{
        timer: {
            type: String,
            required: true,
        },
        state: {
            type: Boolean,
            required: true,
        }
    },
    
}
</script>

<style scoped lang="scss">
    .countDown{
        .counter{
            font-weight: 800;
            font-size: 8rem;
            color: #262739;
        }
    }
    .image{
    border-radius: 3rem;
    height: 50vh;
}
</style>