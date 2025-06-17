<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Kết quả khảo sát NIVEA</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body { font-family: Arial; padding: 20px; background: #f0f0f0; }
    canvas { background: #fff; border-radius: 12px; padding: 20px; }
    h1 { color: #005aaa; }
  </style>
</head>
<body>
  <h1>Kết quả khảo sát sản phẩm NIVEA</h1>
  <canvas id="chart" width="600" height="400"></canvas>

  <script>
    const sheetURL = 'https://docs.google.com/spreadsheets/d/1u_IIVX85xrPKdkglqJ95aPPiUqqGmu8dTdO_elHvCjs/gviz/tq?tqx=out:json';

    fetch(sheetURL)
      .then(res => res.text())
      .then(text => {
        const json = JSON.parse(text.substr(47).slice(0, -2));
        const rows = json.table.rows;
        const counts = {};

        rows.forEach(row => {
          const product = row.c[1]?.v;
          if (product) counts[product] = (counts[product] || 0) + 1;
        });

        const labels = Object.keys(counts);
        const values = Object.values(counts);

        const total = values.reduce((a, b) => a + b, 0);
        const percentages = values.map(val => ((val / total) * 100).toFixed(1));

        new Chart(document.getElementById("chart"), {
          type: 'bar',
          data: {
            labels: labels,
            datasets: [{
              label: "% người chọn",
              data: percentages,
              backgroundColor: '#007bff'
            }]
          },
          options: {
            scales: {
              y: {
                beginAtZero: true,
                max: 100,
                title: {
                  display: true,
                  text: "Phần trăm (%)"
                }
              }
            }
          }
        });
      });
  </script>
</body>
</html>
