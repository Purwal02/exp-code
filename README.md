<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>File Upload Utility</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea, #764ba2);
            margin: 0;
            padding: 0;
            color: #333;
        }

        .container {
            width: 90%;
            max-width: 800px;
            margin: 40px auto;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.2);
        }

        h1 {
            text-align: center;
        }

        /* Drag Area */
        .drag-area {
            border: 2px dashed #999;
            border-radius: 10px;
            padding: 40px;
            text-align: center;
            margin: 20px 0;
            cursor: pointer;
            transition: 0.3s;
        }

        .drag-area.hover {
            border-color: #667eea;
            background-color: #f0f3ff;
        }

        .drag-area p {
            font-size: 16px;
        }

        input[type="file"] {
            display: none;
        }

        button {
            background: #667eea;
            color: #fff;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background: #5a67d8;
        }

        /* File List */
        .file-list {
            margin-top: 20px;
        }

        .file-item {
            background: #f9f9f9;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 10px;
        }

        .progress-bar {
            height: 8px;
            background: #ddd;
            border-radius: 5px;
            margin-top: 5px;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            width: 0%;
            background: #667eea;
            transition: width 0.3s;
        }

        .convert-btn {
            margin-top: 20px;
            width: 100%;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>File Converter Tool</h1>

    <!-- Drag & Drop Area -->
    <div class="drag-area" id="dragArea">
        <p>Drag & Drop Files Here</p>
        <p>or</p>
        <button onclick="fileInput.click()">Browse Files</button>
        <input type="file" id="fileInput" multiple>
    </div>

    <!-- File List -->
    <div class="file-list" id="fileList"></div>

    <!-- Convert Button -->
    <button class="convert-btn" onclick="startConversion()">Convert Files</button>
</div>

<script>
    const dragArea = document.getElementById("dragArea");
    const fileInput = document.getElementById("fileInput");
    const fileList = document.getElementById("fileList");

    let files = [];

    // Click Upload
    fileInput.addEventListener("change", () => {
        handleFiles(fileInput.files);
    });

    // Drag Events
    dragArea.addEventListener("dragover", (e) => {
        e.preventDefault();
        dragArea.classList.add("hover");
    });

    dragArea.addEventListener("dragleave", () => {
        dragArea.classList.remove("hover");
    });

    dragArea.addEventListener("drop", (e) => {
        e.preventDefault();
        dragArea.classList.remove("hover");
        handleFiles(e.dataTransfer.files);
    });

    function handleFiles(selectedFiles) {
        for (let i = 0; i < selectedFiles.length; i++) {
            files.push(selectedFiles[i]);
        }
        displayFiles();
    }

    function displayFiles() {
        fileList.innerHTML = "";

        files.forEach((file, index) => {
            const fileItem = document.createElement("div");
            fileItem.classList.add("file-item");

            fileItem.innerHTML = `
                <strong>${file.name}</strong>
                <div class="progress-bar">
                    <div class="progress" id="progress-${index}"></div>
                </div>
            `;

            fileList.appendChild(fileItem);
        });
    }

    function startConversion() {
        files.forEach((file, index) => {
            simulateProgress(index);
        });
    }

    function simulateProgress(index) {
        let progress = 0;
        const progressBar = document.getElementById(`progress-${index}`);

        const interval = setInterval(() => {
            progress += Math.random() * 20;
            if (progress >= 100) {
                progress = 100;
                clearInterval(interval);
            }
            progressBar.style.width = progress + "%";
        }, 500);
    }
</script>

</body>
</html>
