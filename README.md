# DRONE
import javax.swing.*;
import java.awt.event.*;

public class AlertAppGUI extends JFrame {
    private Tracker tracker;
    private JTextField locationField;
    private JButton alertButton;
    private JTextArea logArea;

    public AlertAppGUI() {
        tracker = new Tracker(this);

        setTitle("Alert Signal App");
        setSize(400, 300);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        setLayout(null);

        JLabel locationLabel = new JLabel("Location:");
        locationLabel.setBounds(20, 20, 80, 25);
        add(locationLabel);

        locationField = new JTextField("Latitude: 37.7749, Longitude: -122.4194");
        locationField.setBounds(100, 20, 250, 25);
        add(locationField);

        alertButton = new JButton("Send Alert");
        alertButton.setBounds(140, 60, 120, 30);
        add(alertButton);

        logArea = new JTextArea();
        logArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(logArea);
        scrollPane.setBounds(20, 110, 340, 130);
        add(scrollPane);

        alertButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String location = locationField.getText();
                int confirm = JOptionPane.showConfirmDialog(
                    AlertAppGUI.this,
                    "Do you want to send an alert signal?",
                    "Confirm Alert",
                    JOptionPane.YES_NO_OPTION
                );
                if (confirm == JOptionPane.YES_OPTION) {
                    log("Sending alert from location: " + location);
                    tracker.receiveSignal(location);
                } else {
                    log("Alert cancelled.");
                }
            }
        });
    }

    public void log(String message) {
        logArea.append(message + "\n");
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new AlertAppGUI().setVisible(true));
    }
}
public class Tracker {
    private Drone drone;
    private AlertAppGUI gui;

    public Tracker(AlertAppGUI gui) {
        this.drone = new Drone(gui);
        this.gui = gui;
    }

    public void receiveSignal(String location) {
        gui.log("Tracker received alert signal from location: " + location);
        sendDrone(location);
    }

    public void sendDrone(String location) {
        gui.log("Tracker sending drone to location...");
        drone.travelTo(location);
    }
}
Public class Drone {
    private AlertAppGUI gui;

    public Drone(AlertAppGUI gui) {
        this.gui = gui;
    }

    public void travelTo(String location) {
        gui.log("Drone is en route to: " + location);
        // Simulate travel time in a background thread
        new Thread(() -> {
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                gui.log("Drone travel interrupted.");
            }
            gui.log("Drone arrived at: " + location);
        }).start();
    }
}


