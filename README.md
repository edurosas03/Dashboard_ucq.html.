# Dashboard_ucq.html.
Análisis de Universidades
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Dashboard - Competencia UCQ Querétaro</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f8f9fa;
      margin: 0;
      padding: 20px;
    }
    .container {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 30px;
    }
    .card {
      background: white;
      border-radius: 20px;
      padding: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    h2 {
      font-size: 24px;
      margin-bottom: 15px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th, td {
      padding: 10px;
      border-bottom: 1px solid #ddd;
      text-align: left;
    }
    th {
      background-color: #e9ecef;
    }
    canvas {
      max-width: 100%;
    }
    .export-buttons {
      margin-top: 20px;
    }
    .export-buttons button {
      margin-right: 10px;
      padding: 10px 15px;
      border: none;
      border-radius: 8px;
      background-color: #007bff;
      color: white;
      cursor: pointer;
    }
    .export-buttons button:hover {
      background-color: #0056b3;
    }
  </style>
</head>
<body>
  <h1>Dashboard: Competencia Directa de la Universidad Cuauhtémoc Querétaro</h1>
  <div class="export-buttons">
    <button onclick="exportToExcel()">Descargar Excel</button>
    <button onclick="exportToPDF()">Descargar PDF</button>
  </div>
  <div class="container">
    <div class="card">
      <h2>Principales Competidoras</h2>
      <table id="tablaCompetencia">
        <thead>
          <tr>
            <th>Universidad</th>
            <th>Tipo</th>
            <th>Carreras Destacadas</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Universidad Anáhuac Qro</td>
            <td>Privada, Prestigio</td>
            <td>Medicina, Derecho, Negocios</td>
          </tr>
          <tr>
            <td>ITESM Campus Qro</td>
            <td>Privada, Nacional</td>
            <td>Ingeniería, Negocios, Comunicación</td>
          </tr>
          <tr>
            <td>UVM Querétaro</td>
            <td>Privada, Red Nacional</td>
            <td>Administración, Derecho, Modalidad Online</td>
          </tr>
          <tr>
            <td>UNITEC Querétaro</td>
            <td>Privada, Red Laureate</td>
            <td>Comercio, Derecho, Administración</td>
          </tr>
          <tr>
            <td>UNIQ, Mondragón, Humanitas</td>
            <td>Privadas, Innovadoras</td>
            <td>Comunicación, Ingeniería, Administración</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="card">
      <h2>Comparativa de Carreras Similares</h2>
      <canvas id="chartCarreras"></canvas>
    </div>
    <div class="card">
      <h2>Ranking Regional (1 = mejor)</h2>
      <canvas id="chartRanking"></canvas>
    </div>
    <div class="card">
      <h2>Comparativa de Colegiaturas Aproximadas (MXN/mes)</h2>
      <canvas id="chartCostos"></canvas>
    </div>
    <div class="card">
      <h2>Modalidades y Duración de Carreras</h2>
      <table id="tablaModalidades">
        <thead>
          <tr>
            <th>Universidad</th>
            <th>Modalidades</th>
            <th>Duración promedio</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Universidad Cuauhtémoc</td>
            <td>Presencial / En línea</td>
            <td>3 años (cuatrimestres)</td>
          </tr>
          <tr>
            <td>Universidad Anáhuac</td>
            <td>Presencial</td>
            <td>4 a 4.5 años (semestres)</td>
          </tr>
          <tr>
            <td>ITESM Campus Querétaro</td>
            <td>Presencial / Híbrido</td>
            <td>4 a 4.5 años</td>
          </tr>
          <tr>
            <td>UVM Querétaro</td>
            <td>Presencial / En línea</td>
            <td>3 a 4 años</td>
          </tr>
          <tr>
            <td>UNITEC Querétaro</td>
            <td>Presencial / En línea</td>
            <td>3 años</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <script>
    // Gráficas
    const ctxCarreras = document.getElementById('chartCarreras');
    new Chart(ctxCarreras, {
      type: 'bar',
      data: {
        labels: ['Derecho', 'Negocios', 'Ingeniería', 'Comunicación', 'Medicina'],
        datasets: [
          {
            label: 'UCQ',
            data: [1, 1, 1, 1, 0],
            backgroundColor: '#007bff'
          },
          {
            label: 'Competencia',
            data: [5, 5, 4, 3, 1],
            backgroundColor: '#ffc107'
          }
        ]
      },
      options: {
        responsive: true,
        scales: {
          y: {
            beginAtZero: true,
            stepSize: 1
          }
        }
      }
    });

    const ctxRanking = document.getElementById('chartRanking');
    new Chart(ctxRanking, {
      type: 'bar',
      data: {
        labels: ['ITESM', 'Anáhuac', 'UVM', 'UNITEC', 'UCQ'],
        datasets: [{
          label: 'Ranking (1 = mejor)',
          data: [1, 2, 3, 4, 5],
          backgroundColor: '#28a745'
        }]
      },
      options: {
        indexAxis: 'y',
        scales: {
          x: {
            beginAtZero: true,
            reverse: true
          }
        }
      }
    });

    const ctxCostos = document.getElementById('chartCostos');
    new Chart(ctxCostos, {
      type: 'bar',
      data: {
        labels: ['ITESM', 'Anáhuac', 'UVM', 'UNITEC', 'UCQ'],
        datasets: [{
          label: 'Costo mensual (MXN)',
          data: [18000, 15000, 9000, 8500, 7500],
          backgroundColor: '#dc3545'
        }]
      },
      options: {
        responsive: true,
        scales: {
          y: {
            beginAtZero: true
          }
        }
      }
    });

    // Funciones de exportación
    function exportToExcel() {
      const wb = XLSX.utils.book_new();
      const tables = ['tablaCompetencia', 'tablaModalidades'];
      tables.forEach(id => {
        const ws = XLSX.utils.table_to_sheet(document.getElementById(id));
        XLSX.utils.book_append_sheet(wb, ws, id);
      });
      XLSX.writeFile(wb, 'competencia_ucq.xlsx');
    }

    async function exportToPDF() {
      const {{ jsPDF }} = window.jspdf;
      const doc = new jsPDF();
      doc.setFontSize(14);
      doc.text('Resumen de Competencia UCQ Querétaro', 10, 10);
      doc.setFontSize(10);
      doc.text('Universidades, modalidades, rankings y colegiaturas.', 10, 18);
      doc.save('competencia_ucq.pdf');
    }
  </script>
</body>
</html>
