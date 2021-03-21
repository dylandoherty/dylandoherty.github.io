---
layout: default
title: Breakout
parent: Projects
nav_order: 2
---
<html>
<head>
  <meta charset="utf-8" />
  <title>Gamedev Canvas Workshop</title>
  <style>
    * {padding: 0; margin: 0;}
    canvas {background: #eee; display: block; margin: 0 auto;}
  </style>
</head>
<body>
  <canvas id="myCanvas" width="480" height="320"></canvas>
  <script>
    // Variables for canvas and canvas context
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");
    //Variables for the ball
    var x=canvas.width/2;
    var y=canvas.height-30;
    var dx=2;
    var dy=-2;
    var ballRadius=10;
    //Variables for the paddle
    var paddleHeight=10;
    var paddleWidth=75;
    var paddleX=(canvas.width-paddleWidth)/2;
    var rightPressed = false;
    var leftPressed = false;
    var start = false;
    //Variables for the bricks
    var brickRowCount=3;
    var brickColumnCount=5;
    var brickWidth=75;
    var brickHeight=20;
    var brickPadding=10;
    var brickOffsetTop=30;
    var brickOffsetLeft=30;
    var score = 0;
    //Array thing for bricks
    var bricks = [];
    for(var c=0; c<brickColumnCount; c++) {
      bricks[c] = [];
      for(var r=0; r<brickRowCount; r++) {
        bricks[c][r] = { x: 0, y: 0 , status:1};
      }
    }

    document.addEventListener("keydown",keyDownHandler,false);
    document.addEventListener("keyup",keyUpHandler,false);
    function keyDownHandler(e) {
      if(e.key=="Right" ||e.key=="ArrowRight") {
        rightPressed = true;
      }
      else if(e.key=="Left"||e.key=="ArrowLeft") {
        leftPressed=true;
      }
    }
    function keyUpHandler(e) {
      if(e.key=="Right"||e.key=="ArrowRight") {
        rightPressed=false;
      }
      else if(e.key=="Left"||e.key=="ArrowLeft") {
        leftPressed=false;
      }
    }
    //Function for collision detection
    function collisionDetection() {
      for(var c=0; c<brickColumnCount;c++) {
        for(var r=0; r<brickRowCount;r++) {
          var b = bricks[c][r];
          if(b.status == 1) {
            if (x>b.x && x<b.x+brickWidth && y>b.y && y<b.y+brickHeight) {
              dy = -dy;
              b.status = 0;
              score++;
              if (score == brickRowCount*brickColumnCount) {
                alert("YOU WIN! CONGRATULATIONS!");
                document.location.reload();
                clearInterval(interval); //for chrome losers.
              }
            }
          }
        }
      }
    }
    function drawScore() {
      ctx.font = "16px Arial";
      ctx.fillStyle = "#0095DD";
      ctx.fillText("Score: "+score, 8, 20);
    }
    //Function that draws the ball
    function drawBall() {
      ctx.beginPath();
      ctx.arc(x,y,ballRadius,0,Math.PI*2);
      ctx.fillStyle="#0095DD";
      ctx.fill();
      ctx.closePath();
    }
    //Function that draws the paddle
    function drawPaddle() {
      ctx.beginPath();
      ctx.rect(paddleX, canvas.height-paddleHeight,paddleWidth,paddleHeight);
      ctx.fillStyle="0095DD";
      ctx.fill();
      ctx.closePath();
    }
    function drawBricks() {
      for(var c=0; c<brickColumnCount;c++) {
        for(var r=0; r<brickRowCount; r++) {
          if (bricks[c][r].status == 1) {
            var brickX = (c*(brickWidth+brickPadding))+brickOffsetLeft;
            var brickY = (r * (brickHeight + brickPadding)) + brickOffsetTop;
            bricks[c][r].x = brickX;
            bricks[c][r].y = brickY;
            ctx.beginPath();
            ctx.rect(brickX, brickY, brickWidth, brickHeight);
            ctx.fillStyle = "#0095DD";
            ctx.fill();
            ctx.closePath();
          }
        }
      }
    }
    function drawIntro(){
      ctx.font = "16px Arial";
      ctx.fillStyle = "#0095DD";
      ctx.fillText("Press right to begin", 8, 20);
    }
    //Function that clears the screen, calls drawBall to actually draw it, then updates the coordinates for the next ball.
    function draw() {
      if (y+dy<ballRadius) {
        dy=-dy;
      }
      else if(y+dy>canvas.height-ballRadius) {
        if (x>paddleX && x<paddleX + paddleWidth) {
          dy=-dy;
        }
        else {
        alert("GAME OVER");
        document.location.reload();
        clearInterval(interval); //for Chrome
        }
      }
      if(x+dx>canvas.width-ballRadius || x+dx<ballRadius){
        dx=-dx;
      }
      if (start == false) {
        drawIntro();
      } else {
          ctx.clearRect(0,0,canvas.width,canvas.height);
          drawBricks();
          drawBall();
          drawPaddle();
          drawScore();
          collisionDetection();
          x += dx;
          y += dy;
          if(rightPressed) {
            paddleX += 7;
            if (paddleX+paddleWidth>canvas.width){
              paddleX=canvas.width-paddleWidth;
            }
          }
          else if(leftPressed) {
            paddleX -= 7;
            if (paddleX<0){
              paddleX=0;
            }
          }
        }
        if (start == false && rightPressed) {
          start = true;
        }
      }
    var interval = setInterval(draw,10);
  </script>
</body>
</html>
