<!DOCTYPE html>
<html lang="ka">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Voiceline</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial}
body{background:#050000;color:white}

/* NAV */
nav{padding:15px 30px;background:#000;border-bottom:1px solid #300}
.logo{color:red;font-size:20px;font-weight:bold}

/* SEARCH */
.search{display:flex;justify-content:center;margin:20px}
.search input{width:60%;padding:12px;border-radius:10px;border:none;background:#111;color:white}

/* GRID */
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:20px;padding:20px}

.card{background:#111;border-radius:12px;overflow:hidden;position:relative;cursor:pointer;transition:.3s}
.card:hover{transform:scale(1.05);box-shadow:0 0 20px rgba(255,0,0,0.4)}

.card img{width:100%;height:250px;object-fit:cover}
.card h3{padding:10px}

/* BUTTON */
.btn{margin:10px;background:red;padding:8px;border-radius:6px;text-align:center}

/* FAVORITE */
.fav{position:absolute;top:10px;right:10px;cursor:pointer}

/* MODAL */
.modal{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.9);display:none;align-items:center;justify-content:center}
.content{width:80%}
.close{position:absolute;top:20px;right:30px;font-size:30px;cursor:pointer}

.ep{background:#111;margin:5px;padding:8px;border-radius:6px;cursor:pointer}
.ep:hover{background:red}

iframe{width:100%;height:400px;border:none}
</style>
</head>

<body>

<nav><div class="logo">VOICELINE</div></nav>

<div class="search">
<input id="search" placeholder="Search anime...">
</div>

<div class="grid" id="grid"></div>

<!-- MODAL -->
<div class="modal" id="modal">
<span class="close" onclick="closeModal()">✖</span>
<div class="content">
<h2 id="title"></h2>
<p id="desc"></p>
<iframe id="video"></iframe>
<div id="episodes"></div>
</div>
</div>

<script>

/* 🎬 DATA */
const data = {
  Naruto:{
    desc:"Ninja story",
    img:"https://cdn.myanimelist.net/images/anime/1223/96541.jpg",
    episodes:[
      "https://www.youtube.com/embed/JwZ3N7e0M5I",
      "https://www.youtube.com/embed/6h0J0Wf08rE"
    ]
  },
  "Attack on Titan":{
    desc:"Titans war",
    img:"https://cdn.myanimelist.net/images/anime/10/47347.jpg",
    episodes:[
      "https://www.youtube.com/embed/MGRm4IzK1SQ"
    ]
  },
  "One Piece":{
    desc:"Pirate adventure",
    img:"https://cdn.myanimelist.net/images/anime/5/73199.jpg",
    episodes:[
      "https://www.youtube.com/embed/S8_YwFLCh4U"
    ]
  }
};

/* 📂 CREATE CARDS */
function load(){
  let grid=document.getElementById("grid");
  grid.innerHTML="";

  for(let name in data){
    let d=data[name];

    let div=document.createElement("div");
    div.className="card";

    div.innerHTML=`
      <div class="fav">♡</div>
      <img src="${d.img}">
      <h3>${name}</h3>
      <div class="btn">▶ Watch</div>
    `;

    div.onclick=()=>openAnime(name);

    // ❤️ favorites
    let favBtn=div.querySelector('.fav');
    let favs=JSON.parse(localStorage.getItem("favs")||"[]");
    if(favs.includes(name)) favBtn.innerText="❤️";

    favBtn.onclick=(e)=>{
      e.stopPropagation();
      let favs=JSON.parse(localStorage.getItem("favs")||"[]");

      if(favs.includes(name)){
        favs=favs.filter(f=>f!==name);
        favBtn.innerText="♡";
      }else{
        favs.push(name);
        favBtn.innerText="❤️";
      }

      localStorage.setItem("favs",JSON.stringify(favs));
    };

    grid.appendChild(div);
  }
}

/* 🔍 SEARCH */
document.getElementById("search").onkeyup=function(){
  let val=this.value.toLowerCase();
  document.querySelectorAll(".card").forEach(c=>{
    c.style.display=c.innerText.toLowerCase().includes(val)?"block":"none";
  });
};

/* 🎬 OPEN ANIME */
function openAnime(name){
  let d=data[name];

  document.getElementById("modal").style.display="flex";
  document.getElementById("title").innerText=name;
  document.getElementById("desc").innerText=d.desc;

  let epBox=document.getElementById("episodes");
  epBox.innerHTML="";

  d.episodes.forEach((ep,i)=>{
    let div=document.createElement("div");
    div.className="ep";
    div.innerText="Episode "+(i+1);
    div.onclick=()=>document.getElementById("video").src=ep;
    epBox.appendChild(div);
  });

  document.getElementById("video").src=d.episodes[0];
}

/* ❌ CLOSE */
function closeModal(){
  document.getElementById("modal").style.display="none";
  document.getElementById("video").src="";
}

load();

</script>

</body>
</html>
