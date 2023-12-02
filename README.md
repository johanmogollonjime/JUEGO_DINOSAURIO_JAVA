# JUEGO_DINOSAURIO_JAVA
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;

class DinosaurGame extends JFrame implements KeyListener {
    private int maxX = 40;
    private int maxY = 10;
    private int playerX = 10;
    private int playerY = maxY - 3;
    private int jump = 0;
    private int obstacleX = maxX;
    private int score = 0;
    private int successfulJumps = 0; // Nuevo marcador para contar saltos exitosos
    private Timer timer;
    private JLabel scoreLabel;
    private JLabel jumpsLabel; // Nuevo JLabel para mostrar el marcador de saltos exitosos

    public DinosaurGame() {
        setTitle("Dinosaur Game");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 200);
        setLocationRelativeTo(null);
        setResizable(false);
        setLayout(new BorderLayout());

        addKeyListener(this);

        scoreLabel = new JLabel("Score: " + score, SwingConstants.CENTER);
        scoreLabel.setFont(new Font("Arial", Font.BOLD, 16));
        add(scoreLabel, BorderLayout.NORTH);

        jumpsLabel = new JLabel("Successful Jumps: " + successfulJumps, SwingConstants.CENTER);
        jumpsLabel.setFont(new Font("Arial", Font.BOLD, 16));
        add(jumpsLabel, BorderLayout.SOUTH);

        timer = new Timer(100, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                update();
                repaint();
            }
        });
        timer.start();
    }

    private void update() {
        obstacleX--;

        if (playerX == obstacleX && playerY == maxY - 3) {
            JOptionPane.showMessageDialog(this, "¡Has perdido! Puntuación: " + score + " - Successful Jumps: " + successfulJumps);
            timer.stop();
            int choice = JOptionPane.showConfirmDialog(this, "¿Quieres jugar de nuevo?", "Replay", JOptionPane.YES_NO_OPTION);
            if (choice == JOptionPane.YES_OPTION) {
                restart();
            } else {
                System.exit(0);
            }
        }

        if (obstacleX == 0) {
            score++;
            obstacleX = maxX;
        }

        // Incrementar los saltos exitosos si se salta el obstáculo
        if (jump != 0 && obstacleX == playerX) {
            successfulJumps++;
        }

        if (jump != 0) {
            playerY -= 2;
            if (playerY <= 0) {
                jump = 0;
                playerY = maxY - 3;
            }
        }

        // Actualizar el texto de los JLabels con los valores actualizados
        scoreLabel.setText("Score: " + score);
        jumpsLabel.setText("Successful Jumps: " + successfulJumps);
    }

    private void restart() {
        playerX = 10;
        playerY = maxY - 3;
        jump = 0;
        obstacleX = maxX;
        score = 0;
        successfulJumps = 0; // Reiniciar los saltos exitosos
        scoreLabel.setText("Score: " + score);
        jumpsLabel.setText("Successful Jumps: " + successfulJumps);
        timer.start();
    }

    @Override
    public void paint(Graphics g) {
        super.paint(g);

        for (int i = 0; i < maxX; i++) {
            g.drawString("_", i * 10, (maxY - 3) * 20);
        }

        g.drawString("&", playerX * 10, playerY * 20);
        g.drawString("X", obstacleX * 10, (maxY - 3) * 20);
    }

    @Override
    public void keyTyped(KeyEvent e) {
    }

    @Override
    public void keyPressed(KeyEvent e) {
        if (e.getKeyCode() == KeyEvent.VK_SPACE && jump == 0) {
            jump = 2;
        }
    }

    @Override
    public void keyReleased(KeyEvent e) {
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                DinosaurGame game = new DinosaurGame();
                game.setVisible(true);
            }
        });
    }
}
