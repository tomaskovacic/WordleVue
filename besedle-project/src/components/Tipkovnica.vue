<script setup>
    import Keyboard from "simple-keyboard";
    import "simple-keyboard/build/css/index.css";
    import { ref, onMounted, watch } from "vue";

    const emit = defineEmits(["onKeyPress"]);

    const props = defineProps ({
        guessedLetters: Object,
    });

    const keyboard = ref(null);

    const onKeyPress = (button) => {
        emit("onKeyPress", button);
    };

    onMounted(() => {
        keyboard.value = new Keyboard("simple-keyboard", {
            layout: {
                default: [
                    "q w e r t z u i o p š ž",
                    "a s d f g h j k l č",
                    "{enter} y x c v b n m {bksp}",
                ],
            },
            onKeyPress: onKeyPress,
        });
    });

    watch(
        () => props.guessedLetters,
        (guessedLetters, prevGuessedLetters) => {
            keyboard.value.addButtonTheme(
                guessedLetters.miss.join(" "),
                "miss"
            );
            keyboard.value.addButtonTheme(
                guessedLetters.found.join(" "),
                "found"
            );
            keyboard.value.addButtonTheme(
                guessedLetters.hint.join(" "),
                "hint"
            );
        },
        {deep: true}
    );
</script>

<template>
    <div class="simple-keyboard"></div>
</template>

<style>
.simple-keyboard {
    color: black !important;
    max-width: 580px;
    margin: 100px auto;
}

div.miss {
    background-color: gray !important;
    color: white !important;
}
div.found {
    background-color: green !important;
    color: white !important;
}
div.hint:not(.found) {
    background-color: rgb(223, 186, 0) !important;
    color: white !important;
}
</style>