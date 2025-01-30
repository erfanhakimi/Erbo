import javax.swing.*;
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.util.ArrayList;
import java.util.Random;

public class BicycleGame extends JPanel implements Runnable, KeyListener {
    private static final int WIDTH = 800;
    private static final int HEIGHT = 400;
    private static final int GROUND = 350;
    private static final int BICYCLE_WIDTH = 50;
    private static final int BICYCLE_HEIGHT = 30;
    private static final int OBSTACLE_WIDTH = 40;
    private static final int OBSTACLE_HEIGHT = 40;

    private int bicycleX = 100;
    private int bicycleY = GROUND - BICYCLE_HEIGHT;
    private int bicycleSpeedY = 0;
    private boolean isJumping = false;

    private ArrayList<Rectangle> obstacles;
    private Random random;

    public BicycleGame() {
        setPreferredSize(new Dimension(WIDTH, HEIGHT));
        setBackground(Color.WHITE);
        setFocusable(true);
        addKeyListener(this);

        obstacles = new ArrayList<>();
        random = new Random();

        // ایجاد موانع اولیه
        for (int i = 0; i < 3; i++) {
            addObstacle();
        }
    }

    private void addObstacle() {
        int x = WIDTH + random.nextInt(300);
        int y = GROUND - OBSTACLE_HEIGHT;
        obstacles.add(new Rectangle(x, y, OBSTACLE_WIDTH, OBSTACLE_HEIGHT));
    }

    @Override
    public void run() {
        while (true) {
            update();
            repaint();
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    private void update() {
        // حرکت دوچرخه
        if (isJumping) {
            bicycleY += bicycleSpeedY;
            bicycleSpeedY += 1; // گرانش
            if (bicycleY >= GROUND - BICYCLE_HEIGHT) {
                bicycleY = GROUND - BICYCLE_HEIGHT;
                isJumping = false;
            }
        }

        // حرکت موانع
        for (Rectangle obstacle : obstacles) {
            obstacle.x -= 5;
        }

        // حذف موانع خارج از صفحه و اضافه کردن موانع جدید
        if (obstacles.get(0).x + OBSTACLE_WIDTH < 0) {
            obstacles.remove(0);
            addObstacle();
        }

        // بررسی برخورد
        Rectangle bicycleRect = new Rectangle(bicycleX, bicycleY, BICYCLE_WIDTH, BICYCLE_HEIGHT);
        for (Rectangle obstacle : obstacles) {
            if (bicycleRect.intersects(obstacle)) {
                gameOver();
                return;
            }
        }
    }

    private void gameOver() {
        System.out.println("Game Over!");
        System.exit(0);
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);

        // رسم زمین
        g.setColor(Color.GREEN);
        g.fillRect(0, GROUND, WIDTH, HEIGHT - GROUND);

        // رسم دوچرخه
        g.setColor(Color.BLUE);
        g.fillRect(bicycleX, bicycleY, BICYCLE_WIDTH, BICYCLE_HEIGHT);

        // رسم موانع (ماشین‌های سوخته)
        g.setColor(Color.RED);
        for (Rectangle obstacle : obstacles) {
            g.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
        }
    }

    @Override
    public void keyPressed(KeyEvent e) {
        if (e.getKeyCode() == KeyEvent.VK_SPACE && !isJumping) {
            isJumping = true;
            bicycleSpeedY = -15; // سرعت پرش
        }
    }

    @Override
    public void keyReleased(KeyEvent e) {}

    @Override
    public void keyTyped(KeyEvent e) {}

    public static void main(String[] args) {
        JFrame frame = new JFrame("Bicycle Game");
        BicycleGame game = new BicycleGame();
        frame.add(game);
        frame.pack();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);

        new Thread(game).start();
    }
}
