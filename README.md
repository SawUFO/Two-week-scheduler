<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2-Week Scheduler</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        .container {
            max-width: 1200px;
            margin: 20px auto;
            padding: 20px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
        }
        form {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            justify-content: center;
        }
        input, button {
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        button {
            background-color: #007BFF;
            color: white;
            font-weight: bold;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(10, 1fr);
            gap: 5px;
        }
        .cell {
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 10px;
            text-align: center;
            background-color: #fff;
            position: relative;
        }
        .cell.completed {
            background-color: #d3d3d3;
            text-decoration: line-through;
        }
        .cell .mark-complete {
            position: absolute;
            top: 5px;
            right: 5px;
            background: green;
            color: white;
            border: none;
            border-radius: 50%;
            font-size: 12px;
            cursor: pointer;
        }
        .archive-btn {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }
        .archive-btn:hover {
            background-color: #218838;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>2-Week Scheduler</h1>
        <form id="projectForm">
            <input type="text" id="projectName" placeholder="Project Name" required>
            <input type="number" id="units" placeholder="Number of Units" min="1" required>
            <button type="submit">Add Project</button>
        </form>
        <div class="grid" id="grid"></div>
        <button class="archive-btn" id="archiveButton">Archive Schedule</button>
    </div>
    <script>
        const form = document.getElementById("projectForm");
        const grid = document.getElementById("grid");
        const archiveButton = document.getElementById("archiveButton");
        let currentSchedule = [];

        // Initialize grid
        function initializeGrid() {
            grid.innerHTML = "";
            for (let i = 0; i < 150; i++) { // 150 slots (10 days x 15 slots)
                const cell = document.createElement("div");
                cell.classList.add("cell");
                cell.dataset.index = i;
                cell.innerHTML = "";
                grid.appendChild(cell);
            }
        }

        // Add project to grid
        form.addEventListener("submit", (e) => {
            e.preventDefault();
            const projectName = document.getElementById("projectName").value;
            const units = parseInt(document.getElementById("units").value);
            addProjectToGrid(projectName, units);
            form.reset();
        });

        function addProjectToGrid(name, units) {
            let addedUnits = 0;
            for (const cell of grid.children) {
                if (!cell.innerHTML && addedUnits < units) {
                    cell.innerHTML = `${name} <button class="mark-complete" onclick="markComplete(this)">✓</button>`;
                    cell.dataset.project = name;
                    addedUnits++;
                }
            }
            if (addedUnits < units) {
                alert("Not enough space to add the entire project!");
            }
        }

        // Mark a unit as complete
        function markComplete(button) {
            const cell = button.parentElement;
            cell.classList.add("completed");
            cell.innerHTML = cell.dataset.project; // Remove the button
        }

        // Archive the schedule
        archiveButton.addEventListener("click", () => {
            const archiveData = [];
            for (const cell of grid.children) {
                archiveData.push({
                    project: cell.dataset.project || null,
                    completed: cell.classList.contains("completed"),
                });
            }
            currentSchedule = archiveData;
            alert("Schedule archived! You can now start a new schedule.");
            initializeGrid();
        });

        // Initialize the grid on page load
        initializeGrid();
    </script>
</body>
</html>
