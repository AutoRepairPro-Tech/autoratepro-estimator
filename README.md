<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Estimate Calculator</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 2rem;
      background-color: #1e1e2f;
      color: #f4f4f4;
    }
    .container {
      background-color: #2e2e3e;
      padding: 2rem;
      max-width: 700px;
      margin: auto;
      border-radius: 12px;
      box-shadow: 0 0 15px rgba(0,0,0,0.4);
    }
    h2 {
      text-align: center;
      color: #00c896;
    }
    .logo-text {
      text-align: center;
      font-size: 0.9rem;
      color: #bbb;
      margin-top: -0.5rem;
      margin-bottom: 1rem;
    }
    label {
      display: block;
      margin-top: 1rem;
      color: #f4f4f4;
    }
    input, select, textarea {
      width: 100%;
      padding: 0.5rem;
      margin-top: 0.25rem;
      border: 1px solid #555;
      border-radius: 4px;
      background-color: #444;
      color: #fff;
    }
    .output {
      margin-top: 2rem;
      padding: 1rem;
      background-color: #3e3e4f;
      border-radius: 4px;
    }
    .button-group {
      margin-top: 1.5rem;
      text-align: center;
    }
    button {
      background-color: #00c896;
      border: none;
      color: white;
      padding: 0.5rem 1rem;
      margin: 0.5rem;
      font-size: 1rem;
      border-radius: 4px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Estimate Calculator</h2>
    <p style="text-align:center; font-style:italic; color:#ccc; margin-top:0.25rem;">Your shop. Your rates. Your parts. Our tool builds the perfect estimate.</p>
    <div class="logo-text">Powered by AutoRate Pro</div>
    <form id="pricingForm">
      <label>Job Name/Type:
        <input type="text" id="jobName" />
      </label>
      <label>Job Difficulty:
        <select id="difficulty">
          <option value="easy">Easy</option>
          <option value="medium">Medium</option>
          <option value="hard">Hard</option>
        </select>
      </label>
      <label>Labor Hours:
        <input type="number" id="laborHours" min="0" step="0.1" />
      </label>
      <label>Hourly Rate (based on difficulty):
        <input type="number" id="hourlyRate" min="0" step="1" />
      </label>
      <label>Part Cost (Total for all parts):
        <input type="number" id="partsCost" min="0" step="0.01" />
      </label>
      <label>Customer Notes:
        <textarea id="notes" rows="3"></textarea>
      </label>
    </form>
    <div class="button-group">
      <button onclick="resetForm()">Reset</button>
      <button onclick="downloadQuote()">Download Quote</button>
    </div>
    <div id="output" class="output"></div>
  </div>

  <script>
    const inputs = document.querySelectorAll('#pricingForm input, #pricingForm select, #pricingForm textarea');

    function updateEstimate() {
      const jobName = document.getElementById('jobName').value.trim();
      const difficulty = document.getElementById('difficulty').value;
      const laborHours = parseFloat(document.getElementById('laborHours').value) || 0;
      const hourlyRate = parseFloat(document.getElementById('hourlyRate').value) || 0;
      const partsCost = parseFloat(document.getElementById('partsCost').value) || 0;
      const notes = document.getElementById('notes').value.trim();

      const laborCost = laborHours * hourlyRate;
      const totalEstimate = partsCost + laborCost;

      document.getElementById('output').innerHTML = `
        <h3 style="color:#00c896">Estimate Summary</h3>
        <p><strong>Job Name:</strong> ${jobName}</p>
        <p><strong>Difficulty:</strong> ${difficulty}</p>
        <p><strong>Labor Hours:</strong> ${laborHours} at $${hourlyRate}/hr</p>
        <p><strong>Labor Cost:</strong> $${laborCost.toFixed(2)}</p>
        <p><strong>Parts Cost:</strong> $${partsCost.toFixed(2)}</p>
        <hr>
        <p style="font-size:1.25rem;"><strong><u>Estimated Total:</u> $${totalEstimate.toFixed(2)}</strong></p>
        ${notes ? `<p><strong>Notes:</strong> ${notes}</p>` : ''}
      `;
    }

    function resetForm() {
      document.getElementById('pricingForm').reset();
      updateEstimate();
    }

    function downloadQuote() {
      const output = document.getElementById('output').innerText;
      const blob = new Blob([output], { type: 'text/plain' });
      const link = document.createElement('a');
      link.download = 'Estimate.txt';
      link.href = window.URL.createObjectURL(blob);
      link.click();
    }

    inputs.forEach(input => {
      input.addEventListener('input', updateEstimate);
    });

    updateEstimate();
  </script>
</body>
</html>
