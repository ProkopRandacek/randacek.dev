---
permalink: /s/vpl
title: Věcička pro Lucku
layout: default
---
# Věcička pro Lucku

******

<textarea id="input" cols="40" rows="5" style="color: black"></textarea>

<button onclick="format()" style="color: black">format</button>

<p id="output"></p>

<script>
function format() {
document.getElementById("input").value = document.getElementById("input").value.replaceAll("\n\n\n", "\n\n").replaceAll("\n\n", "\n");

var copyText = document.getElementById("input");

/* Select the text field */
copyText.select();
copyText.setSelectionRange(0, 99999); /* For mobile devices */

/* Copy the text inside the text field */
document.execCommand("copy");

/* Alert the copied text */
alert("Zkopírováno do schránky");
}
</script>
