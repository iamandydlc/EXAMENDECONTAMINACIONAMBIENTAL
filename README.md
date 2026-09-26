<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Laboratorio Virtual - Contaminación Ambiental</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5/dist/echarts.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Figtree:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        * { font-family: 'Figtree', sans-serif; }
        .info-section { transition: opacity 0.4s ease, filter 0.4s ease; }
        .info-section.locked { 
            opacity: 0.4; 
            filter: blur(4px); 
            pointer-events: none;
            user-select: none;
        }
        .modal-backdrop { backdrop-filter: blur(6px); }
        .question-card { transition: all 0.3s ease; }
        .option-btn { transition: all 0.2s ease; }
        .option-btn:hover:not(.selected):not(.correct):not(.wrong) {
            transform: translateX(6px);
            border-color: #0d9669;
        }
        .option-btn.selected { border-color: #4f46e5; background: #eef2ff; }
        .option-btn.correct { border-color: #10b981; background: #ecfdf5; }
        .option-btn.wrong { border-color: #ef4444; background: #fef2f2; }
        .result-stamp { animation: popIn 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        @keyframes popIn {
            0% { transform: scale(0.7); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        .gradient-text {
            background: linear-gradient(120deg, #0d9669, #4f46e5);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        /* Corrección: área PDF visible pero fuera de pantalla */
        #contenidoPDF {
            position: absolute;
            left: -9999px;
            top: 0;
            width: 210mm;
            background: white;
            z-index: -1;
        }
        @media print {
            #contenidoPDF { position: static; left: auto; width: auto; }
            .no-print { display: none !important; }
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen">

<!-- Barra de Navegación -->
<nav class="bg-white shadow-md sticky top-0 z-40 no-print">
    <div class="max-w-7xl mx-auto px-4 py-3 flex justify-between items-center">
        <div class="flex items-center gap-2">
            <span class="text-2xl">🌍</span>
            <h1 class="text-lg font-bold">Lab. Contaminación Ambiental</h1>
        </div>
        <div class="flex gap-3">
            <button id="btnRepasar" onclick="mostrarRepasar()" class="px-4 py-2 rounded-lg font-medium border border-slate-200 hover:bg-slate-50">
                📚 Repasar
            </button>
            <button id="btnExamen" onclick="abrirAdvertencia()" class="px-4 py-2 rounded-lg font-medium bg-emerald-600 text-white hover:bg-emerald-700">
                📝 Realizar Examen
            </button>
        </div>
    </div>
</nav>

<!-- Contenido Principal -->
<main class="max-w-7xl mx-auto px-4 py-8">

    <!-- Sección de Información -->
    <div id="seccionInformacion" class="info-section">
        <header class="mb-10 text-center">
            <h2 class="text-4xl font-extrabold mb-3 gradient-text">Contaminación Ambiental</h2>
            <p class="text-lg text-slate-600 max-w-2xl mx-auto">Todo lo que necesitas saber sobre el impacto humano en el planeta: causas, tipos, consecuencias y soluciones. Estudia bien antes de evaluarte.</p>
        </header>

        <!-- Tarjetas de Datos Clave -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-5 mb-10">
            <div class="bg-white rounded-xl p-5 shadow-sm border border-slate-100">
                <p class="text-xs text-slate-500 uppercase font-semibold">CO₂ Atmosférico (2024)</p>
                <p class="text-3xl font-bold text-emerald-600 mt-2">424.6 <span class="text-sm font-normal text-slate-500">ppm</span></p>
                <p class="text-xs text-red-500 mt-1">Nivel más alto registrado</p>
            </div>
            <div class="bg-white rounded-xl p-5 shadow-sm border border-slate-100">
                <p class="text-xs text-slate-500 uppercase font-semibold">Calentamiento Global</p>
                <p class="text-3xl font-bold text-orange-500 mt-2">+1.47 <span class="text-sm font-normal text-slate-500">°C</span></p>
                <p class="text-xs text-slate-500 mt-1">vs era preindustrial</p>
            </div>
            <div class="bg-white rounded-xl p-5 shadow-sm border border-slate-100">
                <p class="text-xs text-slate-500 uppercase font-semibold">Emisiones CO₂ Fósil</p>
                <p class="text-3xl font-bold text-blue-600 mt-2">38.6 <span class="text-sm font-normal text-slate-500">Gt/año</span></p>
                <p class="text-xs text-slate-500 mt-1">Global Carbon Project</p>
            </div>
            <div class="bg-white rounded-xl p-5 shadow-sm border border-slate-100">
                <p class="text-xs text-slate-500 uppercase font-semibold">Muertes por Aire</p>
                <p class="text-3xl font-bold text-red-600 mt-2">7 <span class="text-sm font-normal text-slate-500">millones/año</span></p>
                <p class="text-xs text-slate-500 mt-1">Estimación OMS</p>
            </div>
        </div>

        <!-- Sección de Estudio -->
        <section class="bg-white rounded-xl p-6 shadow-sm border border-slate-100 mb-8">
            <h3 class="text-xl font-bold mb-4 flex items-center gap-2">📘 1. ¿Qué es la contaminación ambiental?</h3>
            <p class="mb-3">Es la introducción en el medio ambiente de elementos o sustancias nocivas que alteran su equilibrio natural, causando daño a los seres vivos, recursos naturales o el clima. Puede ser de origen natural (erupciones volcánicas) o, principalmente, <strong>antrópica</strong> (generada por el ser humano).</p>
            
            <h3 class="text-xl font-bold mb-4 mt-6 flex items-center gap-2">🏭 2. Tipos de contaminación</h3>
            <ul class="space-y-3 mb-4">
                <li><strong>• Atmosférica:</strong> Emisión de gases de efecto invernadero (CO₂, metano, óxidos de nitrógeno) y partículas. Proviene de transporte, industria, generación eléctrica. Es la principal causa del calentamiento global.</li>
                <li><strong>• Del agua:</strong> Vertidos industriales, agrícolas (fertilizantes), plásticos y aguas residuales. Afecta ríos, mares y fuentes de agua potable.</li>
                <li><strong>• Del suelo:</strong> Pesticidas, metales pesados, residuos sólidos y plásticos que degradan la tierra fértil.</li>
                <li><strong>• Acústica, lumínica y térmica:</strong> Ruido excesivo, sobreiluminación y alteración de temperaturas que afectan la biodiversidad y la salud humana.</li>
            </ul>

            <h3 class="text-xl font-bold mb-4 mt-6 flex items-center gap-2">⚠️ 3. Principales causas</h3>
            <ul class="space-y-2 mb-4">
                <li>🔹 Quema de combustibles fósiles (carbón, petróleo, gas) para energía y transporte → libera el 75% de los gases de efecto invernadero.</li>
                <li>🔹 Deforestación: se pierden pulmones naturales que absorben CO₂; se talan más de 10 millones de hectáreas anuales.</li>
                <li>🔹 Industria y producción masiva: generación de residuos, emisiones químicas y calentamiento.</li>
                <li>🔹 Agricultura intensiva: ganadería emite metano; fertilizantes liberan óxido nitroso, un gas 300 veces más potente que el CO₂.</li>
            </ul>

            <h3 class="text-xl font-bold mb-4 mt-6 flex items-center gap-2">🌡️ 4. Consecuencias</h3>
            <ul class="space-y-2 mb-4">
                <li>🔸 <strong>Cambio climático:</strong> aumento de temperatura, derretimiento de polos, subida del nivel del mar, fenómenos extremos (sequías, huracanes).</li>
                <li>🔸 <strong>Pérdida de biodiversidad:</strong> especies en extinción, degradación de ecosistemas, arrecifes coralinos desapareciendo.</li>
                <li>🔸 <strong>Salud humana:</strong> enfermedades respiratorias, cardiovasculares, alergias, cáncer y muertes prematuras.</li>
                <li>🔸 <strong>Escasez de recursos:</strong> agua potable, suelo fértil y alimentos en riesgo.</li>
            </ul>

            <h3 class="text-xl font-bold mb-4 mt-6 flex items-center gap-2">💡 5. Soluciones y acciones</h3>
            <ul class="space-y-2">
                <li>✅ Transición a energías renovables (solar, eólica, hidráulica)</li>
                <li>✅ Reducción, reutilización y reciclaje; menor uso de plásticos de un solo uso</li>
                <li>✅ Reforestación y protección de bosques</li>
                <li>✅ Transporte sostenible: caminar, bicicleta, transporte público, vehículos eficientes</li>
                <li>✅ Educación y leyes internacionales como el Acuerdo de París (limitar calentamiento a 1.5-2 °C)</li>
            </ul>
        </section>

        <!-- Gráfico Informativo -->
        <section class="bg-white rounded-xl p-6 shadow-sm border border-slate-100 mb-8">
            <h3 class="text-xl font-bold mb-4">📈 Evolución de las Emisiones de CO₂ (1900–2024)</h3>
            <div id="graficoInfo" style="height: 350px;"></div>
        </section>
    </div>

    <!-- Sección del Examen (Oculta al inicio) -->
    <div id="seccionExamen" class="hidden">
        <header class="mb-8 text-center">
            <h2 class="text-3xl font-extrabold text-indigo-600 mb-2">📝 Examen de Contaminación Ambiental</h2>
            <p class="text-slate-500">Responde las siguientes preguntas y comprueba tus conocimientos.</p>
        </header>

        <form id="formExamen" class="space-y-6 mb-8">
            <!-- Pregunta 1 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">1.</span> ¿Cuál es la concentración de CO₂ en la atmósfera registrada en 2024?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q1" value="a" class="hidden"> A) 280 ppm
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q1" value="b" class="hidden"> B) 350 ppm
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q1" value="c" class="hidden"> C) 424.6 ppm
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q1" value="d" class="hidden"> D) 500 ppm
                    </label>
                </div>
            </div>

            <!-- Pregunta 2 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">2.</span> ¿En cuántos grados ha aumentado la temperatura global respecto a la era preindustrial (dato 2024)?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q2" value="a" class="hidden"> A) +0.5 °C
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q2" value="b" class="hidden"> B) +1.47 °C
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q2" value="c" class="hidden"> C) +3.0 °C
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q2" value="d" class="hidden"> D) +5.0 °C
                    </label>
                </div>
            </div>

            <!-- Pregunta 3 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">3.</span> ¿Qué gas produce la ganadería y es mucho más potente que el CO₂?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q3" value="a" class="hidden"> A) Oxígeno
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q3" value="b" class="hidden"> B) Hidrógeno
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q3" value="c" class="hidden"> C) Metano
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q3" value="d" class="hidden"> D) Nitrógeno
                    </label>
                </div>
            </div>

            <!-- Pregunta 4 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">4.</span> El Acuerdo de París busca limitar el calentamiento global por debajo de:</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q4" value="a" class="hidden"> A) 1 °C
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q4" value="b" class="hidden"> B) 2 °C (aspirando a 1.5 °C)
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q4" value="c" class="hidden"> C) 5 °C
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q4" value="d" class="hidden"> D) 10 °C
                    </label>
                </div>
            </div>

            <!-- Pregunta 5 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">5.</span> ¿Cuál de las siguientes NO es una energía renovable?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q5" value="a" class="hidden"> A) Energía solar
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q5" value="b" class="hidden"> B) Energía eólica
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q5" value="c" class="hidden"> C) Energía de carbón
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q5" value="d" class="hidden"> D) Energía hidráulica
                    </label>
                </div>
            </div>

            <!-- Pregunta 6 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">6.</span> ¿Qué tipo de contaminación afecta principalmente ríos, mares y fuentes de agua potable?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q6" value="a" class="hidden"> A) Contaminación acústica
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q6" value="b" class="hidden"> B) Contaminación del agua
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q6" value="c" class="hidden"> C) Contaminación lumínica
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q6" value="d" class="hidden"> D) Contaminación del suelo
                    </label>
                </div>
            </div>

            <!-- Pregunta 7 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">7.</span> ¿Cuántas muertes prematuras estima la OMS anualmente por contaminación del aire?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q7" value="a" class="hidden"> A) 1 millón
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q7" value="b" class="hidden"> B) 3 millones
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q7" value="c" class="hidden"> C) 7 millones
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q7" value="d" class="hidden"> D) 15 millones
                    </label>
                </div>
            </div>

            <!-- Pregunta 8 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">8.</span> ¿Qué actividad libera el 75% de los gases de efecto invernadero?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q8" value="a" class="hidden"> A) Quema de combustibles fósiles
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q8" value="b" class="hidden"> B) Plantación de árboles
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q8" value="c" class="hidden"> C) Reciclaje de plástico
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q8" value="d" class="hidden"> D) Uso de energía solar
                    </label>
                </div>
            </div>

            <!-- Pregunta 9 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">9.</span> ¿Qué consecuencia NO corresponde con el cambio climático?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q9" value="a" class="hidden"> A) Subida del nivel del mar
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q9" value="b" class="hidden"> B) Más fenómenos extremos
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q9" value="c" class="hidden"> C) Aumento de biodiversidad
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q9" value="d" class="hidden"> D) Derretimiento de glaciares
                    </label>
                </div>
            </div>

            <!-- Pregunta 10 -->
            <div class="question-card bg-white rounded-xl p-6 shadow-sm border border-slate-100">
                <p class="font-bold text-lg mb-4"><span class="text-indigo-600">10.</span> ¿Cuál es una acción correcta para proteger el medio ambiente?</p>
                <div class="space-y-3">
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q10" value="a" class="hidden"> A) Usar plásticos de un solo uso
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q10" value="b" class="hidden"> B) Caminar o usar transporte público
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q10" value="c" class="hidden"> C) Dejar luces encendidas sin uso
                    </label>
                    <label class="option-btn block w-full text-left p-3 border rounded-lg cursor-pointer">
                        <input type="radio" name="q10" value="d" class="hidden"> D) Talar bosques para agricultura
                    </label>
                </div>
            </div>

            <button type="submit" class="w-full py-3 bg-indigo-600 text-white rounded-xl font-bold hover:bg-indigo-700 transition-colors text-lg">
                ✅ Enviar Examen
            </button>
        </form>

        <!-- Tarjeta de Resultados -->
        <div id="tarjetaResultados" class="hidden bg-white rounded-xl p-8 shadow-lg border border-slate-100 text-center">
            <div class="result-stamp mb-6">
                <div id="iconoNota" class="text-6xl mb-3"></div>
                <h3 class="text-2xl font-bold mb-2">¡Examen Finalizado!</h3>
                <p id="fraseNota" class="text-lg text-slate-500 mb-4"></p>
                <div id="notaFinal" class="text-5xl font-extrabold mb-2"></div>
                <p id="detalleNota" class="text-slate-500 mb-6"></p>
            </div>
            
            <div id="resumenRespuestas" class="text-left bg-slate-50 rounded-lg p-4 mb-6 text-sm"></div>

            <div class="flex flex-col sm:flex-row gap-4 justify-center">
                <button onclick="mostrarRepasar()" class="px-6 py-3 border border-slate-200 rounded-lg font-medium hover:bg-slate-50">
                    📚 Volver a Repasar
                </button>
                <button id="btnDescargarPDF" class="px-6 py-3 bg-emerald-600 text-white rounded-lg font-medium hover:bg-emerald-700">
                    📄 Descargar Examen en PDF
                </button>
            </div>
        </div>
    </div>

