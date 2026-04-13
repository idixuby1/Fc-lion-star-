<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Real Lion FC</title>

<style>
body{
    margin:0;
    font-family: Arial, sans-serif;
    background:#0b6623;
    color:white;
    text-align:center;
}

header{
    background:#111;
    padding:20px;
    position:relative;
}

header img{
    width:100px;
    border-radius:50%;
}

#adminBtn{
    position:absolute;
    top:20px;
    right:20px;
    background:#444;
    color:white;
    padding:8px 12px;
    border:none;
    border-radius:5px;
    cursor:pointer;
}

#adminStatus{
    color:yellow;
    display:none;
}

#loginBox{
    display:none;
    background:#111;
    padding:15px;
    border-radius:10px;
    width:200px;
    margin:10px auto;
}

section{
    margin:20px auto;
    padding:20px;
    border-radius:12px;
    width:90%;
    max-width:900px;
    box-shadow:0 4px 10px rgba(0,0,0,0.4);
}

#playersSection{ background:#336699; }
#matchesSection{ background:#339966; }
#newsSection{ background:#993333; }
#gallerySection{ background:#999933; }
#adminPanel{ background:#eeeeee; color:black; }
#contactSection{ background:#333366; }

.pitch{
    position:relative;
    height:500px;
    background:green;
    border:4px solid white;
    border-radius:10px;
}

.player{
    position:absolute;
    width:50px;
    text-align:center;
    cursor:grab;
}

.player img{
    width:50px;
    height:50px;
    border-radius:50%;
    border:2px solid white;
    transition:0.2s;
}

.player img:hover{
    transform:scale(1.1);
}

.player span{
    display:block;
    font-size:11px;
    background:white;
    color:black;
    border-radius:10px;
}

button,input{
    padding:10px;
    margin:5px;
    border:none;
    border-radius:5px;
}

button{
    background:#111;
    color:white;
    cursor:pointer;
}

.adminOnly{
    display:none;
}

.deleteBtn{
    background:red;
    font-size:12px;
}

footer{
    background:#111;
    padding:15px;
    color:#aaa;
}
</style>
</head>

<body>

<header>
<img src="images - 2026-03-03T054620.224.jpeg">
<h1>⚽ Real Lion FC</h1>
<p id="adminStatus">Admin Mode</p>
<button id="adminBtn" onclick="toggleLogin()">Control</button>
</header>

<div id="loginBox">
<input type="text" id="accessCode" placeholder="Access Code"><br>
<button onclick="login()">Enter</button>
</div>

<section>
<h2>About Team</h2>
<p>Real Lion FC is a passionate football club based in Kano, Nigeria.</p>
</section>

<section id="adminPanel" style="display:none;">
<h2>Club Control</h2>
<button onclick="logout()">Exit</button><br>

<input type="text" id="playerName" placeholder="Player name">
<button class="adminOnly" onclick="addPlayerInfo()">Add Player</button>

<input type="text" id="matchText" placeholder="Match">
<button class="adminOnly" onclick="addMatch()">Add Match</button>

<input type="text" id="newsText" placeholder="News">
<button class="adminOnly" onclick="addNews()">Add News</button>

<input type="file" id="galleryImage">
<button class="adminOnly" onclick="addGallery()">Add Image</button>
</section>

<section id="playersSection"><h2>Players</h2><div id="playersList"></div></section>
<section id="matchesSection"><h2>Matches</h2><div id="matchesList"></div></section>
<section id="newsSection"><h2>News</h2><div id="newsList"></div></section>
<section id="gallerySection"><h2>Gallery</h2><div id="galleryList"></div></section>

<footer>© 2026 Real Lion FC</footer>

<script>
const ADMIN_CODE = "LionFC@2026";

function toggleLogin(){
    let box = document.getElementById("loginBox");
    box.style.display = box.style.display === "block" ? "none" : "block";
}

function login(){
    let code = document.getElementById("accessCode").value.trim();

    if(code === ADMIN_CODE){
        sessionStorage.setItem("admin","true");
        showAdminPanel();
        document.getElementById("loginBox").style.display = "none";
    } else {
        alert("Wrong code!");
    }
}

