<!DOCTYPE html>
<html lang="no">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Logel AS – Tapt inntjening → Xref</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #111;
      color: #fff;
      margin: 0;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: #0f0;
      font-size: 1.8rem;
      margin-bottom: 30px;
    }

    .loss-block {
      background-color: #222;
      padding: 25px;
      border-radius: 0;
      box-shadow: 0 0 12px #0f0;
      max-width: 500px;
      margin: 0 auto 40px auto;
      text-align: center;
    }

    .loss-title {
      font-size: 1.4rem;
      font-weight: bold;
      color: #0f0;
      margin-bottom: 10px;
    }

    .loss-value {
      font-size: 2rem;
      font-weight: bold;
      color: #fff;
    }

    .firms-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 20px;
    }

    @media (min-width: 700px) {
      .firms-grid {
        grid-template-columns: 1fr 1fr;
      }
    }

    .firm-block {
      background-color: #222;
      padding: 20px;
      border-radius: 0;
      box-shadow: 0 0 10px #0f0;
    }

    .firm-title {
      font-size: 1.2rem;
      margin-bottom: 10px;
      color: #0f0;
    }

    .summary {
      font-size: 1.1rem;
      margin-top: 10px;
    }
  </style>
</head>
<body>

  <h1>Logel AS – Tapt inntjening → Xref</h1>

  <div class="loss-block">
    <div class="loss-title">Tapt inntjening hittil i år</div>
    <div class="loss-value" id="loss-value">kr 0</div>
    <div class="loss-title" style="margin-top: 20px;">Snitt av prosentsats</div>
    <div class="loss-value" id="average-value">0%</div>
  </div>

  <div class="firms-grid" id="firms-container"></div>

  <script>
    const firms = [
      "Lefdal Installasjon AS",
      "Elektro kontakten AS",
      "Smart elektro AS",
      "Alt installasjon AS",
      "Billkvam & Fagerhaug AS"
    ];

    const container = document.getElementById("firms-container");
    let total = 0;
    let percentSum = 0;
    let percentCount = 0;

    firms.forEach((firm, index) => {
      const saved = JSON.parse(localStorage.getItem(`firm${index}`)) || { amount: "", percent: "" };
      const amount = parseFloat(saved.amount) || 0;
      const percent = parseFloat(saved.percent) || 0;

      total += amount;

      if (!isNaN(percent) && saved.percent !== "") {
        percentSum += percent;
        percentCount++;
      }

      const block = document.createElement("div");
      block.className = "firm-block";

      block.innerHTML = `
        <div class="firm-title">${firm}</div>
        <div class="summary">Beløp: kr ${amount.toLocaleString('no-NO')}</div>
        <div class="summary">Prosent: ${saved.percent || 0}%</div>
      `;
      container.appendChild(block);
    });

    const average = percentCount > 0 ? (percentSum / percentCount).toFixed(1) : 0;

    document.getElementById("loss-value").textContent = `kr ${total.toLocaleString('no-NO')}`;
    document.getElementById("average-value").textContent = `${average}%`;
  </script>

</body>
</html>