</main>

<!-- Modal de Advertencia -->
<div id="modalAdvertencia" class="fixed inset-0 z-50 hidden flex items-center justify-center no-print">
    <div class="modal-backdrop absolute inset-0 bg-black/50"></div>
    <div class="relative bg-white rounded-2xl p-8 max-w-md w-full mx-4 shadow-2xl">
        <div class="text-center">
            <div class="text-5xl mb-4">⚠️</div>
            <h3 class="text-2xl font-bold mb-3">¿Estás seguro?</h3>
            <p class="text-slate-600 mb-6">Al iniciar el examen, el contenido de repaso se bloqueará temporalmente. No podrás consultar la información mientras respondes.</p>
            <div class="flex flex-col sm:flex-row gap-3">
                <button onclick="cerrarAdvertencia()" class="flex-1 py-3 border border-slate-200 rounded-xl font-medium hover:bg-slate-50">
                    ← Volver a Repasar
                </button>
                <button onclick="iniciarExamen()" class="flex-1 py-3 bg-emerald-600 text-white rounded-xl font-bold hover:bg-emerald-700">
                    ✅ Continuar al Examen
                </button>
            </div>
        </div>
    </div>
</div>

<!-- Área para PDF (corregida: visible fuera de pantalla) -->
<div id="contenidoPDF">
    <div style="border-bottom: 2px solid #0d9669; padding-bottom: 16px; margin-bottom: 24px;">
        <h1 style="font-size: 28px; font-weight: 800; color: #0f172a; margin: 0;">Laboratorio Virtual de Contaminación Ambiental</h1>
        <p style="color: #64748b; margin: 4px 0 0 0;">Certificado de Realización de Examen</p>
    </div>
    <p style="margin: 4px 0;"><strong>Fecha:</strong> <span id="pdfFecha"></span></p>
    <p style="margin: 4px 0;"><strong>Evaluación:</strong> Examen General de Conocimientos</p>
    <div id="pdfNota" style="margin: 24px 0; padding: 20px; border-radius: 12px; background: #f0fdf4; text-align: center;"></div>
    <h3 style="margin-top: 32px; border-bottom: 1px solid #e2e8f0; padding-bottom: 8px;">Detalle de Respuestas</h3>
    <div id="pdfDetalle"></div>
    <p style="margin-top: 32px; padding-top: 16px; border-top: 1px solid #e2e8f0; font-size: 12px; color: #94a3b8;">Fuentes: Global Carbon Project, NOAA, NASA GISS, OMS. Datos actualizados a 2024.</p>
