<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>YSWMF</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      background: black;
      height: 100%;
      width: 100%;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      font-family: "Monotype Corsiva", sans-serif;
    }

    button {
     
      padding: 12px 24px;
      font-size: 18px;
      border: none;
      border-radius: 8px;
      background-color: #8fbc8f;
      color: white;
      cursor: pointer;
      z-index: 10;
    }

    canvas {
      display: none;
      width: 100%;
      height: 100vh;
    }
  </style>
</head>
<body>

  <button onclick="startSketch()">Click to Draw</button>
  <canvas id="drawCanvas"></canvas>

  <script>
    function startSketch() {
      const button = document.querySelector("button");
      button.style.display = "none";

      const canvas = document.getElementById("drawCanvas");
      const ctx = canvas.getContext("2d");
      const img = new Image();
      img.src = "00.png";

      img.onload = () => {
        const aspectRatio = img.width / img.height;
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        canvas.style.display = "block";

        // Calculate drawing size to fit fullscreen
        let drawWidth, drawHeight;
        if (canvas.width / canvas.height < aspectRatio) {
          drawWidth = canvas.width;
          drawHeight = drawWidth / aspectRatio;
        } else {
          drawHeight = canvas.height;
          drawWidth = drawHeight * aspectRatio;
        }

        const offsetX = (canvas.width - drawWidth) / 2;
        const offsetY = (canvas.height - drawHeight) / 2;

        let currentY = 0;
        const step = 5;

        function drawStep() {
          if (currentY < img.height) {
            ctx.drawImage(
              img,
              0, currentY, img.width, step,
              offsetX, offsetY + (currentY * (drawHeight / img.height)),
              drawWidth, step * (drawHeight / img.height)
            );
            currentY += step;
            requestAnimationFrame(drawStep);
          }
        }

        drawStep();
      };
    }
  </script>

</body>
</html>
