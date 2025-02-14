<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>情人节快乐 - 刘宇航 & 李爽</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: linear-gradient(135deg, #ff9a9e, #fad0c4);
            font-family: 'Arial', sans-serif;
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            position: relative;
        }

        .container {
            text-align: center;
            z-index: 2;
        }

        h1 {
            font-size: 3rem;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        p {
            font-size: 1.5rem;
            margin-bottom: 40px;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
        }

        .rose {
            font-size: 10rem;
            color: #ff4d4d;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-20px);
            }
        }

        .heart {
            position: absolute;
            top: -10%;
            font-size: 2rem;
            color: #ff4d4d;
            animation: fall 5s linear infinite;
            z-index: 1;
        }

        @keyframes fall {
            0% {
                transform: translateY(-10%) rotate(0deg);
            }
            100% {
                transform: translateY(110vh) rotate(360deg);
            }
        }

        .input-container {
            position: absolute;
            top: 20px;
            z-index: 3;
            background: rgba(255, 255, 255, 0.8);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .input-container input {
            padding: 10px;
            font-size: 1rem;
            border: 1px solid #ccc;
            border-radius: 5px;
            margin-right: 10px;
        }

        .input-container button {
            padding: 10px 20px;
            font-size: 1rem;
            background: #ff4d4d;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .input-container button:hover {
            background: #ff1a1a;
        }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="input-container">
        <input type="text" id="yourName" placeholder="你的名字">
        <input type="text" id="loverName" placeholder="爱人的名字">
        <button onclick="checkNames()">确认</button>
    </div>

    <div class="container hidden" id="content">
        <h1>情人节快乐！</h1>
        <p>刘宇航 & 李爽</p>
        <p>2023年3月9号相识相知相爱至今</p>
        <p>从那一刻起，我的世界因你而美丽。<br>每一天都像是情人节，因为有你在我身边。<br>愿我们的爱如这玫瑰般绚烂，永远绽放。</p>
        <div class="rose">🌹</div>
    </div>

    <script>
        // 检查名字
        function checkNames() {
            const yourName = document.getElementById("yourName").value;
            const loverName = document.getElementById("loverName").value;

            if (yourName === "刘宇航" && loverName === "李爽") {
                document.querySelector(".input-container").classList.add("hidden");
                document.getElementById("content").classList.remove("hidden");
                createHearts();
            } else {
                alert("名字错误，程序无法运行。");
            }
        }

        // 创建满屏跳动的心
        function createHearts() {
            const numHearts = 50;
            for (let i = 0; i < numHearts; i++) {
                const heart = document.createElement("div");
                heart.classList.add("heart");
                heart.innerHTML = "❤";
                heart.style.left = `${Math.random() * 100}vw`;
                heart.style.animationDuration = `${Math.random() * 3 + 2}s`;
                document.body.appendChild(heart);
            }
        }
    </script>
</body>
</html>
