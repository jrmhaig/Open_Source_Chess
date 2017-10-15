---
layout: default
---
  <div id="board"></div>
  <pre id="moves"></pre>

  <script>
    gameMoves = "";
    function loadGame() {
      $.ajax({
        url: "CURRENT_GAME.pgn",
        dataType: "text",
        success: function(data) {
          gameMoves = data;
          $("#moves").html(data);
          game.load_pgn(data);
          board.position(game.fen());
        }
      });
    }

    var removeGreySquares = function() {
      $('#board .square-55d63').css('background', '');
    };

    var greySquare = function(square) {
      var squareEl = $('#board .square-' + square);
  
      var background = '#a9a9a9';
      if (squareEl.hasClass('black-3c85d') === true) {
        background = '#696969';
      }

      squareEl.css('background', background);
    };

    var onMouseoverSquare = function(square, piece) {
      // get list of possible moves for this square
      var moves = game.moves({
        square: square,
        verbose: true
      });

      // exit if there are no moves available for this square
      if (moves.length === 0) return;

      // highlight the square they moused over
      greySquare(square);

      // highlight the possible squares for this piece
      for (var i = 0; i < moves.length; i++) {
        greySquare(moves[i].to);
      }
    };

    var onMouseoutSquare = function(square, piece) {
      removeGreySquares();
    };

    var game=new Chess();
    var board = ChessBoard('board', {
      onMouseoutSquare: onMouseoutSquare,
      onMouseoverSquare: onMouseoverSquare
    });

    $(document).ready(function() {
      loadGame();      
    });
  </script>

