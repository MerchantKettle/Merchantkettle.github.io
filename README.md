# Merchantkettle.github.io
Practice 
<h1>Isolation</h1>
<img src="Isolation.jpeg">

<style>
body{
  margin:0;
  height:100vh;
  background: linear-gradient(to bottom, purple, yellow);
}
</style>

<html>

<body style="text-align:center;
margin-top:100px;
">

<h1>Music Player</h1>

<audio id="music">
  <source src="music.mp3" type="audio/mp3">
</audio>

<button onclick="playMusic()">
Play Music
</button>

<script>
function playMusic(){
  document.getElementById("music").play();
}
</script>

</body>
</html>
