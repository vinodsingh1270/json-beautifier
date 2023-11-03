<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JSON Beautifier</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 0;
        background-color: #f0f0f0;
      }

      .header {
        background-color: #333;
        color: #fff;
        text-align: center;
        padding: 0px;
        position: fixed;
        top: 0;
        width: 100%;
      }

      .container {
        max-width: 100%;
        margin: 0 auto;
        padding: 50px;
        background-color: #fff;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
        border-radius: 5px;
        margin-top: 31px;
      }

      /* To push content below the fixed header */
      .json-output {
        width: 100%;
        min-height: 250px;
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        margin-top: 10px;
        white-space: pre-wrap;
      }

      .search-box {
        margin-top: 20px;
      }

      .footer {
        position: fixed;
        bottom: 0;
        right: 0;
        background-color: #333;
        color: #fff;
        padding: 10px;
        width: 100%;
        text-align: right;
      }

      .highlighted {
        background-color: yellow;
        font-weight: bold;
      }

      /* Add these styles to your existing CSS */
      .button {
        padding: 3px 10px;
        font-size: 16px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        margin: 5px;
        transition: background-color 0.3s;
      }

      .primary-button {
        background-color: #0074d9;
        color: #fff;
      }

      .secondary-button {
        background-color: #ccc;
        color: #000;
      }

      .success-button {
        background-color: #2ecc40;
        color: #fff;
      }

      .button:hover {
        background-color: #0056a4;
      }

      .json-input {
        width: 100%;
        height: 200px;
        padding: 10px;
        font-family: monospace;
        border: 1px solid #ccc;
        border-radius: 5px;
        font-size: 14px;
      }

      .json-input:focus {
        border-color: #0074d9;
        /* Change the border color when the input is focused */
        outline: none;
        /* Remove the default outline on focus */
        box-shadow: 0 0 5px rgba(0, 116, 217, 0.5);
        /* Add a subtle shadow when focused */
      }

      .json-input::placeholder {
        color: #888;
        /* Customize the placeholder text color */
      }

      .search-box {
        margin-top: 20px;
      }

      #search-word {
        width: 100%;
        padding: 5px;
        font-family: monospace;
        border: 1px solid #ccc;
        border-radius: 5px;
        font-size: 14px;
      }

      #search-word:focus {
        border-color: #0074d9;
        /* Change the border color when the input is focused */
        outline: none;
        /* Remove the default outline on focus */
        box-shadow: 0 0 5px rgba(0, 116, 217, 0.5);
        /* Add a subtle shadow when focused */
      }
    </style>
  </head>
  <body>
    <div class="header">
      <h3>JSON Beautifier</h3>
    </div>
    <div class="container">
      <!-- <label for="json-input">Enter JSON:</label> or  -->
      <button onclick="pasteJson()" style="margin-bottom:5px;" class="button secondary-button">Paste</button>
      <textarea id="json-input" class="json-input" placeholder="Paste or Enter your JSON here..."></textarea>
      <button onclick="beautifyJson()" class="button primary-button">Beautify JSON</button>
      <button onclick="clearInput()" class="button secondary-button">Clear</button>
      <div class="search-box">
        <!-- <label for="search-word">Search Word:</label> -->
        <input type="text" id="search-word" placeholder="Enter word to search" oninput="searchJson()">
        <span id="match-count" class="match-count"></span>
        <!-- <button class="button success-button">Search</button> -->
      </div>
      <div class="json-output" id="json-output"></div>
    </div>
    <div class="footer"> &copy; 2023 Developed By Vinod Singh </div>
    <script>
      function beautifyJson() {
        const input = document.getElementById('json-input').value;
        const output = document.getElementById('json-output');
        try {
          const jsonObject = JSON.parse(input);
          output.innerHTML = `
						<pre>${JSON.stringify(jsonObject, null, 2)}</pre>`;
        } catch (error) {
          output.textContent = 'Invalid JSON';
        }
      }

      function clearInput() {
        const inputField = document.getElementById('json-input');
        inputField.value = '';
      } < !-- function searchJson() {
        -- > < !--
        const searchTerm = document.getElementById('search-word').value;
        -- > < !--
        const jsonOutput = document.getElementById('json-output');
        -- > < !--
        const text = jsonOutput.textContent;
        -- > < !--
        if (searchTerm) {
          -- > < !--
          const regex = new RegExp(searchTerm, 'g');
          -- > < !--
          const highlightedText = text.replace(regex, '<span class="highlighted">$&</span>');
          -- > < !--jsonOutput.innerHTML = highlightedText;
          -- > < !--
        }-- > < !--
      }-- > function pasteJson() {
        const inputField = document.getElementById('json-input');
        navigator.clipboard.readText().then((clipboardText) => {
          inputField.value = clipboardText;
        }).catch((error) => {
          console.error('Failed to read clipboard text: ', error);
        });
      }

      function searchJson() {
        const searchTerm = document.getElementById('search-word').value.toLowerCase();
        const jsonOutput = document.getElementById('json-output');
        const originalText = jsonOutput.textContent;
        if (searchTerm) {
          const regex = new RegExp(searchTerm, 'gi');
          const matches = originalText.match(regex);
          if (matches) {
            const matchCount = matches.length;
            const matchCountElement = document.getElementById('match-count');
            matchCountElement.textContent = `Match count: ${matchCount}`;
            const highlightedText = originalText.replace(regex, match => `
						<span class="highlighted">${match}</span>`);
            jsonOutput.innerHTML = highlightedText;
          } else {
            // If there are no matches, display a message
            const matchCountElement = document.getElementById('match-count');
            matchCountElement.textContent = 'No matches found';
            jsonOutput.textContent = originalText;
          }
        } else {
          // If the search term is empty, reset the JSON output and match count
          const matchCountElement = document.getElementById('match-count');
          matchCountElement.textContent = '';
          jsonOutput.textContent = originalText;
        }
      }
    </script>
  </body>
</html>
