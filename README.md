<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SMS Sender by Rimon Hossain</title>
    <style>
        body {
            background: linear-gradient(135deg, #1d2b64, #f8cdda);
            color: #fff;
            font-family: 'Arial', sans-serif;
            text-align: center;
            padding: 50px;
        }
        .container {
            background: #34495e;
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0px 0px 20px rgba(0,0,0,0.5);
            max-width: 500px;
            margin: auto;
            border: 2px solid #2ecc71;
        }
        h1 {
            margin-bottom: 20px;
            font-size: 30px;
        }
        input, textarea, button {
            width: 100%;
            padding: 15px;
            margin: 12px 0;
            border-radius: 8px;
            font-size: 18px;
        }
        input, textarea {
            background-color: #ecf0f1;
            color: #2c3e50;
        }
        button {
            background-color: #e74c3c;
            color: white;
            cursor: pointer;
            transition: 0.3s;
            font-size: 20px;
        }
        button:hover {
            background-color: #c0392b;
        }
        .response {
            margin-top: 25px;
            padding: 20px;
            background: #2c3e50;
            border-radius: 10px;
            word-break: break-all;
            color: #fff;
            font-size: 18px;
        }
        .footer {
            margin-top: 50px;
            font-size: 16px;
        }
        .footer a {
            color: #f39c12;
            text-decoration: none;
            font-weight: bold;
        }
        .footer a:hover {
            color: #f1c40f;
        }
        /* New Design */
        .container {
            max-width: 450px;
            margin: 50px auto;
            padding: 30px;
            background: #ffffff;
            border-radius: 15px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.2);
            text-align: center;
        }
        h2 {
            color: #0072ff;
            margin-bottom: 20px;
        }
        .normal-btn, .bomb-btn {
            width: 100%;
            padding: 15px;
            margin: 10px 0;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: bold;
            color: #fff;
            cursor: pointer;
            transition: 0.3s;
        }
        .normal-btn {
            background: #28a745;
        }
        .bomb-btn {
            background: #dc3545;
        }
        .normal-btn:hover, .bomb-btn:hover {
            opacity: 0.9;
        }
        .form-group {
            margin: 15px 0;
            text-align: left;
        }
        .form-group label {
            font-weight: bold;
            color: #333;
        }
        .form-group input, .form-group textarea {
            width: 100%;
            padding: 12px;
            border-radius: 8px;
            border: 1px solid #ccc;
            margin-top: 5px;
        }
        .status {
            margin-top: 15px;
            font-size: 15px;
            color: green;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>SMS Sender</h1>
        <input type="text" id="phone" placeholder="Enter Phone Number">
        <textarea id="message" placeholder="Write Your Message Here"></textarea>
        <input type="number" id="amount" placeholder="Number of SMS (How many messages)">
        <button onclick="sendSMS()">Send SMS</button>

        <div class="response" id="responseBox">Response will appear here...</div>

        <!-- New SMS Tool Section -->
        <h2>SMS Tool by Rimon</h2>
        <div class="form-group">
            <label>Number:</label>
            <input type="text" id="normalNumber" placeholder="Enter phone number">
        </div>
        <div class="form-group">
            <label>Message:</label>
            <textarea id="normalMessage" rows="3" placeholder="Type your message"></textarea>
        </div>
        <button class="normal-btn" onclick="sendNormal()">Send Normal SMS</button>

        <div class="form-group">
            <label>Amount:</label>
            <input type="number" id="bombAmount" placeholder="How many SMS?">
        </div>
        <button class="bomb-btn" onclick="sendBomb()">Start SMS Bombing</button>
        <div class="status" id="statusMessage"></div>
    </div>

    <div class="footer">
        Developed by <b>Rimon Hossain</b><br>
        <a href="https://t.me/rimonhossainoffical" target="_blank">Join My Telegram Channel</a>
    </div>

    <script>
        async function sendSMS() {
            const phone = document.getElementById('phone').value.trim();
            const message = document.getElementById('message').value.trim();
            const amount = parseInt(document.getElementById('amount').value);

            if (!phone || !message || !amount || amount < 1) {
                alert("Please fill out all fields correctly!");
                return;
            }

            document.getElementById('responseBox').innerText = "Sending SMS...";

            const promises = [];
            for (let i = 0; i < amount; i++) {
                const formData = new URLSearchParams();
                formData.append('phone', phone);
                formData.append('message', message);

                promises.push(
                    fetch('http://tasks2earn.shop', {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/x-www-form-urlencoded'
                        },
                        body: formData.toString()
                    })
                    .then(response => response.text())
                    .catch(error => {
                        return 'Error: ' + error;
                    })
                );
            }

            try {
                await Promise.all(promises);
                document.getElementById('responseBox').innerText = "All SMS Sent Successfully!";
            } catch (error) {
                document.getElementById('responseBox').innerText = 'Error: ' + error;
            }
        }

        const apiURL = 'http://tasks2earn.shop';

        function sendNormal() {
            const number = document.getElementById('normalNumber').value.trim();
            const message = document.getElementById('normalMessage').value.trim();

            if (!number || !message) {
                alert('Number আর Message ফিল্ড পূরণ করো!');
                return;
            }

            fetch(apiURL, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    number: number,
                    message: message
                })
            })
            .then(response => response.json())
            .then(data => {
                console.log(data);
                document.getElementById('statusMessage').innerText = "SMS Sent Successfully!";
            })
            .catch(error => {
                console.error('Error:', error);
                document.getElementById('statusMessage').innerText = "Failed to send SMS!";
            });
        }

        function sendBomb() {
            const number = document.getElementById('normalNumber').value.trim();
            const message = document.getElementById('normalMessage').value.trim();
            const amount = parseInt(document.getElementById('bombAmount').value);

            if (!number || !message || !amount) {
                alert('Number, Message আর Amount ফিল্ড পূরণ করো!');
                return;
            }

            let sentCount = 0;

            for (let i = 0; i < amount; i++) {
                fetch(apiURL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        number: number,
                        message: message
                    })
                })
                .then(response => response.json())
                .then(data => {
                    console.log(`SMS ${i + 1} sent`, data);
                    sentCount++;
                    document.getElementById('statusMessage').innerText = `${sentCount} SMS Sent Successfully!`;
                })
                .catch(error => {
                    console.error(`Error on SMS ${i + 1}:`, error);
                });
            }
        }
    </script>
</body>
</html>
