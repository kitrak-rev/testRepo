<!-- src/main/resources/static/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Submit Data</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        form {
            max-width: 600px;
            margin: auto;
        }
        label, input, textarea {
            display: block;
            margin-bottom: 10px;
        }
        textarea {
            width: 100%;
            height: 100px;
        }
        button {
            padding: 10px 20px;
        }
        #response {
            margin-top: 20px;
            font-size: 1.2em;
        }
    </style>
</head>
<body>
    <h1>Submit Data</h1>
    <form id="dataForm">
        <label for="domain">Domain:</label>
        <input type="text" id="domain" name="domain" required>
        
        <label for="subdomain">Subdomain:</label>
        <input type="text" id="subdomain" name="subdomain" required>
        
        <label for="language">Language:</label>
        <input type="text" id="language" name="language" required>
        
        <label for="version">Version:</label>
        <input type="text" id="version" name="version" required>
        
        <label for="attributes">Attributes (JSON format):</label>
        <textarea id="attributes" name="attributes"></textarea>

        <!-- Choose one of the following sections -->

        <!-- Text Area for Value -->
        <label for="value">Value:</label>
        <textarea id="value" name="value"></textarea>

        <!-- OR File Input for Value -->
        <!--
        <label for="file">Upload File:</label>
        <input type="file" id="file" name="file">
        -->

        <button type="submit">Submit Data</button>
    </form>
    <div id="response"></div>

    <script>
        document.getElementById('dataForm').addEventListener('submit', function(event) {
            event.preventDefault();

            const formData = {
                domain: document.getElementById('domain').value,
                subdomain: document.getElementById('subdomain').value,
                language: document.getElementById('language').value,
                version: document.getElementById('version').value,
                attributes: JSON.parse(document.getElementById('attributes').value || '{}')
            };

            const valueText = document.getElementById('value').value;
            formData.value = valueText; // Use this if you're using the text area for value

            // For file upload
            /*
            const fileInput = document.getElementById('file');
            if (fileInput.files.length > 0) {
                const file = fileInput.files[0];
                const reader = new FileReader();
                reader.onload = function(event) {
                    formData.value = event.target.result;
                    sendData(formData);
                };
                reader.readAsText(file);
            } else {
                sendData(formData);
            }
            */

            function sendData(data) {
                fetch('/api/submitData', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(data)
                })
                .then(response => response.text())
                .then(data => {
                    document.getElementById('response').innerText = data;
                })
                .catch(error => {
                    console.error('Error submitting data:', error);
                });
            }

            sendData(formData);
        });
    </script>
</body>
</html>
