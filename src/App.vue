<template>
    <img alt="Vue logo" src="./assets/logo.png" />
    <TextEditor 
        :placeholder="''"
        :text="''"
        :active="true"
        :autoresize="true"
    />
</template>

<script>
import TextEditor from "./components/TextEditor.vue"

export default {
    name: "App",
    components: {
        TextEditor,
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
