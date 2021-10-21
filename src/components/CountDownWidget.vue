<template>
    <div class="countDown">
        <h1 class="counter">
            {{timer}}
        </h1>
    </div>
</template>
<script>
export default {
    name: "CountDownWidget",
    props:{
        timer: {
            type: String,
            required: true,
        }
    }
}
</script>

<style scoped lang="scss">
    .countDown{
        .counter{
            font-weight: 800;
            font-size: 3rem;
            color: #262739;
        }
    }
</style>