function showAdminPanel(){
    document.getElementById("adminPanel").style.display="block";
    document.getElementById("adminStatus").style.display="block";
    document.querySelectorAll(".adminOnly").forEach(el=> el.style.display="inline-block");
}

if(sessionStorage.getItem("admin")==="true"){
    showAdminPanel();
}

function logout(){
    sessionStorage.removeItem("admin");
    location.reload();
}

setTimeout(()=>{
    sessionStorage.removeItem("admin");
},600000);

function safe(text){
    return text.replace(/[&<>"']/g, c => ({
        '&':'&amp;',
        '<':'&lt;',
        '>':'&gt;',
        '"':'&quot;',
        "'":'&#39;'
    }[c]));
}

function addPlayerInfo(){
    let players = JSON.parse(localStorage.getItem("players"))||[];
    let name = document.getElementById("playerName").value;
    if(name){
        players.push(name);
        localStorage.setItem("players", JSON.stringify(players));
        loadPlayers();
    }
}

function loadPlayers(){
    let list = document.getElementById("playersList");
    let players = JSON.parse(localStorage.getItem("players"))||[];
    list.innerHTML="";
    players.forEach((p,i)=>{
        list.innerHTML+=`<p>${safe(p)} <button class="deleteBtn adminOnly" onclick="deleteItem('players',${i})">X</button></p>`;
    });
}

function addMatch(){
    let matches = JSON.parse(localStorage.getItem("matches"))||[];
    let text = document.getElementById("matchText").value;
    if(text){
        matches.push(text);
        localStorage.setItem("matches", JSON.stringify(matches));
        loadMatches();
    }
}

function loadMatches(){
    let list = document.getElementById("matchesList");
    let matches = JSON.parse(localStorage.getItem("matches"))||[];
    list.innerHTML="";
    matches.forEach((m,i)=>{
        list.innerHTML+=`<p>${safe(m)} <button class="deleteBtn adminOnly" onclick="deleteItem('matches',${i})">X</button></p>`;
    });
}

function addNews(){
    let news = JSON.parse(localStorage.getItem("news"))||[];
    let text = document.getElementById("newsText").value;
    if(text){
        news.push(text);
        localStorage.setItem("news", JSON.stringify(news));
        loadNews();
    }
}

function loadNews(){
    let list = document.getElementById("newsList");
    let news = JSON.parse(localStorage.getItem("news"))||[];
    list.innerHTML="";
    news.forEach((n,i)=>{
        list.innerHTML+=`<p>${safe(n)} <button class="deleteBtn adminOnly" onclick="deleteItem('news',${i})">X</button></p>`;
    });
}

function addGallery(){
    let file = document.getElementById("galleryImage").files[0];
    if(!file) return;

    if(file.size > 2*1024*1024){
        alert("Image too large");
        return;
    }

    let reader = new FileReader();
    reader.onload = function(e){
        let gallery = JSON.parse(localStorage.getItem("gallery"))||[];
        gallery.push(e.target.result);
        localStorage.setItem("gallery", JSON.stringify(gallery));
        loadGallery();
    }
    reader.readAsDataURL(file);
}

function loadGallery(){
    let list = document.getElementById("galleryList");
    let gallery = JSON.parse(localStorage.getItem("gallery"))||[];
    list.innerHTML="";
    gallery.forEach((img,i)=>{
        list.innerHTML+=`<div><img src="${img}" width="100"><br><button class="deleteBtn adminOnly" onclick="deleteItem('gallery',${i})">X</button></div>`;
    });
}

function deleteItem(type,index){
    let data = JSON.parse(localStorage.getItem(type))||[];
    data.splice(index,1);
    localStorage.setItem(type, JSON.stringify(data));
    loadAll();
}

function loadAll(){
    loadPlayers();
    loadMatches();
    loadNews();
    loadGallery();
}

loadAll();
</script>

</body>
</html>
