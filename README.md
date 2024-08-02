<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ABCD123</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
    }
    .input-section, .response-section {
      margin-bottom: 20px;
    }
    .error {
      color: red;
    }
  </style>
</head>
<body>
  <h1>ABCD123</h1>
  <div class="input-section">
    <textarea id="json-input" placeholder='Enter JSON' rows='4' cols='50'></textarea>
    <br>
    <button id="submit-btn">Submit</button>
  </div>
  <div class="response-section">
    <div>
      <label>
        <input type='checkbox' value='Alphabets' class="option-checkbox">
        Alphabets
      </label>
      <label>
        <input type='checkbox' value='Numbers' class="option-checkbox">
        Numbers
      </label>
      <label>
        <input type='checkbox' value='Highest Alphabet' class="option-checkbox">
        Highest Alphabet
      </label>
    </div>
    <div id="response-output"></div>
  </div>
  <script>
    const submitBtn = document.getElementById('submit-btn');
    const jsonInput = document.getElementById('json-input');
    const responseOutput = document.getElementById('response-output');
    const optionCheckboxes = document.querySelectorAll('.option-checkbox');

    submitBtn.addEventListener('click', async () => {
      const input = jsonInput.value;
      try {
        const jsonData = JSON.parse(input);
        const response = await fetch('https://your-heroku-app.herokuapp.com/bfhl', {  // Replace with your Heroku app URL
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(jsonData)
        });
        const data = await response.json();
        if (data.is_success) {
          displayResponse(data);
        } else {
          responseOutput.innerHTML = <p class="error">Error: ${data.error}</p>;
        }
      } catch (error) {
        responseOutput.innerHTML = <p class="error">Invalid JSON or server error</p>;
      }
    });

    const displayResponse = (data) => {
      const numbers = data.numbers;
      const alphabets = data.alphabets;
      const highestAlphabet = data.highest_alphabet;
      const selectedOptions = Array.from(optionCheckboxes)
        .filter(checkbox => checkbox.checked)
        .map(checkbox => checkbox.value);

      let filteredResponse = {};

      if (selectedOptions.includes('Numbers')) filteredResponse.numbers = numbers;
      if (selectedOptions.includes('Alphabets')) filteredResponse.alphabets = alphabets;
      if (selectedOptions.includes('Highest Alphabet')) filteredResponse.highest_alphabet = highestAlphabet;

      responseOutput.innerHTML = <pre>${JSON.stringify(filteredResponse, null, 2)}</pre>;
    };
  </script>
</body>
</html>