</div>

<script>
// ==================== DATOS Y CONFIGURACIÓN ====================
const respuestasCorrectas = {
    q1: 'c', q2: 'b', q3: 'c', q4: 'b', q5: 'c',
    q6: 'b', q7: 'c', q8: 'a', q9: 'c', q10: 'b'
};

const nombresPreguntas = {
    q1: 'Concentración de CO₂ 2024',
    q2: 'Aumento de temperatura global',
    q3: 'Gas de la ganadería',
    q4: 'Objetivo del Acuerdo de París',
    q5: 'Energía NO renovable',
    q6: 'Tipo de contaminación de ríos y mares',
    q7: 'Muertes por contaminación del aire (OMS)',
    q8: 'Actividad principal de emisiones',
    q9: 'Consecuencia que NO corresponde',
    q10: 'Acción correcta contra la contaminación'
};

let examenIniciado = false;
let examenFinalizado = false;

// ==================== GRÁFICO ====================
document.addEventListener('DOMContentLoaded', () => {
    const grafico = echarts.init(document.getElementById('graficoInfo'));
    grafico.setOption({
        tooltip: { trigger: 'axis', backgroundColor: '#fff', borderColor: '#e2e8f0' },
        grid: { left: '3%', right: '4%', bottom: '3%', containLabel: true },
        xAxis: { type: 'category', data: ['1900','1920','1940','1960','1980','2000','2010','2020','2024'], axisLabel: { color: '#64748b' } },
        yAxis: { type: 'value', name: 'Gt CO₂/año', nameTextStyle: { color: '#64748b' }, axisLabel: { color: '#64748b' } },
        series: [{
            name: 'Emisiones CO₂',
            type: 'line',
            smooth: true,
            symbolSize: 8,
            lineStyle: { width: 3, color: '#0d9669' },
            itemStyle: { color: '#0d9669' },
            areaStyle: { color: { type: 'linear', x: 0, y: 0, x2: 0, y2: 1, colorStops: [
                { offset: 0, color: 'rgba(13,150,105,0.3)' },
                { offset: 1, color: 'rgba(13,150,105,0.05)' }
            ]}},
            data: [2.0, 3.8, 5.0, 9.4, 19.5, 25.5, 33.4, 34.8, 38.6]
        }]
    });
    window.addEventListener('resize', () => grafico.resize());
});

