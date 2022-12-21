// JavaScript
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const player = {
  x: 0,
  y: 0,
  width: 32,
  height: 32,
  speed: 5,
  jumping: false,
  grounded: false
};

const gravity = 0.5;
const jumpStrength = 10;

const keys = {
  left: false,
  right: false,
  up: false
};

// Handle key down events
document.addEventListener("keydown", function(event) {
  switch (event.keyCode) {
    case 37: // left arrow
      keys.left = true;
      break;
    case 38: // up arrow
      keys.up = true;
      break;
    case 39: // right arrow
      keys.right = true;
      break;
  }
});

// Handle key up events
document.addEventListener("keyup", function(event) {
  switch (event.keyCode) {
    case 37: // left arrow
      keys.left = false;
      break;
    case 38: // up arrow
      keys.up = false;
      break;
    case 39: // right arrow
      keys.right = false;
      break;
  }
});

// Main game loop
function update() {
  // Update player position based on keys pressed
  if (keys.left) {
    player.x -= player.speed;
  }
  if (keys.right) {
    player.x += player.speed;
  }
  if (keys.up && player.grounded) {
    player.jumping = true;
  }

  // Apply gravity to player
  player.y += gravity;

  // Check if player is jumping
  if (player.jumping) {
    player.y -= jumpStrength;
    jumpStrength -= 0.5;

    // Check if player has landed
    if (jumpStrength <= 0) {
      player.jumping = false;
      player.grounded = false;
      jumpStrength = 10;
    }
  }

  // Check if player has hit the ground
  if (player.y + player.height > canvas.height) {
    player.y = canvas.height - player.height;
    player.grounded = true;
  }

  // Clear the canvas
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Draw the player
  ctx.fillRect(player.x, player.y, player.width, player.height);

  // Loop the game loop
  requestAnimationFrame(update);
}

update();
