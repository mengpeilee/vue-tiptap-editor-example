<template>
  <div id="app">
    <TextEditor />
  </div>
</template>

<script>
import TextEditor from "./components/TextEditor";

export default {
  name: "App",
  components: { TextEditor }
};
</script>

<style>
.body-blue {
  color: blue;
}
.body-red {
  color: red;
}
.body-black {
  color: black;
}
</style>
