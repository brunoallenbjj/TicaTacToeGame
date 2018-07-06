# TicaTacToeGame
Just a simple game in JS.


<!DOCTYPE html>
<html>
<link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.0.0-alpha.6/css/bootstrap.min.css">
<link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js">
<meta charset="UTF-8">


<!-- CSS CODE -->

<style type="text/css">

.container{
  text-align: center;
}


.board {
  width: 252px;
  height: 252px;
  margin: auto;
}

.square {
  margin: 2px;
  width: 80px;
  height: 80px;
  float: left;
  font-size: 48px;
  padding: 6px;
  cursor: pointer;
  background-color: #CCC;
}	

</style>


<!-- HTML CODE -->


<body>

<div class="container">
  <h3>Tic Tac Toe</h3>
  <p class="small">You know what to do : )</p>
  <div class="btn-group" role="group" aria-label="...">
  <button type="button" class="btn symbol btn-default btn-primary">X</button>
  <button type="button" class="btn btn-danger" id="restart">Restart</button>
  <button type="button" class="btn symbol btn-default">O</button>
</div>
  </br></br>
  <div class="board">
    <div class="square"> </div>
    <div class="square"> </div>
    <div class="square"> </div>
    <div class="square"> </div>
    <div class="square"> </div>
    <div class="square"> </div>
    <div class="square"> </div>
    <div class="square"> </div>
    <div class="square"> </div>
  </div>
  </br>
</div>

<!-- JS CODE -->

<script type="text/javascript">	


var players = {
  "human": "X",
  "bot": "O"
};
var scores = {
  "X": -1,
  "O": 1,
  " ": 0
};
var combinations = [
  [0,1,2],
  [3,4,5],
  [6,7,8],
  [0,3,6],
  [1,4,7],
  [2,5,8],
  [0,4,8],
  [2,4,6]
];

$(".square").click(function() {
  if ($(this).text() == " ") {
    $(this).text(players["human"]);
    setTimeout(play, 500);
  }
});

function play() {
  
  // loop winning combinations
  var positions = [];
  $.each(combinations, function(combination, array) {
    // use scores array to give a weighting to each combination
    positions.push(
      scores[$(".square").eq(array[0]).text()] +
      scores[$(".square").eq(array[1]).text()] +
      scores[$(".square").eq(array[2]).text()]
    );
  });
  
  // test strategies
  var squareToPlay = false;
  
  squareToPlay = strategy(positions, -3);
  if (squareToPlay) {
    // player has won the game
    gameOver("You won!"); return false;
  }
  
  squareToPlay = strategy(positions, 2);
  if (squareToPlay) {
    // play the empty space and win the game
    $(".square").eq(squareToPlay).text(players["bot"]);
    setTimeout(gameOver, 500); return false;
  }
  
  squareToPlay = strategy(positions, -2);
  if (squareToPlay) {
    // play the empty space to stop the human
    $(".square").eq(squareToPlay).text(players["bot"]);
    return false;
  }
  
  // check if board is full
  if ($(".square:contains(' ')").length == 0) {
    gameOver("Game is a draw."); return false;
  }
  
  // else choose a random square
  choose();
   
}
           
function strategy(positions, score) {
  if (positions.indexOf(score) > -1) {
    var squareToPlay = true;
    $.each(combinations[positions.indexOf(score)], function(index, value) {
      if ($(".square").eq(value).text() == " ") {
        squareToPlay = value;
      }
    });
    return squareToPlay;
  }
  return false;
}

function choose() {
  var choosing = true;
  while (choosing) {
    var random = Math.floor(Math.random() * 9) - 1;
    if ($(".square").eq(random).text() == " ") {
      $(".square").eq(random).text(players["bot"]);
      choosing = false;
    }
  }
  if ($(".square:contains(' ')").length == 0) {
    // that was the last move, so play strategies one more time
    setTimeout(play, 500);
  }
}

function gameOver(message) {
  if (message) {
    alert(message);
  } else {
    alert("You lost!");
  }
  newGame();
}

function newGame() {
  $(".square").text(" ");
}

// swap symbols (only between rounds)
$(".symbol").click(function() {
  if ($(".square:contains(' ')").length == 9) {
    // set symbols
    players["human"] = $(this).html();
    players["bot"] = ($(this).html() == "X") ? "O" : "X";
    // set scores
    scores[players["human"]] = -1;
    scores[players["bot"]] = 1;
    // set button styles
    $(".btn-primary").removeClass("btn-primary");
    $(this).addClass("btn-primary");
  } else {
    alert("Round is in progress.");
  }
});

$("#restart").click(function() {
  newGame();
});


</script>
</body>
</html>
