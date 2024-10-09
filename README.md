<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>猫狗决策币旋转动画！（by姜酒）</title>
    <style>
        body, html {
            height: 100%;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            background: #ffffff;
        }
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        .rotate-wrap {
            width: 500px;
            height: 500px;
            transform-style: preserve-3d;
            position: relative;
        }
        .rotate-wrap .front, .rotate-wrap .reverse {
            width: 100%;
            height: 100%;
            background-size: cover;
            position: absolute;
            top: 0;
            left: 0;
        }
        .rotate-wrap .front {
            background-image: url('http://m.qpic.cn/psc?/V51DTRy526CTpY0GNoJf0sZsMG0UfFCO/LiySpxowE0yeWXwBdXN*SVIVroVCzsUKYzV8Nfe3vQhJBG.YC*LWiVlvoLm*S4IhHIcPGW0Ib817T7yreP5FO7pGJzcHngutvzqJroAalkY!/b&bo=gAKAAoACgAIDORw!&rf=viewer_4');
        }
        .rotate-wrap .reverse {
            background-image: url('https://m.qpic.cn/psc?/V51DTRy526CTpY0GNoJf0sZsMG0UfFCO/LiySpxowE0yeWXwBdXN*SZsbhckHKaXODLLjw2IZi6Cb.U*i4Y*5uw3eKQwMyuOcahbzAZ8r129t*gU75y7TQpK*K29PB.6uWltjUzS*LYY!/b&bo=gAKAAoACgAIDCSw!&rf=viewer_4');
            transform: scaleX(-1);
        }
        .images-container {
            display: flex; /* Make the container flex to align children in a row */
        }
        .gif-img {
            width: 200px;
            height: auto;
            margin: 10px; /* Add margin for spacing around images */
        }
        .button-container {
            display: flex;
            justify-content: center;
        }
        button, input[type="file"] {
            margin: 10px;
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover, input[type="file"]:hover {
            background-color: #ccc;
        }
    </style>
</head>
<body>
<div class="container">
    <div class="button-container">
        <input type="file" id="upload-front" accept="image/*">
        <input type="file" id="upload-back" accept="image/*">
    </div>
    <div class="images-container">
        <img class="gif-img" src="https://phototj.photo.store.qq.com/psc?/V51DTRy526CTpY0GNoJf0sZsMG0UfFCO/bqQfVz5yrrGYSXMvKr.cqX1mTge.9HZrdGSM*79oUiFUUdfhGIIQEo1VXz0mpE9Vrj8cxXgjREzgzt2WHk.zxuZpdVMXlvFJbtLHGQ9e63o!/b&bo=LAEsASwBLAECCS0!&rf=viewer_4" alt="Dynamic Image 1">
        <img class="gif-img" src="http://phototj.photo.store.qq.com/psc?/V51DTRy526CTpY0GNoJf0sZsMG0UfFCO/bqQfVz5yrrGYSXMvKr.cqb82V7njBWazucHupbeNuz49JICvGXpfX5ESGMXcxdcSUElb20fpy4Qfg7Jh5p1WOI.hEwGmtGyQ2K07Yb2YXXg!/b&bo=LAEsASwBLAECGT0!&rf=viewer_4" alt="Dynamic Image 2">
    </div>
    <div class="button-container">
        <button onclick="startRotate()">开始</button>
        <button onclick="stopRotate()">停止</button>
    </div>
    <div class="rotate-wrap" id="coin">
        <div class="front circle" style="transform: translateZ(1px);"></div>
        <div class="reverse circle"></div>
    </div>
</div>

<script>
function startRotate() {
    var coin = document.getElementById('coin');
    coin.style.animation = 'rotate 2s linear infinite';
}

function stopRotate() {
    var coin = document.getElementById('coin');
    coin.style.animation = 'none';
}

document.getElementById('upload-front').addEventListener('change', function(event) {
    var reader = new FileReader();
    reader.onload = function(e) {
        var front = document.querySelector('.rotate-wrap .front');
        front.style.backgroundImage = `url('${e.target.result}')`;
    };
    reader.readAsDataURL(event.target.files[0]);
});

document.getElementById('upload-back').addEventListener('change', function(event) {
    var reader = new FileReader();
    reader.onload = function(e) {
        var back = document.querySelector('.rotate-wrap .reverse');
        back.style.backgroundImage = `url('${e.target.result}')`;
    };
    reader.readAsDataURL(event.target.files[0]);
});
</script>

</body>
</html>




<script>
var coin = document.getElementById('coin');
var animation = null;
var storedAngle = 0;

function startRotate() {
    if (!animation) {
        coin.style.transform = `rotateY(${storedAngle}deg)`;
        animation = coin.animate([
            { transform: `rotateY(${storedAngle}deg)` },
            { transform: `rotateY(${storedAngle + 360}deg)` }
        ], {
            duration: 100,
            iterations: Infinity,
            fill: 'forwards'
        });
    } else {
        animation.play();
    }
}
function stopRotate() {
    if (animation) {
        // 暂停动画
        animation.pause();

        // 获取硬币当前的转动矩阵
        var computedStyle = window.getComputedStyle(coin);
        var matrix = computedStyle.transform;

        // 从matrix中解析出当前角度
        var values = matrix.match(/matrix.*\((.+)\)/)[1].split(', ');
        var a = parseFloat(values[0]);
        var b = parseFloat(values[1]);
        
        // 计算当前的角度
        var angle = Math.atan2(b, a) * (180 / Math.PI);
        // 将角度转换为0-360度范围
        if (angle < 0) {
            angle += 360;
        }
        angle = angle % 360; // 规范到0-360度

        // 确定最近的正面或反面角度
        var finalAngle;
        if ((angle >= 0 && angle < 90) || (angle > 270 && angle <= 360)) {
            finalAngle = 0; // 更接近正面
        } else {
            finalAngle = 180; // 更接近反面
        }

        // 计算应旋转到的目标角度
        var targetAngle = storedAngle + (finalAngle - (storedAngle % 360));
        
        // 应用角度修正
        coin.style.transform = `rotateY(${targetAngle}deg)`;

        // 更新存储的角度
        storedAngle = targetAngle;

        // 重置并取消当前动画
        animation.cancel();
        animation = null;
    }
}


</script>

</body>
</html>
