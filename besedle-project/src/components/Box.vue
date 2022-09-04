<template>
    <div class="box" :class="{'gray' :color == 'gray','green' :color == 'green','yellow' :color == 'yellow'}">
        <div class="crka">{{ letter }}</div>
    </div>
</template>


<script setup>
    const props = defineProps ({
        letter: {
            type: String,
            default: "",
        },
        color: {
            type: String,
            default: ""
        }
    });
</script>

<style scoped>
.box {
    height: 60px;
    width: 60px;
    border: 2px solid rgb(89, 89, 89);
    display: flex;
    justify-content: center;
    align-items: center;
}

.crka {
    font-size: 28px;
    text-transform: uppercase;
    font-weight: 700;
}
.gray {
    background-color: gray;
}
.green {
    background-color: green;
}
.yellow {
    background-color: rgb(223, 186, 0);
}
</style>