// ==================== FUNCIONES DE NAVEGACIÓN ====================
function abrirAdvertencia() {
    if (examenFinalizado) {
        alert('✅ Ya completaste el examen. Puedes descargar tu certificado o volver a repasar.');
        return;
    }
    document.getElementById('modalAdvertencia').classList.remove('hidden');
}

function cerrarAdvertencia() {
    document.getElementById('modalAdvertencia').classList.add('hidden');
}

function iniciarExamen() {
    cerrarAdvertencia();
    examenIniciado = true;
    // Bloquear información
    document.getElementById('seccionInformacion').classList.add('locked');
    // Ocultar info, mostrar examen
    document.getElementById('seccionInformacion').style.display = 'none';
    document.getElementById('seccionExamen').classList.remove('hidden');
    // Desplazarse al examen
    document.getElementById('seccionExamen').scrollIntoView({ behavior: 'smooth' });
}

function mostrarRepasar() {
    if (!examenIniciado) {
        document.getElementById('seccionInformacion').style.display = 'block';
        document.getElementById('seccionExamen').classList.add('hidden');
        document.getElementById('tarjetaResultados').classList.add('hidden');
        return;
    }
    if (examenFinalizado) {
        document.getElementById('seccionInformacion').classList.remove('locked');
        document.getElementById('seccionInformacion').style.display = 'block';
        document.getElementById('seccionExamen').classList.add('hidden');
        document.getElementById('formExamen').classList.remove('hidden');
        document.getElementById('tarjetaResultados').classList.add('hidden');
        document.getElementById('formExamen').reset();
        document.querySelectorAll('.option-btn').forEach(b => {
            b.classList.remove('selected', 'correct', 'wrong');
        });
        examenIniciado = false;
        examenFinalizado = false;
    }
}

