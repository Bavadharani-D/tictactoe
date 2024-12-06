# tictactoe
package javatictactoefinal;
import java.awt.*;
import java.awt.event.*;

public class TicTacToeAWT extends Frame implements ActionListener {
    private Button[] buttons = new Button[9]; // 9 buttons for the grid
    private char currentPlayer = 'X'; // Current player ('X' or 'O')
    private boolean gameWon = false; // To track if the game is won

    public TicTacToeAWT() {
        // Set up the Frame
        setTitle("Tic-Tac-Toe");
        setSize(400, 400);
        setLayout(new GridLayout(3, 3));
        
        // Initialize buttons
        for (int i = 0; i < 9; i++) {
            buttons[i] = new Button("");
            buttons[i].setFont(new Font("Arial", Font.BOLD, 50));
            buttons[i].addActionListener(this);
            add(buttons[i]);
        }
        
        addWindowListener(new WindowAdapter() {
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });

        setVisible(true);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (gameWon) return; // Ignore clicks after the game is won

        Button clickedButton = (Button) e.getSource();

        // If the button is already marked, do nothing
        if (!clickedButton.getLabel().equals("")) {
            return;
        }

        // Mark the button with the current player's symbol
        clickedButton.setLabel(String.valueOf(currentPlayer));

        // Check if the game is won or drawn
        if (checkWin()) {
            gameWon = true;
            showResult("Player " + currentPlayer + " wins!");
        } else if (isDraw()) {
            showResult("It's a draw!");
        } else {
            // Switch player
            currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
        }
    }

    private boolean checkWin() {
        // Check rows, columns, and diagonals
        int[][] winPatterns = {
            {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, // Rows
            {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, // Columns
            {0, 4, 8}, {2, 4, 6}             // Diagonals
        };

        for (int[] pattern : winPatterns) {
            if (!buttons[pattern[0]].getLabel().equals("") &&
                buttons[pattern[0]].getLabel().equals(buttons[pattern[1]].getLabel()) &&
                buttons[pattern[0]].getLabel().equals(buttons[pattern[2]].getLabel())) {
                return true;
            }
        }
        return false;
    }

    private boolean isDraw() {
        for (Button button : buttons) {
            if (button.getLabel().equals("")) {
                return false; // If any button is empty, the game is not a draw
            }
        }
        return true; // All buttons are filled and no one has won
    }

    private void showResult(String message) {
        // Display the result in a dialog box
        Dialog resultDialog = new Dialog(this, "Game Over", true);
        resultDialog.setLayout(new FlowLayout());
        resultDialog.setSize(300, 150);

        Label resultLabel = new Label(message);
        resultLabel.setFont 
        (new Font("Arial", Font.BOLD, 16));
        resultDialog.add(resultLabel);

        Button okButton = new Button("OK");
        okButton.addActionListener(e -> System.exit(0));
        resultDialog.add(okButton);

        resultDialog.setVisible(true);
    }

    public static void main(String[] args) {
        new TicTacToeAWT();
    }
}
