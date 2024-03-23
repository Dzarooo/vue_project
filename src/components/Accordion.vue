<script setup>
    function toggleWrapper(wrapperId) {
        let wrapper = document.getElementById("wrapper" + wrapperId);
        //let arrow = document.getElementById("arrow" + wrapperId);

        if(wrapper.style.gridTemplateRows == "0fr") {
            wrapper.style.gridTemplateRows = "1fr";
        }
        else {
            wrapper.style.gridTemplateRows = "0fr";
        }

        // if (wrapper.classList.contains("[grid-template-rows:1fr]")) {
        //     arrow.style.transform = "";
        // } else {
        //     arrow.style.transform = "rotate(180deg)";
        // }
    }
</script>

<template>
    <div style="background-color:rgb(39, 39, 39); border-radius:20px; padding:15px; margin-bottom:15px">

        <div v-on:click="toggleWrapper(1)"><h1>fajny tytul</h1></div>

        <div id="wrapper1" style="display: grid; grid-template-rows: 0fr; overflow:hidden; transition-property:all; transition-duration:300ms;">
            <div style="overflow:hidden;">
                <p>jasdadasd</p><p>jasdadasd</p><p>jasdadasd</p>
            </div>
        </div>

    </div>
</template>