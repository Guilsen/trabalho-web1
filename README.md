# trabalho-web1
Este projeto foi desenvolvido como parte da disciplina Programação Web 1, com o objetivo de praticar a criação de interfaces web. A aplicação permite visualizar de forma simples e organizada os valores gastos, valores ganhos e o saldo geral.

# Código

<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Dashboard Financeiro</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #121214;
      color: white;
      padding: 20px;
    }
    .top-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .top-bar button {
      background-color: #00c86f;
      border: none;
      border-radius: 50%;
      color: white;
      font-size: 20px;
      width: 40px;
      height: 40px;
      cursor: pointer;
    }
    .cards {
      display: flex;
      gap: 20px;
      margin-top: 20px;
      flex-wrap: wrap;
    }
    .card {
      background-color: #202024;
      padding: 20px;
      border-radius: 10px;
      flex: 1;
      min-width: 250px;
      transition: 0.3s;
    }
    .card:hover {
      background-color: #2a2a2e;
      cursor: pointer;
      transform: scale(1.02);
    }
    .card h3 {
      margin: 0;
      font-size: 14px;
      color: #999;
    }
    .card p {
      font-size: 24px;
      font-weight: bold;
      margin: 5px 0;
    }
    .green { color: #00c86f; }
    .red { color: #ff4c4c; }
    .gray { color: #999; }

   .content {
      display: flex;
      gap: 20px;
      margin-top: 20px;
      flex-wrap: wrap;
    }
    .panel {
      background-color: #202024;
      padding: 20px;
      border-radius: 10px;
      flex: 1;
      min-width: 300px;
    }
    .panel ul {
      list-style: none;
      padding: 0;
    }
    .panel li {
      display: flex;
      justify-content: space-between;
      padding: 8px 0;
      border-bottom: 1px solid #333;
      transition: 0.2s;
    }
    .panel li:hover {
      background-color: #2a2a2e;
      cursor: pointer;
    }
    .panel li:first-child {
      background-color: #29292e;
      padding: 10px;
      border-radius: 5px;
      font-weight: bold;
    }

  .transactions {
      background-color: #202024;
      padding: 20px;
      border-radius: 10px;
      margin-top: 20px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
    }
    th, td {
      padding: 10px;
      border-bottom: 1px solid #333;
      text-align: left;
      font-size: 14px;
    }
    th {
      color: #999;
    }
    td.red {
      color: #ff4c4c;
    }
    tbody tr {
      transition: 0.2s;
    }
    tbody tr:hover {
      background-color: #2a2a2e;
      cursor: pointer;
    }

  canvas {
      max-width: 100%;
      background-color: #121214;
    }
  </style>
</head>
<body>

  <div class="top-bar">
    <div><span style="font-size: 24px;">🐷</span></div>
    <button>+</button>
  </div>

  <div class="cards">
    <div class="card">
      <h3>Entradas</h3>
      <p class="green">R$ 7.840,56</p>
      <p class="gray">Somada todas as entradas do período.</p>
    </div>
    <div class="card">
      <h3>Saídas</h3>
      <p class="red">R$ 1.580,45</p>
      <p class="gray">Somada todas as saídas do período.</p>
    </div>
    <div class="card">
      <h3>Balanço</h3>
      <p class="green">R$ 6.260,11</p>
      <p class="gray">Somada todas as entradas e saídas do período.</p>
    </div>
  </div>

  <div class="content">
    <div class="panel">
      <h3>Análise</h3>
      <canvas id="graficoCategorias" width="400" height="200"></canvas>
    </div>
    <div class="panel">
      <h3>Categorias</h3>
      <ul>
        <li>🍽️ Alimentação <span>10</span> <span>R$ 1.508,15</span></li>
        <li>🛒 Mercado <span>8</span> <span>R$ 508,90</span></li>
        <li>🍽️ Alimentação <span>10</span> <span>R$ 1.508,15</span></li>
        <li>🍽️ Alimentação <span>10</span> <span>R$ 1.508,15</span></li>
        <li>🍽️ Alimentação <span>10</span> <span>R$ 1.508,15</span></li>
      </ul>
    </div>
  </div>

  <div class="transactions">
    <h3>Transações</h3>
    <table>
      <thead>
        <tr>
          <th>Descrição</th>
          <th>Tipo</th>
          <th>Valor</th>
          <th>Banco</th>
          <th>Data</th>
          <th>Parcelas</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>🛒 Supermercado Big Master</td>
          <td>Crédito</td>
          <td class="red">R$ 896,00</td>
          <td>Nubank</td>
          <td>21/03/2024</td>
          <td>1/1</td>
        </tr>
        <tr>
          <td>🛒 Supermercado Big Master</td>
          <td>Crédito</td>
          <td class="red">R$ 896,00</td>
          <td>Nubank</td>
          <td>21/03/2024</td>
          <td>1/1</td>
        </tr>
      </tbody>
    </table>
  </div>

  <canvas id="graficoCategorias" width="400" height="200"></canvas>

<script>
  const ctx = document.getElementById('graficoCategorias').getContext('2d');
  new Chart(ctx, {
    type: 'bar',
    data: {
      labels: [
        'Alimentação 1',
        'Alimentação 2',
        'Alimentação 3',
        'Alimentação 4',
        'Mercado'
      ],
      datasets: [{
        label: 'Gastos por Item',
        data: [1508.15, 1508.15, 1508.15, 1508.15, 508.90],
        backgroundColor: [
          '#00c86f',
          '#00b45a',
          '#009f4a',
          '#008837',
          '#ff4c4c'
        ],
        borderRadius: 5,
        borderSkipped: false
      }]
    },
    options: {
      responsive: true,
      plugins: {
        legend: {
          labels: {
            color: '#fff'
          }
        },
        tooltip: {
          callbacks: {
            label: function(context) {
              return 'R$ ' + context.parsed.y.toFixed(2).replace('.', ',');
            }
          }
        }
      },
      scales: {
        x: {
          ticks: {
            color: '#fff'
          },
          grid: {
            color: '#333'
          }
        },
        y: {
          beginAtZero: true,
          ticks: {
            color: '#fff'
          },
          grid: {
            color: '#333'
          }
        }
      }
    }
  });
</script>

</body>
</html>


