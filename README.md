# calculator
Split bill calculator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>E7sbly - Split Bill Calculator</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      padding: 30px;
      font-family: 'Inter', sans-serif;
      background: linear-gradient(120deg, #a6c0fe, #f68084);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }

    .title {
      font-size: 2.2rem;
      color: white;
      margin-bottom: 10px;
      text-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
    }

    #currentTime {
      color: #f0f0f0;
      margin-bottom: 30px;
    }

    .container {
      background: rgba(255, 255, 255, 0.15);
      backdrop-filter: blur(15px);
      border: 1px solid rgba(255, 255, 255, 0.2);
      padding: 30px;
      border-radius: 20px;
      max-width: 400px;
      width: 100%;
      box-shadow: 0 12px 30px rgba(0,0,0,0.2);
      color: #fff;
      animation: fadeIn 1s ease-in-out;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    label {
      margin: 12px 0 4px;
      display: block;
      color: white;
    }

    input {
      width: 100%;
      padding: 10px;
      font-size: 16px;
      margin-bottom: 10px;
      border-radius: 10px;
      border: none;
      outline: none;
      background: rgba(255,255,255,0.2);
      color: #fff;
    }

    input::placeholder {
      color: rgba(255, 255, 255, 0.7);
    }

    .checkboxes {
      display: flex;
      flex-direction: column;
      gap: 16px;
      margin: 20px 0;
    }

    .toggle-container {
      display: flex;
      align-items: center;
      justify-content: space-between;
      background: rgba(255, 255, 255, 0.1);
      padding: 12px 16px;
      border-radius: 12px;
      color: #fff;
      font-size: 15px;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    .toggle-container:hover {
      background: rgba(255, 255, 255, 0.2);
    }

    .toggle-switch {
      position: relative;
      width: 50px;
      height: 24px;
    }

    .toggle-switch input {
      opacity: 0;
      width: 0;
      height: 0;
    }

    .slider {
      position: absolute;
      cursor: pointer;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: #ccc;
      transition: 0.4s;
      border-radius: 24px;
    }

    .slider:before {
      position: absolute;
      content: "";
      height: 20px;
      width: 20px;
      left: 2px;
      bottom: 2px;
      background-color: white;
      border-radius: 50%;
      transition: 0.4s;
    }

    .toggle-switch input:checked + .slider {
      background-color: #00bfff;
    }

    .toggle-switch input:checked + .slider:before {
      transform: translateX(26px);
    }

    button {
      width: 100%;
      padding: 12px;
      font-size: 18px;
      font-weight: bold;
      border: none;
      border-radius: 12px;
      background: linear-gradient(135deg, #00c6ff, #0072ff);
      color: white;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 6px 15px rgba(0, 114, 255, 0.3);
      margin-top: 10px;
    }

    button:hover {
      background: linear-gradient(135deg, #0072ff, #00c6ff);
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(0, 114, 255, 0.5);
    }

    .result {
      margin-top: 20px;
      font-weight: bold;
      white-space: pre-line;
      background: rgba(0,0,0,0.2);
      padding: 10px;
      border-radius: 10px;
      animation: fadeIn 0.7s ease;
    }
  </style>
</head>
<body>

  <h1 class="title">E7sbly Calculator</h1>
  <p id="currentTime">Loading time...</p>

  <div class="container">
    <h2>Split Bill</h2>

    <label>Total Bill ($)</label>
    <input type="number" id="totalBill" placeholder="Enter total amount" />

    <label>Number of People</label>
    <input type="number" id="peopleCount" placeholder="Enter number of people" />

    <div class="checkboxes">
      <label class="toggle-container">
        Include Service (14%)
        <div class="toggle-switch">
          <input type="checkbox" id="includeService" />
          <span class="slider"></span>
        </div>
      </label>

      <label class="toggle-container">
        Include Tax (12%)
        <div class="toggle-switch">
          <input type="checkbox" id="includeTax" />
          <span class="slider"></span>
        </div>
      </label>
    </div>

    <button onclick="calculate()">CALCULATE</button>

    <div class="result" id="resultArea"></div>
  </div>

  <script>
    function calculate() {
      const bill = parseFloat(document.getElementById("totalBill").value);
      const people = parseInt(document.getElementById("peopleCount").value);
      const includeService = document.getElementById("includeService").checked;
      const includeTax = document.getElementById("includeTax").checked;

      if (isNaN(bill) || isNaN(people) || people <= 0) {
        document.getElementById("resultArea").innerText = "Please enter valid numbers.";
        return;
      }

      let service = includeService ? bill * 0.14 : 0;
      let tax = includeTax ? bill * 0.12 : 0;
      let total = bill + service + tax;
      let perPerson = total / people;

      let result = "";
      if (includeService) result += `Service (14%): $${service.toFixed(2)}\n`;
      if (includeTax) result += `Tax (12%): $${tax.toFixed(2)}\n`;
      result += `Total Bill: $${total.toFixed(2)}\n`;
      result += `Each Pays: $${perPerson.toFixed(2)}`;

      document.getElementById("resultArea").innerText = result;
    }

    function updateTime() {
      const now = new Date();
      const timeString = now.toLocaleTimeString();
      document.getElementById("currentTime").innerText = "Current Time: " + timeString;
    }

    setInterval(updateTime, 1000);
    updateTime();
  </script>

</body>
</html>
