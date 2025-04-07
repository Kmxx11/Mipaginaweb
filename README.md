# Ruta base del proyecto
base_path = "/mnt/data/sitio_profesional"
os.makedirs(base_path, exist_ok=True)

# HTML principal
index_html = """<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Explorador Espacial</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <header>
    <h1 class="glow">Explorador Espacial</h1>
    <div class="search-container">
      <input type="text" id="busqueda" placeholder="¿Qué querés descubrir en el universo?" onkeypress="buscar(event)">
    </div>
  </header>

  <div class="slideshow-container">
    <div class="slide fade"><div class="placeholder">Imagen 1</div></div>
    <div class="slide fade"><div class="placeholder">Imagen 2</div></div>
    <div class="slide fade"><div class="placeholder">Imagen 3</div></div>
    <div class="space-overlay"></div>
  </div>

  <div class="results" id="resultados">
    <p>Escribí algo arriba y presioná Enter.</p>
  </div>

  <footer>
    &copy; 2025 Explorador Espacial. Proyecto experimental.
  </footer>

  <script src="script.js"></script>
</body>
</html>
"""

# CSS
styles_css = """
body {
  margin: 0;
  font-family: 'Orbitron', sans-serif;
  background: radial-gradient(ellipse at bottom, #1b2735 0%, #090a0f 100%);
  color: white;
  overflow-x: hidden;
}
header {
  padding: 60px 20px;
  text-align: center;
}
header h1 {
  font-size: 3em;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 2px;
}
.search-container {
  margin-top: 30px;
}
input[type="text"] {
  padding: 15px 20px;
  width: 90%;
  max-width: 500px;
  font-size: 1em;
  border-radius: 30px;
  border: 2px solid #0ff;
  background-color: #000;
  color: white;
  outline: none;
}
.results {
  text-align: center;
  padding: 40px 20px;
  min-height: 150px;
}
.fade-in {
  animation: fadeIn 1s ease forwards;
  opacity: 0;
}
@keyframes fadeIn {
  to { opacity: 1; }
}
.slideshow-container {
  position: relative;
  max-width: 90%;
  margin: auto;
  overflow: hidden;
  border-radius: 15px;
  border: 2px solid #0ff;
  box-shadow: 0 0 20px #0ff;
  background: #111;
}
.slide {
  display: none;
  width: 100%;
  height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.placeholder {
  width: 100%;
  height: 100%;
  color: #0ff;
  font-size: 2em;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #222;
}
.space-overlay {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: linear-gradient(to bottom, rgba(0,0,0,0.3), rgba(0,0,0,0.7));
}
footer {
  text-align: center;
  padding: 30px;
  font-size: 0.9em;
  color: #aaa;
}
.glow {
  text-shadow: 0 0 10px #0ff, 0 0 20px #0ff;
}
"""

# JS
script_js = """
const datos = {
  marte: "Marte es el cuarto planeta del sistema solar, conocido como el planeta rojo.",
  luna: "La Luna es el satélite natural de la Tierra. Ha sido visitada por astronautas en el siglo XX.",
  jupiter: "Júpiter es el planeta más grande, con una gran tormenta conocida como la Gran Mancha Roja.",
  galaxia: "Las galaxias son conjuntos enormes de estrellas, planetas, polvo y materia oscura.",
  spacex: "SpaceX es una empresa que lidera el avance aeroespacial con cohetes reutilizables y misiones hacia Marte.",
};

function buscar(event) {
  if (event.key === "Enter") {
    const valor = document.getElementById("busqueda").value.toLowerCase();
    const resultado = document.getElementById("resultados");

    if (datos[valor]) {
      resultado.innerHTML = `<p class="fade-in">${datos[valor]}</p>`;
    } else {
      resultado.innerHTML = `<p class="fade-in">No encontramos información sobre "<strong>${valor}</strong>".</p>`;
    }
  }
}

// Slideshow automático
let slideIndex = 0;
function showSlides() {
  const slides = document.getElementsByClassName("slide");
  for (let i = 0; i < slides.length; i++) {
    slides[i].style.display = "none";
  }
  slideIndex++;
  if (slideIndex > slides.length) { slideIndex = 1; }
  slides[slideIndex - 1].style.display = "flex";
  setTimeout(showSlides, 5000); // 5 segundos
}

showSlides();
"""

# Guardar los archivos
with open(f"{base_path}/index.html", "w") as f:
    f.write(index_html)
with open(f"{base_path}/styles.css", "w") as f:
    f.write(styles_css)
with open(f"{base_path}/script.js", "w") as f:
    f.write(script_js)

# Crear el archivo ZIP
zip_path = "/mnt/data/pagina_super_pro.zip"
with zipfile.ZipFile(zip_path, "w") as zipf:
    for foldername, _, filenames in os.walk(base_path):
        for filename in filenames:
            file_path = os.path.join(foldername, filename)
            zipf.write(file_path, os.path.relpath(file_path, base_path))

zip_path