// ==================== SELECCIÓN DE OPCIONES ====================
document.querySelectorAll('.option-btn').forEach(boton => {
    boton.addEventListener('click', () => {
        const input = boton.querySelector('input');
        const nombre = input.name;
        document.querySelectorAll(`input[name="${nombre}"]`).forEach(i => {
            i.closest('.option-btn').classList.remove('selected');
        });
        boton.classList.add('selected');
        input.checked = true;
    });
});

// ==================== ENVÍO Y CALIFICACIÓN ====================
document.getElementById('formExamen').addEventListener('submit', function(e) {
    e.preventDefault();
    
    let aciertos = 0;
    let total = Object.keys(respuestasCorrectas).length;
    let resumenHTML = '';
    let pdfDetalleHTML = '';

    for (const [pregunta, correcta] of Object.entries(respuestasCorrectas)) {
        const seleccion = document.querySelector(`input[name="${pregunta}"]:checked`);
        const valor = seleccion ? seleccion.value : null;
        const todos = document.querySelectorAll(`input[name="${pregunta}"]`);
        
        todos.forEach(i => {
            const lbl = i.closest('.option-btn');
            lbl.classList.remove('selected');
            if (i.value === correcta) {
                lbl.classList.add('correct');
            } else if (i.checked && i.value !== correcta) {
                lbl.classList.add('wrong');
            }
        });

        const esCorrecta = valor === correcta;
        if (esCorrecta) aciertos++;

        const estado = esCorrecta ? '✅ CORRECTA' : '❌ INCORRECTA';
        const textoSel = seleccion ? seleccion.parentElement.textContent.trim() : 'No respondida';
        const textoCorr = document.querySelector(`input[name="${pregunta}"][value="${correcta}"]`).parentElement.textContent.trim();

        resumenHTML += `<div class="py-2 border-b border-slate-100">
            <strong>${pregunta.replace('q','Pregunta ')}</strong>: ${estado}<br>
            <span class="text-slate-500">Tu respuesta: ${textoSel}</span><br>
            <span class="text-emerald-600">Respuesta correcta: ${textoCorr}</span>
        </div>`;

        pdfDetalleHTML += `<div style="padding: 10px 0; border-bottom: 1px solid #eee;">
            <strong>${nombresPreguntas[pregunta]}</strong> — ${esCorrecta ? '✅ Acierto' : '❌ Fallo'}<br>
            <span style="color:#666;">Respuesta elegida: ${textoSel || 'Sin respuesta'}</span><br>
            <span style="color:#0d9669;">Respuesta correcta: ${textoCorr}</span>
        </div>`;
    }

    const nota = (aciertos / total * 10).toFixed(1);
    examenFinalizado = true;

    document.getElementById('formExamen').classList.add('hidden');
    const tarjeta = document.getElementById('tarjetaResultados');
    tarjeta.classList.remove('hidden');
    
    const icono = document.getElementById('iconoNota');
    const frase = document.getElementById('fraseNota');
    const notaEl = document.getElementById('notaFinal');
    const detalle = document.getElementById('detalleNota');

    if (nota >= 9) {
        icono.textContent = '🏆';
        frase.textContent = '¡Excelente! Demuestras gran conocimiento.';
        notaEl.style.color = '#0d9669';
    } else if (nota >= 7) {
        icono.textContent = '🎉';
        frase.textContent = '¡Bien hecho! Sigues por buen camino.';
        notaEl.style.color = '#4f46e5';
    } else if (nota >= 5) {
        icono.textContent = '📖';
        frase.textContent = 'Aprobado, pero puedes repasar más.';
        notaEl.style.color = '#d4724a';
    } else {
        icono.textContent = '🔄';
        frase.textContent = 'No alcanzaste el mínimo. Vuelve a estudiar.';
        notaEl.style.color = '#ef4444';
    }

    notaEl.textContent = `${nota} / 10`;
    detalle.textContent = `${aciertos} de ${total} respuestas correctas`;
    document.getElementById('resumenRespuestas').innerHTML = resumenHTML;

    const fecha = new Date().toLocaleString('es-ES', { dateStyle: 'long', timeStyle: 'short' });
    document.getElementById('pdfFecha').textContent = fecha;
    document.getElementById('pdfNota').innerHTML = `
        <p style="margin: 0; font-size: 16px; color: #0d9669;">Calificación Obtenida</p>
        <p style="margin: 8px 0; font-size: 42px; font-weight: 800; color: #0f172a;">${nota} / 10</p>
        <p style="margin: 0; color: #64748b;">${aciertos} de ${total} preguntas correctas</p>
    `;
    document.getElementById('pdfDetalle').innerHTML = pdfDetalleHTML;
});

// ==================== DESCARGAR PDF CORREGIDA ====================
document.getElementById('btnDescargarPDF').addEventListener('click', async () => {
    const elemento = document.getElementById('contenidoPDF');
    const nombreArchivo = `Examen_Contaminacion_Ambiental_${new Date().toLocaleDateString('es-ES').replaceAll('/','-')}.pdf`;
    
    const opciones = {
        margin: 15,
        filename: nombreArchivo,
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { 
            scale: 2, 
            useCORS: true,
            logging: false,
            letterRendering: true
        },
        jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' },
        pagebreak: { mode: ['avoid-all', 'css', 'legacy'] }
    };

    try {
        await window.html2pdf()
            .set(opciones)
            .from(elemento)
            .save();
    } catch (err) {
        console.error('Error al generar PDF:', err);
        alert('❌ Hubo un problema al generar el PDF. Inténtalo de nuevo.');
    }
});
</script>

</body>
</html>
