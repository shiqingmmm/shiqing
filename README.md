<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>爱心表白便签</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { 
            background-color: #FFE6F2; /* 粉色背景 */
            height: 100vh; 
            overflow: hidden; /* 隐藏溢出的便签 */
            cursor: pointer;
        }
        .note {
            position: absolute;
            width: 120px;
            height: 120px;
            padding: 15px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: "Arial", sans-serif;
            font-size: 16px;
            color: #E63946; /* 红色文字 */
            transform: rotate(0deg); /* 保证不歪斜 */
            transition: all 1.5s ease; /* 动画过渡 */
        }
    </style>
</head>
<body onclick="createHearts()">
    <script>
        // 不重复的表白内容列表
        const loveTexts = [
            "你是我余生的甜", "心跳为你而快", "满脑子都是你", 
            "想和你到永远", "你超可爱的", "喜欢你没道理", 
            "余生请多指教", "你是我的偏爱", "心动的信号是你", 
            "眼里只有你", "你偷走了我的心", "专属你的温柔",
            "每天都想见到你", "你让世界更美好", "爱你不止今天"
        ];

        // 心形坐标计算（基于数学公式）
        function getHeartPoints(count) {
            const points = [];
            for (let i = 0; i < count; i++) {
                const t = (i / count) * Math.PI * 2; // 角度遍历
                const x = 16 * Math.pow(Math.sin(t), 3); // x坐标
                const y = 13 * Math.cos(t) - 5 * Math.cos(2*t) - 2 * Math.cos(3*t) - Math.cos(4*t); // y坐标
                // 缩放并居中（适配屏幕）
                const scale = 20;
                const centerX = window.innerWidth / 2;
                const centerY = window.innerHeight / 2;
                points.push({
                    x: centerX + x * scale,
                    y: centerY - y * scale // 负号矫正y轴方向
                });
            }
            return points;
        }

        // 创建便签并执行动画
        function createHearts() {
            // 清除已有便签
            document.querySelectorAll('.note').forEach(n => n.remove());
            
            const count = loveTexts.length;
            const heartPoints = getHeartPoints(count); // 获取心形坐标

            // 1. 创建便签并定位到心形位置
            loveTexts.forEach((text, index) => {
                const note = document.createElement('div');
                note.className = 'note';
                note.textContent = text;
                // 初始位置：心形对应坐标
                note.style.left = `${heartPoints[index].x}px`;
                note.style.top = `${heartPoints[index].y}px`;
                document.body.appendChild(note);

                // 2. 延迟1.5秒后，聚集到屏幕中心
                setTimeout(() => {
                    note.style.left = `${window.innerWidth / 2 - 60}px`; // 居中（减去便签半宽）
                    note.style.top = `${window.innerHeight / 2 - 60}px`;
                }, 1500);

                // 3. 再延迟1.5秒后，炸开（随机位置）
                setTimeout(() => {
                    const randomX = Math.random() * (window.innerWidth - 120);
                    const randomY = Math.random() * (window.innerHeight - 120);
                    note.style.left = `${randomX}px`;
                    note.style.top = `${randomY}px`;
                }, 3000);
            });
        }
    </script>
</body>
</html>
