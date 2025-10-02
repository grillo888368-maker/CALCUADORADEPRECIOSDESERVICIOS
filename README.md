# CALCUADORADEPRECIOSDESERVICIOS
CALCULADORA DE PRECIOS 
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Calculadora 303 ARQ</title>
  <style>
    :root{
      --bg:#f3f3f3;
      --card:#ffffff;
      --accent:#555;
      --muted:#777;
      --pad:18px;
      --radius:12px;
    }
    body{font-family:Inter,system-ui,Arial,sans-serif;background:var(--bg);color:#222;margin:0;padding:24px;}
    h1,h2{text-align:center;color:#333;margin:0 0 14px;}
    .wrap{max-width:980px;margin:16px auto;display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:18px;padding:6px;}
    .card{background:var(--card);border-radius:var(--radius);padding:var(--pad);box-shadow:0 6px 18px rgba(0,0,0,0.06);}
    label{display:block;margin:8px 0 6px;font-size:14px;color:var(--muted);}
    input[type="number"], select{width:100%;padding:10px;border:1px solid #ddd;border-radius:8px;font-size:15px;}
    button{display:inline-block;padding:10px 14px;border-radius:10px;border:none;background:var(--accent);color:#fff;font-weight:600;cursor:pointer;margin-top:10px;}
    .result{margin-top:12px;padding:12px;border-left:4px solid var(--accent);background:#fafafa;border-radius:8px;}
    .small{font-size:13px;color:var(--muted);}
    .row{display:flex;gap:8px}
    .row > *{flex:1}
    @media (max-width:420px){body{padding:12px}}
  </style>
</head>
<body>
  <h1>Calculadoras 303 ARQ</h1>
  <p style="text-align:center;color:var(--muted);margin:6px 0 20px;">Calcula área total (m²) por pisos y cotiza servicios o construcción. Compatible con móvil.</p>

  <div class="wrap">
    <!-- Servicios -->
    <div class="card" id="card-servicios">
      <h2>Servicios — Diseño y Planos</h2>

      <label>Largo (m)</label>
      <input type="number" id="largoServicios" placeholder="Ej: 10" min="0" />

      <label>Ancho (m)</label>
      <input type="number" id="anchoServicios" placeholder="Ej: 8" min="0" />

      <label>Número de pisos</label>
      <input type="number" id="pisosServicios" value="1" min="1" />

      <button onclick="calcularServicios()">Calcular — Servicios</button>

      <div class="result" id="resultadoServicios"></div>
    </div>

    <!-- Obra -->
    <div class="card" id="card-obra">
      <h2>Construcción — Obra</h2>

      <label>Largo (m)</label>
      <input type="number" id="largoObra" placeholder="Ej: 10" min="0" />

      <label>Ancho (m)</label>
      <input type="number" id="anchoObra" placeholder="Ej: 8" min="0" />

      <label>Número de pisos</label>
      <input type="number" id="pisosObra" value="1" min="1" />

      <button onclick="calcularObra()">Calcular — Obra</button>

      <div class="result" id="resultadoObra"></div>
    </div>
  </div>

<script>
/* === CONFIGURACIÓN: cambia estos valores según necesites === */
const PHONE_NUMBER = "593XXXXXXXXX"; // Reemplaza por tu número sin +, sin espacios. Ej: 593991234567
const preciosServicios = {
  modelado: 4.5,
  planosArquitectonicos: 5.5,
  planosCompletos: 9.5,
  paqueteLegalizado: 14.5
};
const preciosConstruccion = {
  obraGris: 250,     // ajusta
  obraBlanca: 400,   // ajusta
  llaveEnMano: 600   // ajusta
};
/* ========================================================= */

function formatMoney(v){ return "$" + Number(v).toLocaleString(undefined,{minimumFractionDigits:2,maximumFractionDigits:2}); }

function crearBotonWhatsApp(mensaje){
  const url = "https://wa.me/" + PHONE_NUMBER + "?text=" + encodeURIComponent(mensaje);
  return `<div style="margin-top:10px;"><button onclick="window.open('${url}','_blank')">Enviar cotización por WhatsApp</button></div>`;
}

/* --- Servicios --- */
function calcularServicios(){
  const l = parseFloat(document.getElementById("largoServicios").value);
  const a = parseFloat(document.getElementById("anchoServicios").value);
  const p = parseInt(document.getElementById("pisosServicios").value) || 1;
  const out = document.getElementById("resultadoServicios");
  if(isNaN(l)||isNaN(a)||l<=0||a<=0||p<=0){ out.innerHTML = '<span class="small">Por favor introduce valores válidos (mayores que 0).</span>'; return; }
  const areaBase = l * a;
  const areaTotal = areaBase * p;
  const rModelado = areaTotal * preciosServicios.modelado;
  const rPlanos = areaTotal * preciosServicios.planosArquitectonicos;
  const rCompletos = areaTotal * preciosServicios.planosCompletos;
  const rLegal = areaTotal * preciosServicios.paqueteLegalizado;

  let html = `<div><strong>Área base:</strong> ${areaBase} m²<br><strong>Pisos:</strong> ${p}<br><strong>Área total:</strong> ${areaTotal} m²</div><hr>`;
  html += `<div class="small">
    Modelado y diseño: ${formatMoney(rModelado)} (${preciosServicios.modelado}$/m²)<br>
    Planos arquitectónicos: ${formatMoney(rPlanos)} (${preciosServicios.planosArquitectonicos}$/m²)<br>
    Planos completos: ${formatMoney(rCompletos)} (${preciosServicios.planosCompletos}$/m²)<br>
    Paquete legalizado: ${formatMoney(rLegal)} (${preciosServicios.paqueteLegalizado}$/m²)
  </div>`;

  // WhatsApp message
  const msg = `Hola 👋 quiero cotizar servicios de diseño para un terreno de ${areaTotal} m² (${areaBase}x${p} pisos). 
Modelado: ${formatMoney(rModelado)}. 
Planos: ${formatMoney(rPlanos)}. 
Planos completos: ${formatMoney(rCompletos)}. 
Paquete legalizado: ${formatMoney(rLegal)}.
Por favor contáctenme.`;
  html += crearBotonWhatsApp(msg);
  out.innerHTML = html;
}

/* --- Obra --- */
function calcularObra(){
  const l = parseFloat(document.getElementById("largoObra").value);
  const a = parseFloat(document.getElementById("anchoObra").value);
  const p = parseInt(document.getElementById("pisosObra").value) || 1;
  const out = document.getElementById("resultadoObra");
  if(isNaN(l)||isNaN(a)||l<=0||a<=0||p<=0){ out.innerHTML = '<span class="small">Por favor introduce valores válidos (mayores que 0).</span>'; return; }
  const areaBase = l * a;
  const areaTotal = areaBase * p;
  const rGris = areaTotal * preciosConstruccion.obraGris;
  const rBlanca = areaTotal * preciosConstruccion.obraBlanca;
  const rLlave = areaTotal * preciosConstruccion.llaveEnMano;

  let html = `<div><strong>Área base:</strong> ${areaBase} m²<br><strong>Pisos:</strong> ${p}<br><strong>Área total:</strong> ${areaTotal} m²</div><hr>`;
  html += `<div class="small">
    Obra gris: ${formatMoney(rGris)} (${preciosConstruccion.obraGris}$/m²)<br>
    Obra blanca: ${formatMoney(rBlanca)} (${preciosConstruccion.obraBlanca}$/m²)<br>
    Llave en mano: ${formatMoney(rLlave)} (${preciosConstruccion.llaveEnMano}$/m²)
  </div>`;

  const msg = `Hola 👋 quiero cotizar construcción para ${areaTotal} m² (${areaBase}x${p} pisos). 
Obra gris: ${formatMoney(rGris)}.
Obra blanca: ${formatMoney(rBlanca)}.
Llave en mano: ${formatMoney(rLlave)}.
Por favor contáctenme.`;
  html += crearBotonWhatsApp(msg);
  out.innerHTML = html;
}
</script>
</body>
</html>
