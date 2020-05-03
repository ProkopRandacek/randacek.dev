<script>
function update()
{
var loader = document.getElementById("type").value
var trains = document.getElementById("trains").value
var stack  = document.getElementById("stack").value
var sides = document.getElementById("sides").value
if (loader == "loader")
{
var num = trains * 40 * stack
document.getElementById("sides").style.display = "none"
document.getElementById("sidesLabel").style.display = "none"
document.getElementById("result").innerHTML = 'Set the condition to "Enable/disable" and "[item] > ' + num + '"'
}
else
{
if (sides == "yes") { s = 2 }
else { s = 1 }
var num = (trains * 6 * 48 * stack * s) - (trains * 40 * stack)
document.getElementById("sides").style.display = ""
document.getElementById("sidesLabel").style.display = ""
document.getElementById("result").innerHTML = 'Set the condition to "Enable/disable" and "[item] < ' + num + '"'
}
}
</script>
# Smart Train Stop Enabler Calcualtor
Description wip
******
<form onchange="update()">
	<label for="type">Type of station:</label>
	<select id="type" name="type">
		<option value="loader">Loader</option>
		<option value="unloader">Unloader</option>
	</select><br>
	<label for="trains">Number of cargo wagons:</label>
	<input type="number" id="trains" name="trains"><br>
	<label for="stack">Stack size of the item:</label>
	<input type="number" id="stack" name="stack"><br>
	<label for="sides" id="sidesLabel">Do you unload from both sides of the train?:</label>
	<select id="sides" name="side">
		<option value="yes">Yes</option>
		<option value="no">No</option>
	</select><br>
</form><br>
<p id="result"></p>
<br>
[Download python script](trainstopenabler.zip)
<script>
document.getElementById("type").selectedIndex = 1
document.getElementById("trains").value = 8
document.getElementById("stack").value = 50
document.getElementById("type").selectedIndex = 1
update()
</script>
