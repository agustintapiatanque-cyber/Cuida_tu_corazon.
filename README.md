# Cuida_tu_corazon.
Cuida un corazón,sube su ánimo y Hacelo divertir.
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Corazón Interior</title>

<style>
body {
  margin: 0;
  display: flex;
  height: 100vh;
  font-family: Arial, sans-serif;
  background: #eef2ff;
}

/* PANEL */
#panel {
  width: 220px;
  background: white;
  padding: 10px;
  box-shadow: 2px 0 10px rgba(0,0,0,0.1);
}

button {
  width: 100%;
  margin: 4px 0;
  padding: 8px;
  cursor: pointer;
}

/* JUEGO */
#juego {
  flex: 1;
  text-align: center;
  padding-top: 50px;
}

#corazon {
  font-size: 100px;
  margin: 20px;
}

/* ESTADOS */
.feliz { animation: latido 1.5s infinite; }
.triste { opacity: 0.5; }
.enojado { animation: temblar 0.2s infinite; }

@keyframes latido {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
}

@keyframes temblar {
  0% { transform: translateX(-2px); }
  50% { transform: translateX(2px); }
}

/* ALEGRÍA */
#barra {
  width: 300px;
  height: 20px;
  border: 2px solid #444;
  margin: auto;
}

#alegria {
  height: 100%;
  width: 50%;
  background: pink;
}

/* MINIJUEGOS */
.minijuego {
  display: none;
  margin-top: 20px;
}
</style>
</head>

<body>

<div id="panel">
<h3>🏠 Habitaciones</h3>
<button onclick="habitacion('Cocina')">🍳 Cocina</button>
<button onclick="habitacion('Dormitorio')">🛏 Dormitorio</button>
<button onclick="habitacion('Baño')">🛁 Baño</button>
<button onclick="habitacion('Sala')">🛋 Sala</button>

<h3>💗 Acciones</h3>
<button onclick="accion(10,'Alimentarlo también es cuidarlo')">🍎 Comer</button>
<button onclick="accion(8,'El consuelo también sana')">🧸 Peluche</button>
<button onclick="accion(7,'El descanso importa')">🛌 Manta</button>
<button onclick="accion(6,'Limpiarte es un acto de amor')">🧼 Bañar</button>

<h3>🎮 Minijuegos</h3>
<button onclick="mostrar('puzzle')">🧩 Rompecabezas</button>
<button onclick="mostrar('memoria')">🧠 Memoria</button>
<button onclick="mostrar('respirar')">🌬 Respirar</button>
<button onclick="mostrar('crecer')">🌱 Crecer</button>
</div>

<div id="juego">
<div id="corazon">❤️</div>
<p id="mensaje">Cuida tu corazón con paciencia.</p>

<div id="barra"><div id="alegria"></div></div>

<!-- ROMPECABEZAS -->
<div id="puzzle" class="minijuego">
<h3>🧩 Siempre puedes reconstruirte</h3>
<div id="piezas"></div>
<button onclick="mezclar()">Mezclar</button>
</div>

<!-- MEMORIA -->
<div id="memoria" class="minijuego">
<h3>🧠 Recuerda lo bueno</h3>
<div id="cartas"></div>
</div>

<!-- RESPIRAR -->
<div id="respirar" class="minijuego">
<h3>🌬 Respira con calma</h3>
<button onclick="respirar()">Inhalar / Exhalar</button>
</div>

<!-- CRECER -->
<div id="crecer" class="minijuego">
<h3>🌱 Lo pequeño también florece</h3>
<button onclick="regar()">Regar 🌧</button>
<p id="planta">🌱</p>
</div>

</div>

<script>
let alegria = 50;
const corazon = document.getElementById("corazon");
const barra = document.getElementById("alegria");
const mensaje = document.getElementById("mensaje");

/* SISTEMA EMOCIONAL */
function actualizar() {
  alegria = Math.max(0, Math.min(100, alegria));
  barra.style.width = alegria + "%";
  corazon.className = "";

  if (alegria >= 70) {
    corazon.classList.add("feliz");
    mensaje.textContent = "💖 Tu corazón está en calma.";
  } else if (alegria <= 30) {
    corazon.classList.add("enojado");
    mensaje.textContent = "😠 El corazón necesita atención.";
  } else {
    corazon.classList.add("triste");
    mensaje.textContent = "😢 Un poco de cuidado ayudaría.";
  }
}

function accion(valor,texto){
  alegria += valor;
  mensaje.textContent = texto;
  actualizar();
}

function habitacion(nombre){
  mensaje.textContent = "Estás en la " + nombre + ".";
}

/* MOSTRAR MINIJUEGOS */
function mostrar(id){
  document.querySelectorAll(".minijuego").forEach(m=>m.style.display="none");
  document.getElementById(id).style.display="block";
}

/* ROMPECABEZAS */
let puzzle=[1,2,3,4];

function mezclar(){
  puzzle.sort(()=>Math.random()-0.5);
  renderPuzzle();
}

function renderPuzzle(){
  const cont=document.getElementById("piezas");
  cont.innerHTML="";
  puzzle.forEach((n,i)=>{
    let b=document.createElement("button");
    b.textContent=n;
    b.onclick=()=>{
      if(i>0){
        [puzzle[i],puzzle[i-1]]=[puzzle[i-1],puzzle[i]];
        renderPuzzle();
      }
    };
    cont.appendChild(b);
  });
  if(JSON.stringify(puzzle)==="[1,2,3,4]"){
    alegria+=15;
    mensaje.textContent="✨ Incluso roto, puedes sanar.";
    actualizar();
  }
}
mezclar();

/* MEMORIA */
let palabras=["Amor","Calma","Fuerza","Luz"];
let cartas=[...palabras,...palabras].sort(()=>Math.random()-0.5);
let sel=[];

function renderMemoria(){
  const cont=document.getElementById("cartas");
  cont.innerHTML="";
  cartas.forEach(p=>{
    let b=document.createElement("button");
    b.textContent="❓";
    b.onclick=()=>{
      b.textContent=p;
      sel.push({p,b});
      if(sel.length===2){
        if(sel[0].p===sel[1].p){
          alegria+=5;
          mensaje.textContent="💗 Las palabras sanan.";
        } else {
          setTimeout(()=>{
            sel[0].b.textContent="❓";
            sel[1].b.textContent="❓";
          },500);
        }
        sel=[];
        actualizar();
      }
    };
    cont.appendChild(b);
  });
}
renderMemoria();

/* RESPIRAR */
let respiraciones=0;
function respirar(){
  respiraciones++;
  mensaje.textContent="Respirando...";
  if(respiraciones>=5){
    alegria+=12;
    mensaje.textContent="🌸 Detenerse también es avanzar.";
    respiraciones=0;
    actualizar();
  }
}

/* CRECER */
let crecimiento=0;
function regar(){
  crecimiento++;
  document.getElementById("planta").textContent=
    crecimiento<3?"🌱":crecimiento<5?"🌿":"🌸";
  if(crecimiento>=5){
    alegria+=15;
    mensaje.textContent="💚 El cuidado constante florece.";
    crecimiento=0;
    actualizar();
  }
}
</script>

</body>
</html>
