<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Used Car Business Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
        }
        header {
            background-color: #333;
            color: white;
            padding: 15px 0;
            text-align: center;
        }
        section {
            padding: 20px;
            margin: 20px;
        }
        table {
            width: 100%;
            margin: 15px 0;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #333;
            color: white;
        }
        input, select {
            padding: 10px;
            margin: 5px;
            width: 100%;
            box-sizing: border-box;
        }
        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>

<header>
    <h1>Used Car Business Tracker</h1>
</header>

<section>
    <h2>Car Inventory</h2>
    <table id="inventoryTable">
        <thead>
            <tr>
                <th>Car ID</th>
                <th>Make</th>
                <th>Model</th>
                <th>Year</th>
                <th>VIN</th>
                <th>Purchase Price</th>
                <th>Acquisition Date</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <!-- Inventory items will be added here -->
        </tbody>
    </table>

    <h3>Add New Car</h3>
    <form id="carForm">
        <input type="text" id="make" placeholder="Make" required>
        <input type="text" id="model" placeholder="Model" required>
        <input type="number" id="year" placeholder="Year" required>
        <input type="text" id="vin" placeholder="VIN" required>
        <input type="number" id="purchasePrice" placeholder="Purchase Price" required>
        <input type="date" id="acquisitionDate" required>
        <button type="submit">Add Car</button>
    </form>
</section>

<section>
    <h2>Repairs & Costs</h2>
    <table id="repairsTable">
        <thead>
            <tr>
                <th>Car ID</th>
                <th>Spare Parts Cost</th>
                <th>Labor Cost</th>
                <th>Outsourcing Repairs</th>
                <th>Government Fees</th>
                <th>Total Repair Cost</th>
            </tr>
        </thead>
        <tbody>
            <!-- Repair costs will be added here -->
        </tbody>
    </table>

    <h3>Add Repair Costs</h3>
    <form id="repairForm">
        <input type="number" id="carIdForRepair" placeholder="Car ID" required>
        <input type="number" id="sparePartsCost" placeholder="Spare Parts Cost" required>
        <input type="number" id="laborCost" placeholder="Labor Cost" required>
        <input type="number" id="outsourcingRepairs" placeholder="Outsourcing Repairs" required>
        <input type="number" id="governmentFees" placeholder="Government Fees" required>
        <button type="submit">Add Repair Costs</button>
    </form>
</section>

<section>
    <h2>Sales & ROI</h2>
    <table id="salesTable">
        <thead>
            <tr>
                <th>Car ID</th>
                <th>Selling Price</th>
                <th>Date of Sale</th>
                <th>ROI</th>
            </tr>
        </thead>
        <tbody>
            <!-- Sales and ROI will be added here -->
        </tbody>
    </table>

    <h3>Add Sale</h3>
    <form id="salesForm">
        <input type="number" id="carIdForSale" placeholder="Car ID" required>
        <input type="number" id="sellingPrice" placeholder="Selling Price" required>
        <input type="date" id="dateOfSale" required>
        <button type="submit">Add Sale</button>
    </form>
</section>

<script>
    const carInventory = [];
    const repairCosts = [];
    const salesData = [];

    // Add car to inventory
    document.getElementById('carForm').addEventListener('submit', function(event) {
        event.preventDefault();
        
        const newCar = {
            id: carInventory.length + 1,
            make: document.getElementById('make').value,
            model: document.getElementById('model').value,
            year: document.getElementById('year').value,
            vin: document.getElementById('vin').value,
            purchasePrice: parseFloat(document.getElementById('purchasePrice').value),
            acquisitionDate: document.getElementById('acquisitionDate').value,
        };

        carInventory.push(newCar);
        renderInventory();
    });

    // Render inventory
    function renderInventory() {
        const inventoryTableBody = document.getElementById('inventoryTable').getElementsByTagName('tbody')[0];
        inventoryTableBody.innerHTML = '';
        
        carInventory.forEach(car => {
            const row = inventoryTableBody.insertRow();
            row.innerHTML = `
                <td>${car.id}</td>
                <td>${car.make}</td>
                <td>${car.model}</td>
                <td>${car.year}</td>
                <td>${car.vin}</td>
                <td>${car.purchasePrice}</td>
                <td>${car.acquisitionDate}</td>
                <td><button onclick="deleteCar(${car.id})">Delete</button></td>
            `;
        });
    }

    // Delete car from inventory
    function deleteCar(carId) {
        const index = carInventory.findIndex(car => car.id === carId);
        if (index !== -1) {
            carInventory.splice(index, 1);
            renderInventory();
        }
    }

    // Add repair costs
    document.getElementById('repairForm').addEventListener('submit', function(event) {
        event.preventDefault();

        const newRepair = {
            carId: parseInt(document.getElementById('carIdForRepair').value),
            sparePartsCost: parseFloat(document.getElementById('sparePartsCost').value),
            laborCost: parseFloat(document.getElementById('laborCost').value),
            outsourcingRepairs: parseFloat(document.getElementById('outsourcingRepairs').value),
            governmentFees: parseFloat(document.getElementById('governmentFees').value),
        };

        repairCosts.push(newRepair);
        renderRepairs();
    });

    // Render repair costs
    function renderRepairs() {
        const repairsTableBody = document.getElementById('repairsTable').getElementsByTagName('tbody')[0];
        repairsTableBody.innerHTML = '';
        
        repairCosts.forEach(repair => {
            const totalCost = repair.sparePartsCost + repair.laborCost + repair.outsourcingRepairs + repair.governmentFees;
            const row = repairsTableBody.insertRow();
            row.innerHTML = `
                <td>${repair.carId}</td>
                <td>${repair.sparePartsCost}</td>
                <td>${repair.laborCost}</td>
                <td>${repair.outsourcingRepairs}</td>
                <td>${repair.governmentFees}</td>
                <td>${totalCost}</td>
            `;
        });
    }

    // Add sale and calculate ROI
    document.getElementById('salesForm').addEventListener('submit', function(event) {
        event.preventDefault();

        const carId = parseInt(document.getElementById('carIdForSale').value);
        const sellingPrice = parseFloat(document.getElementById('sellingPrice').value);
        const dateOfSale = document.getElementById('dateOfSale').value;

        // Find the corresponding car and repair costs
        const car = carInventory.find(car => car.id === carId);
        const repairs = repairCosts.filter(repair => repair.carId === carId);
        const totalRepairCost = repairs.reduce((sum, repair) => sum + repair.sparePartsCost + repair.laborCost + repair.outsourcingRepairs + repair.governmentFees, 0);

        // Calculate ROI
        const totalCost = car.purchasePrice + totalRepairCost;
        const roi = ((sellingPrice - totalCost) / totalCost) * 100;

        salesData.push({ carId, sellingPrice, dateOfSale, roi });
        renderSales();
    });

    // Render sales and ROI
    function renderSales() {
        const salesTableBody = document.getElementById('salesTable').getElementsByTagName('tbody')[0];
        salesTableBody.innerHTML = '';
        
        salesData.forEach(sale => {
            const row = salesTableBody.insertRow();
            row.innerHTML = `
                <td>${sale.carId}</td>
                <td>${sale.sellingPrice}</td>
                <td>${sale.dateOfSale}</td>
                <td>${sale.roi.toFixed(2)}%</td>
            `;
        });
    }

</script>

</body>
</html>
