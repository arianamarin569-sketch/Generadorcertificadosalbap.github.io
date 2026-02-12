<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CertiEngine Pro v6.0 - Full Control Edition</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Alex+Brush&family=Inter:wght@300;400;700&family=Montserrat:wght@400;700&family=Pinyon+Script&family=Playfair+Display:wght@700&family=Roboto+Mono&display=swap" rel="stylesheet">
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

    <style>
        :root {
            --bg: #0f172a;
            --sidebar: #1e293b;
            --accent: #22d3ee;
            --card: #334155;
            --text: #f1f5f9;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0; display: flex; height: 100vh; overflow: hidden;
        }

        /* --- SIDEBAR --- */
        .sidebar {
            width: 480px;
            background: var(--sidebar);
            border-right: 1px solid #475569;
            display: flex;
            flex-direction: column;
            box-shadow: 5px 0 15px rgba(0,0,0,0.5);
        }

        .sidebar-header { padding: 15px 20px; background: #0f172a; border-bottom: 1px solid #475569; }
        .sidebar-header h1 { margin: 0; font-size: 1.1rem; color: var(--accent); text-transform: uppercase; }

        .scroll-area { flex: 1; overflow-y: auto; padding: 15px; }

        details {
            background: var(--card);
            border-radius: 6px;
            margin-bottom: 10px;
            border: 1px solid #475569;
        }
        summary {
            padding: 12px;
            cursor: pointer;
            font-weight: 600;
            font-size: 0.85rem;
            background: #1e293b;
            display: flex; justify-content: space-between;
            align-items: center;
        }
        .section-content { padding: 12px; display: flex; flex-direction: column; gap: 8px; border-top: 1px solid #475569; }

        label { font-size: 0.7rem; color: #cbd5e1; text-transform: uppercase; font-weight: 700; }
        input[type="text"], textarea, select {
            background: #0f172a; border: 1px solid #475569;
            color: white; padding: 8px; border-radius: 4px; font-size: 0.85rem;
        }

        .control-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
        input[type="range"] { width: 100%; accent-color: var(--accent); }
        input[type="color"] { height: 35px; padding: 2px; width: 100%; border: none; cursor: pointer; background: none; }

        /* --- ACCIONES --- */
        .action-area { padding: 15px; background: #0f172a; }
        .btn-main {
            width: 100%; background: var(--accent); color: #0f172a;
            border: none; padding: 12px; border-radius: 6px;
            font-weight: 800; cursor: pointer; transition: 0.2s;
        }
        .btn-main:hover { filter: brightness(1.1); transform: translateY(-1px); }

        /* --- VISOR --- */
        .main-view {
            flex: 1; display: flex; flex-direction: column;
            align-items: center; justify-content: center; padding: 20px;
            background: #111827; position: relative;
        }

        .canvas-container {
            width: 100%; max-width: 950px;
            box-shadow: 0 30px 60px rgba(0,0,0,0.7);
            background: white; line-height: 0;
        }
        canvas { width: 100%; height: auto; }
        
        .status { position: absolute; bottom: 20px; font-family: monospace; color: var(--accent); font-size: 0.8rem; }
    </style>
</head>
<body>

    <aside class="sidebar">
        <div class="sidebar-header">
            <h1>CertiEngine Pro <small>v6.0</small></h1>
        </div>

        <div class="scroll-area">
            <details open>
                <summary>📁 Datos Maestros y Fondo</summary>
                <div class="section-content">
                    <label>Participantes (uno por línea)</label>
                    <textarea id="names" rows="4" oninput="render()">Juan Alberto Pérez</textarea>
                    <label>Imagen de Fondo</label>
                    <input type="file" id="bgIn" accept="image/*">
                </div>
            </details>

            <details>
                <summary>1. Título: CERTIFICADO</summary>
                <div class="section-content">
                    <input type="text" id="t1_val" value="CERTIFICADO" oninput="render()">
                    <div class="control-grid">
                        <select id="t1_f" onchange="render()"><option value="'Playfair Display'">Playfair</option><option value="'Montserrat'">Montserrat</option></select>
                        <input type="color" id="t1_c" value="#1e293b" oninput="render()">
                    </div>
                    <label>Eje X</label><input type="range" id="t1_x" min="0" max="2000" value="1000" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t1_y" min="0" max="1414" value="250" oninput="render()">
                    <label>Tamaño</label><input type="range" id="t1_s" min="20" max="250" value="100" oninput="render()">
                </div>
            </details>

            <details>
                <summary>2. Texto: OTORGADO A:</summary>
                <div class="section-content">
                    <input type="text" id="t2_val" value="OTORGADO A:" oninput="render()">
                    <div class="control-grid">
                        <select id="t2_f" onchange="render()"><option value="'Inter'">Inter</option><option value="'Montserrat'">Montserrat</option></select>
                        <input type="color" id="t2_c" value="#475569" oninput="render()">
                    </div>
                    <label>Eje X</label><input type="range" id="t2_x" min="0" max="2000" value="1000" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t2_y" min="0" max="1414" value="380" oninput="render()">
                </div>
            </details>

            <details>
                <summary>3. Nombre del Participante</summary>
                <div class="section-content">
                    <div class="control-grid">
                        <select id="t3_f" onchange="render()"><option value="'Pinyon Script'">Pinyon</option><option value="'Alex Brush'">Alex Brush</option><option value="'Montserrat'">Montserrat</option></select>
                        <input type="color" id="t3_c" value="#1e293b" oninput="render()">
                    </div>
                    <label>Eje X</label><input type="range" id="t3_x" min="0" max="2000" value="1000" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t3_y" min="0" max="1414" value="550" oninput="render()">
                    <label>Tamaño</label><input type="range" id="t3_s" min="50" max="300" value="180" oninput="render()">
                </div>
            </details>

            <details>
                <summary>4. Texto: Por su Participación...</summary>
                <div class="section-content">
                    <textarea id="t4_val" oninput="render()">Por su Participación y Aprobación en el curso:</textarea>
                    <div class="control-grid">
                        <select id="t4_f" onchange="render()"><option value="'Inter'">Inter</option><option value="'Montserrat'">Montserrat</option></select>
                        <input type="color" id="t4_c" value="#475569" oninput="render()">
                    </div>
                    <label>Eje X</label><input type="range" id="t4_x" min="0" max="2000" value="1000" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t4_y" min="0" max="1414" value="680" oninput="render()">
                </div>
            </details>

            <details>
                <summary>5. Nombre del Curso</summary>
                <div class="section-content">
                    <input type="text" id="t5_val" value="EXCEL EMPRESARIAL AVANZADO" oninput="render()">
                    <div class="control-grid">
                        <select id="t5_f" onchange="render()"><option value="'Montserrat'">Montserrat</option><option value="'Playfair Display'">Playfair</option></select>
                        <input type="color" id="t5_c" value="#0891b2" oninput="render()">
                    </div>
                    <label>Eje X</label><input type="range" id="t5_x" min="0" max="2000" value="1000" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t5_y" min="0" max="1414" value="780" oninput="render()">
                    <label>Tamaño</label><input type="range" id="t5_s" min="20" max="120" value="65" oninput="render()">
                </div>
            </details>

            <details>
                <summary>6. Texto Largo Descriptivo</summary>
                <div class="section-content">
                    <textarea id="t6_val" rows="3" oninput="render()">Desarrollando competencias en el manejo de tablas dinámicas, macros en VBA y análisis de datos financieros complejos con éxito.</textarea>
                    <label>Eje X</label><input type="range" id="t6_x" min="0" max="2000" value="1000" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t6_y" min="0" max="1414" value="880" oninput="render()">
                    <label>Ancho Máximo</label><input type="range" id="t6_w" min="500" max="1800" value="1400" oninput="render()">
                </div>
            </details>

            <details>
                <summary>7. Horas</summary>
                <div class="section-content">
                    <input type="text" id="t7_val" value="Duración: 40 Horas Cronológicas" oninput="render()">
                    <label>Eje X</label><input type="range" id="t7_x" min="0" max="2000" value="1000" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t7_y" min="0" max="1414" value="1020" oninput="render()">
                </div>
            </details>

            <details>
                <summary>8. Fecha</summary>
                <div class="section-content">
                    <input type="text" id="t8_val" value="24 de Febrero de 2026" oninput="render()">
                    <label>Eje X</label><input type="range" id="t8_x" min="0" max="2000" value="400" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t8_y" min="0" max="1414" value="1250" oninput="render()">
                </div>
            </details>

            <details>
                <summary>9. Firma</summary>
                <div class="section-content">
                    <input type="text" id="t9_val" value="Ing. Ricardo Soto - Director" oninput="render()">
                    <label>Eje X</label><input type="range" id="t9_x" min="0" max="2000" value="1600" oninput="render()">
                    <label>Eje Y</label><input type="range" id="t9_y" min="0" max="1414" value="1250" oninput="render()">
                </div>
            </details>
        </div>

        <div class="action-area">
            <button class="btn-main" onclick="processAll()">GENERAR PDF (.ZIP)</button>
            <div id="st" class="status">Sistema Listo.</div>
        </div>
    </aside>

    <main class="main-view">
        <div class="canvas-container">
            <canvas id="engine" width="2000" height="1414"></canvas>
        </div>
    </main>

    <script>
        const canvas = document.getElementById('engine');
        const ctx = canvas.getContext('2d');
        let bgImg = null;

        function get(id) { return document.getElementById(id); }
        function val(id) { return get(id).value; }
        function num(id) { return parseInt(val(id)); }

        function wrapText(text, x, y, maxWidth, lineHeight) {
            const words = text.split(' ');
            let line = '';
            for(let n = 0; n < words.length; n++) {
                const testLine = line + words[n] + ' ';
                if (ctx.measureText(testLine).width > maxWidth && n > 0) {
                    ctx.fillText(line, x, y);
                    line = words[n] + ' ';
                    y += lineHeight;
                } else { line = testLine; }
            }
            ctx.fillText(line, x, y);
        }

        function render(specificName = null) {
            ctx.clearRect(0,0,2000,1414);
            
            // Fondo
            if(bgImg) ctx.drawImage(bgImg, 0,0, 2000, 1414);
            else { ctx.fillStyle="#fff"; ctx.fillRect(0,0,2000,1414); ctx.strokeStyle="#eee"; ctx.lineWidth=20; ctx.strokeRect(0,0,2000,1414); }

            ctx.textAlign = 'center';

            // 1. Título
            ctx.fillStyle = val('t1_c'); ctx.font = `700 ${num('t1_s')}px ${val('t1_f')}`;
            ctx.fillText(val('t1_val'), num('t1_x'), num('t1_y'));

            // 2. Otorgado a
            ctx.fillStyle = val('t2_c'); ctx.font = `400 30px ${val('t2_f')}`;
            ctx.fillText(val('t2_val'), num('t2_x'), num('t2_y'));

            // 3. Nombre (Individual)
            const activeName = specificName || val('names').split('\n')[0] || "Nombre";
            ctx.fillStyle = val('t3_c'); ctx.font = `${num('t3_s')}px ${val('t3_f')}`;
            ctx.fillText(activeName, num('t3_x'), num('t3_y'));

            // 4. Por su Participación
            ctx.fillStyle = val('t4_c'); ctx.font = `400 28px ${val('t4_f')}`;
            ctx.fillText(val('t4_val'), num('t4_x'), num('t4_y'));

            // 5. Nombre Curso
            ctx.fillStyle = val('t5_c'); ctx.font = `700 ${num('t5_s')}px ${val('t5_f')}`;
            ctx.fillText(val('t5_val'), num('t5_x'), num('t5_y'));

            // 6. Texto Largo
            ctx.fillStyle = '#475569'; ctx.font = `300 30px 'Inter'`;
            wrapText(val('t6_val'), num('t6_x'), num('t6_y'), num('t6_w'), 42);

            // 7. Horas
            ctx.font = '600 28px "Inter"'; ctx.fillText(val('t7_val'), num('t7_x'), num('t7_y'));

            // 8 y 9 Fecha y Firma (Ajuste X independiente)
            ctx.font = '400 24px "Inter"'; ctx.fillText(val('t8_val'), num('t8_x'), num('t8_y'));
            ctx.font = '700 28px "Montserrat"'; ctx.fillText(val('t9_val'), num('t9_x'), num('t9_y'));
        }

        get('bgIn').onchange = (e) => {
            const r = new FileReader();
            r.onload = (ev) => { const i = new Image(); i.onload = () => { bgImg = i; render(); }; i.src = ev.target.result; };
            r.readAsDataURL(e.target.files[0]);
        };

        async function processAll() {
            const list = val('names').split('\n').filter(n => n.trim());
            const st = get('st'); const zip = new JSZip(); const { jsPDF } = window.jspdf;
            for(let i=0; i<list.length; i++){
                st.textContent = `Rendering ${i+1}/${list.length}`;
                render(list[i].trim()); await new Promise(r => setTimeout(r, 25));
                const doc = new jsPDF({ orientation: 'l', unit: 'px', format: [2000, 1414] });
                doc.addImage(canvas.toDataURL('image/jpeg', 0.8), 'JPEG', 0, 0, 2000, 1414);
                zip.file(`${list[i].trim()}.pdf`, doc.output('blob'));
            }
            st.textContent = "Packing...";
            saveAs(await zip.generateAsync({type:"blob"}), "Certificados.zip");
            st.textContent = "Listo!"; render();
        }

        document.fonts.ready.then(render);
    </script>
</body>
</html>
