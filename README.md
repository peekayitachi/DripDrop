    import javax.swing.*;
    import java.awt.*;
    import java.awt.event.ActionEvent;
    import java.awt.event.ActionListener;
    import java.util.ArrayList;
    import java.util.List;
    
    public class Main {
        private static List<String> cartItems = new ArrayList<>();
        private static List<Integer> cartPrices = new ArrayList<>();
        private static String loggedInUserId = null;
        private static int totalPurchases = 0;
        private static int totalAmount = 0;
    
        public static void main(String[] args) {
            SwingUtilities.invokeLater(() -> createAndShowGUI());
        }
    
        private static void createAndShowGUI() {
            JFrame frame = new JFrame("Water Delivery Service");
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setSize(400, 200);
            frame.setLayout(new BorderLayout());
    
            JMenuBar menuBar = new JMenuBar();
    
            JMenu optionsMenu = new JMenu("\u2261"); // Unicode symbol for three parallel lines
            JMenuItem buyNowItem = new JMenuItem("Buy Now");
            JMenuItem cartItem = new JMenuItem("Cart");
            JMenuItem myAccountItem = new JMenuItem("My Account");
    
            JMenu loginSubMenu = new JMenu("Login");
            JMenuItem loginItem = new JMenuItem("Login");
            loginSubMenu.add(loginItem);
    
            optionsMenu.add(buyNowItem);
            optionsMenu.add(cartItem);
            optionsMenu.add(myAccountItem);
            optionsMenu.add(loginSubMenu);
    
            menuBar.add(optionsMenu);
            frame.setJMenuBar(menuBar);
    
            myAccountItem.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    showAccountDetailsDialog();
                }
            });
    
            buyNowItem.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    showBuyOptionsDialog();
                }
            });
    
            loginItem.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    showLoginDialog();
                }
            });
    
            cartItem.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    showCartDialog();
                }
            });
    
            JLabel headingLabel = new JLabel("Water Delivery Service", SwingConstants.CENTER);
            Font font = headingLabel.getFont();
            headingLabel.setFont(new Font(font.getName(), Font.PLAIN, 48)); // Set font size to 48
            frame.add(headingLabel, BorderLayout.NORTH);
    
            frame.setVisible(true);
        }
    
        private static void showLoginDialog() {
            JFrame loginFrame = new JFrame("Login");
            JPanel loginPanel = new JPanel(new GridLayout(3, 1));
            JLabel idLabel = new JLabel("ID:");
            JTextField idField = new JTextField(10);
            JLabel passwordLabel = new JLabel("Password:");
            JPasswordField passwordField = new JPasswordField(10);
            JButton loginButton = new JButton("Login");
    
            loginPanel.add(idLabel);
            loginPanel.add(idField);
            loginPanel.add(passwordLabel);
            loginPanel.add(passwordField);
            loginPanel.add(loginButton);
    
            loginButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    String userId = idField.getText();
                    char[] password = passwordField.getPassword();
    
                    if (authenticate(userId, new String(password))) {
                        loggedInUserId = userId;
                        JOptionPane.showMessageDialog(loginFrame, "Login successful!", "Success", JOptionPane.INFORMATION_MESSAGE);
                        loginFrame.dispose();
                    } else {
                        JOptionPane.showMessageDialog(loginFrame, "Login failed. Please try again.", "Error", JOptionPane.ERROR_MESSAGE);
                    }
    
                    passwordField.setText("");
                }
            });
    
            loginFrame.add(loginPanel);
            loginFrame.pack();
            loginFrame.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
            loginFrame.setVisible(true);
        }
    
        private static boolean authenticate(String userId, String password) {
            // Simulate authentication (replace with actual authentication logic)
            return userId.equals("user123") && password.equals("password");
        }
    
        private static void showAccountDetailsDialog() {
            if (loggedInUserId != null) {
                JFrame accountFrame = new JFrame("My Account");
                accountFrame.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
                accountFrame.setSize(300, 150);
    
                JLabel idLabel = new JLabel("Account ID: " + loggedInUserId);
                JLabel purchasesLabel = new JLabel("Previous Purchases: " + totalPurchases + " items");
                JLabel totalAmountLabel = new JLabel("Total Amount: $" + totalAmount);
    
                JPanel accountPanel = new JPanel(new GridLayout(3, 1));
                accountPanel.add(idLabel);
                accountPanel.add(purchasesLabel);
                accountPanel.add(totalAmountLabel);
    
                accountFrame.add(accountPanel);
                accountFrame.setVisible(true);
            } else {
                JOptionPane.showMessageDialog(null, "Please log in to view your account details.", "Login Required", JOptionPane.WARNING_MESSAGE);
            }
        }
    
        private static void showBuyOptionsDialog() {
            JFrame buyFrame = new JFrame("Buy Options");
            buyFrame.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
            buyFrame.setSize(400, 400);
            buyFrame.setLayout(new GridLayout(3, 1));
    
            createBuyOptionPanel(buyFrame, "1 Water Tank ($50)", 50);
            createBuyOptionPanel(buyFrame, "2 Water Tanks ($90)", 90);
            createBuyOptionPanel(buyFrame, "3 Water Tanks ($150)", 150);
    
            buyFrame.setVisible(true);
        }
    
        private static void createBuyOptionPanel(Container container, String optionText, int price) {
            JPanel optionPanel = new JPanel(new BorderLayout());
            JLabel optionLabel = new JLabel(optionText);
            JLabel priceLabel = new JLabel("Price: $" + price);
            JButton addToCartButton = new JButton("Add to Cart");
    
            optionPanel.add(optionLabel, BorderLayout.CENTER);
            optionPanel.add(priceLabel, BorderLayout.SOUTH);
            optionPanel.add(addToCartButton, BorderLayout.EAST);
    
            container.add(optionPanel);
    
            addToCartButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    addToCart(optionText, price);
                }
            });
        }
    
        private static void addToCart(String item, int price) {
            cartItems.add(item);
            cartPrices.add(price);
            totalPurchases++;
            totalAmount += price;
        }
    
        private static void showCartDialog() {
            JFrame cartFrame = new JFrame("Cart");
            cartFrame.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
            cartFrame.setSize(400, 300);
            cartFrame.setLayout(new BorderLayout());
    
            JTextArea cartTextArea = new JTextArea();
            cartTextArea.setEditable(false);
    
            JScrollPane scrollPane = new JScrollPane(cartTextArea);
            cartFrame.add(scrollPane, BorderLayout.CENTER);
    
            int total = 0;
            cartTextArea.append("Cart Contents:\n");
            for (int i = 0; i < cartItems.size(); i++) {
                cartTextArea.append(cartItems.get(i) + " - $" + cartPrices.get(i) + "\n");
                total += cartPrices.get(i);
            }
            cartTextArea.append("Total: $" + total);
    
            JButton buyButton = new JButton("Buy");
            buyButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    completePurchase();
                    cartFrame.dispose();
                }
            });
    
            cartFrame.add(buyButton, BorderLayout.SOUTH);
            cartFrame.setVisible(true);
        }
    
        private static void completePurchase() {
            cartItems.clear();
            cartPrices.clear();
            totalPurchases = 0;
            totalAmount = 0;
            JOptionPane.showMessageDialog(null, "Purchase completed. Cart is now empty.", "Purchase Completed", JOptionPane.INFORMATION_MESSAGE);
        }
    }
