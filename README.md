<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Pong Game JavaScript</title>
    <style>
        body{
            background-color: dimgray;
        }
        #pong{
            border: 2px solid #FFF;
            position: absolute;
            margin :auto;
            top:0;
            right:0;
            left:0;
            bottom:0;
        }
    </style>
</head>
<body>
   <canvas id="pong" width="600" height="400"></canvas>
   <script src="pong.js"></script>
</body>
</html>



const canvas = document.getElementById("pong");


const ctx = canvas.getContext('2d');



const ball = {
    x : canvas.width/2,
    y : canvas.height/2,
    radius : 10,
    velocityX : 5,
    velocityY : 5,
    speed : 7,
    color : "WHITE"
}

const user = {
    x : 0, 
    y : (canvas.height - 100)/2, 
    width : 10,
    height : 100,
    score : 0,
    color : "WHITE"
}


const com = {
    x : canvas.width - 10, // - width of paddle
    y : (canvas.height - 100)/2, // -100 the height of paddle
    width : 10,
    height : 100,
    score : 0,
    color : "WHITE"
}


const net = {
    x : (canvas.width - 2)/2,
    y : 0,
    height : 10,
    width : 2,
    color : "WHITE"
}


function drawRect(x, y, w, h, color){
    ctx.fillStyle = color;
    ctx.fillRect(x, y, w, h);
}


function drawArc(x, y, r, color){
    ctx.fillStyle = color;
    ctx.beginPath();
    ctx.arc(x,y,r,0,Math.PI*2,true);
    ctx.closePath();
    ctx.fill();
}


canvas.addEventListener("mousemove", getMousePos);

function keyDownHandler(event) {
    if(event.keyCode == 39) {
        rightPressed = true;
    }
    else if(event.keyCode == 37) {
        leftPressed = true;
    }
    if(event.keyCode == 40) {
    	downPressed = true;
    }
    else if(event.keyCode == 38) {
    	upPressed = true;
    }
function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    if(rightPressed) {
        playerX += 5;
    }
    else if(leftPressed) {
        playerX -= 5;
    }
    if(downPressed) {
        playerY += 5;
    }
    else if(upPressed) {
        playerY -= 5;
    }
    ctx.drawImage(img, playerX, playerY);
    requestAnimationFrame(draw);
}
this.keyUp = this.input.keyboard.addKey(Phaser.KeyCode.W);
this.keyDown = this.input.keyboard.addKey(Phaser.KeyCode.S);
}
document.addEventListener('keyup', event => {
  if (event.code === 'Space') {
    console.log('Space pressed')
  }
function gamePaused{ /**i need help on this**/ }

function keyDown(e) {
  if (e.keyCode == 80) pauseGame();
}

function pauseGame() {
  if (!gamePaused) {
    game = clearTimeout(game);
    gamePaused = true;
  } else if (gamePaused) {
    game = setTimeout(loop, 1000 / 30);
    gamePaused = false;
  }

}

function resetBall(){
    ball.x = canvas.width/2;
    ball.y = canvas.height/2;
    ball.velocityX = -ball.velocityX;
    ball.speed = 7;
}


function drawNet(){
    for(let i = 0; i <= canvas.height; i+=15){
        drawRect(net.x, net.y + i, net.width, net.height, net.color);
    }
}


function drawText(text,x,y){
    ctx.fillStyle = "#FFF";
    ctx.font = "75px fantasy";
    ctx.fillText(text, x, y);
}


function collision(b,p){
    p.top = p.y;
    p.bottom = p.y + p.height;
    p.left = p.x;
    p.right = p.x + p.width;
    
    b.top = b.y - b.radius;
    b.bottom = b.y + b.radius;
    b.left = b.x - b.radius;
    b.right = b.x + b.radius;
    
    return p.left < b.right && p.top < b.bottom && p.right > b.left && p.bottom > b.top;
}

function update(){
    
    
    if( ball.x - ball.radius < 0 ){
        com.score++;
        comScore.play();
        resetBall();
    }else if( ball.x + ball.radius > canvas.width){
        user.score++;
        userScore.play();
        resetBall();
    }
    
   
    ball.x += ball.velocityX;
    ball.y += ball.velocityY;
    
  
    com.y += ((ball.y - (com.y + com.height/2)))*0.1;
    
    if(ball.y - ball.radius < 0 || ball.y + ball.radius > canvas.height){
        ball.velocityY = -ball.velocityY;
        wall.play();
    }
    
 
    let player = (ball.x + ball.radius < canvas.width/2) ? user : com;
    

    if(collision(ball,player)){
        
        hit.play();
    
        let collidePoint = (ball.y - (player.y + player.height/2));
        
        collidePoint = collidePoint / (player.height/2);
        
     
        let angleRad = (Math.PI/4) * collidePoint;
        
       
        let direction = (ball.x + ball.radius < canvas.width/2) ? 1 : -1;
        ball.velocityX = direction * ball.speed * Math.cos(angleRad);
        ball.velocityY = ball.speed * Math.sin(angleRad);
        
       
        ball.speed += 0.1;
    }
}

function render(){
  
    drawRect(0, 0, canvas.width, canvas.height, "#000");
    
    
    drawText(user.score,canvas.width/4,canvas.height/5);
    
    drawText(com.score,3*canvas.width/4,canvas.height/5);
    
    
    drawNet();
  
    drawRect(user.x, user.y, user.width, user.height, user.color);
    
   
    drawRect(com.x, com.y, com.width, com.height, com.color);
    
    
    drawArc(ball.x, ball.y, ball.radius, ball.color);
}
function game(){
    update();
    render();
}

let framePerSecond = 50;

let loop = setInterval(game,1000/framePerSecond);
