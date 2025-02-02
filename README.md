<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NRC09 Neom Dining Management System</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html5-qrcode/2.3.8/html5-qrcode.min.js"></script>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background: #e0f7e9; /* Light green background for the whole page */
            color: #333;
        }
        .container {
            width: 80%;
            margin: 20px auto;
            background-color: #ffffff; /* White background for content */
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
            border: 1px solid #ddd;
        }
        .header {
            background-color: #e6f7ff; /* Light cyan background for the header */
            text-align: center;
            padding: 20px;
            border-radius: 8px;
            margin-bottom: 20px;
        }
        .title {
            font-size: 38px;
            font-weight: bold;
            color: #ff6347; /* Light coral color */
        }
        input, select, button {
            width: 100%;
            padding: 12px;
            margin: 12px 0;
            border-radius: 8px;
            border: 1px solid #ddd;
            box-sizing: border-box;
            font-size: 16px;
        }
        input, select {
            background-color: #f0f8ff; /* Light blue background for inputs */
        }
        button {
            background-color: #ff6347; /* Light coral color for buttons */
            color: white;
            border: none;
            cursor: pointer;
            font-weight: bold;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #e5533c; /* Darker coral color on hover */
        }
        table {
            width: 100%;
            margin-top: 30px;
            border-collapse: collapse;
        }
        table th, table td {
            padding: 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        table th {
            background-color: #ff6347; /* Light coral header */
            color: white;
        }
        .export-btn {
            background-color: #32cd32; /* Light green button */
            color: white;
            padding: 12px 20px;
            margin-top: 20px;
            font-size: 16px;
            cursor: pointer;
            border-radius: 8px;
        }
        .status {
            font-size: 14px;
            color: #555;
            padding: 10px;
            background-color: #f0f8ff; /* Light blue background for status */
            border-radius: 5px;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="title">NRC09 Neom Dining Management System</div>
        </div>
        <h2>Dining Management System</h2>
        <h3>Scan QR Code or Card</h3>
        <input type="text" id="scanInput" placeholder="Scan QR Code or Card" autofocus>
        <div id="status" class="status">Scanned Code: None</div>
        <h3>Log Dining Entry</h3>
        <form onsubmit="addToTable(event)">
            <input type="text" id="name" placeholder="Name" required>
            <input type="text" id="company" placeholder="Company" required>
            <input type="text" id="category" placeholder="Category" required>
            <input type="text" id="neom_id" placeholder="Neom ID" required>
            <input type="text" id="emp_id" placeholder="Emp ID" required>
            <select id="visitor_type" required>
                <option value="Visitor">Visitor</option>
                <option value="Resident">Resident</option>
            </select>
            <select id="meal_type" required>
                <option>Breakfast</option>
                <option>Lunch</option>
                <option>Dinner</option>
            </select>
            <button type="submit">Log Entry</button>
        </form>
        <table>
            <thead>
                <tr>
                    <th>Sr No</th>
                    <th>Name</th>
                    <th>Company</th>
                    <th>Category</th>
                    <th>Neom ID</th>
                    <th>Emp ID</th>
                    <th>Visitor Type</th>
                    <th>Meal Type</th>
                    <th>Date</th>
                </tr>
            </thead>
            <tbody id="entryTableBody"></tbody>
        </table>
        <button class="export-btn" onclick="exportToCSV()">Export to CSV</button>
    </div>
    <script>
        document.getElementById('scanInput').addEventListener('input', function (event) {
            let scannedData = event.target.value;
            document.getElementById('status').textContent = `Scanned Code: ${scannedData}`;
            let data = scannedData.split(',');
            if (data.length >= 5) {
                document.getElementById('name').value = data[0] || '';
                document.getElementById('company').value = data[1] || '';
                document.getElementById('category').value = data[2] || '';
                document.getElementById('neom_id').value = data[3] || '';
                document.getElementById('emp_id').value = data[4] || '';
            }
        });
        function addToTable(event) {
            event.preventDefault();
            let tableBody = document.getElementById('entryTableBody');
            let rowCount = tableBody.rows.length + 1;
            let name = document.getElementById('name').value;
            let company = document.getElementById('company').value;
            let category = document.getElementById('category').value;
            let neomId = document.getElementById('neom_id').value;
            let empId = document.getElementById('emp_id').value;
            let visitorType = document.getElementById('visitor_type').value;
            let mealType = document.getElementById('meal_type').value;
            let date = new Date().toLocaleDateString();
            let row = `<tr>
                        <td>${rowCount}</td>
                        <td>${name}</td>
                        <td>${company}</td>
                        <td>${category}</td>
                        <td>${neomId}</td>
                        <td>${empId}</td>
                        <td>${visitorType}</td>
                        <td>${mealType}</td>
                        <td>${date}</td>
                      </tr>`;
            tableBody.innerHTML += row;
        }
        function exportToCSV() {
            let table = document.querySelector("table");
            let rows = table.querySelectorAll("tr");
            let csvContent = "";
            rows.forEach(row => {
                let cells = row.querySelectorAll("td, th");
                let rowContent = [];
                cells.forEach(cell => rowContent.push(cell.textContent.trim()));
                csvContent += rowContent.join(",") + "\n";
            });
            let link = document.createElement('a');
            link.href = 'data:text/csv;charset=utf-8,' + encodeURIComponent(csvContent);
            link.download = 'dining_log.csv';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }
    </script>
</body>
</html>

