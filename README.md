# Sunny0
package Java;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Calculator extends JFrame implements ActionListener {
    private JTextField displayField;
    private JButton[][] buttons;
    private String[] buttonLabels = {
        "1", "2", "3", "+",
        "4", "5", "6", "-",
        "7", "8", "9", "/",
        "C", "0", "x", "=",
        "CE", "Backspace", ".", ""
    };
    private String currentOperator;
    private double firstOperand;

    public Calculator() {
        // 프레임 설정
        setTitle("Simple Calculator");
        setSize(400, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // 텍스트 필드 생성
        displayField = new JTextField("0");
        displayField.setEditable(false);
        displayField.setBackground(Color.WHITE); // 배경색 하얀색으로 변경
        displayField.setHorizontalAlignment(JTextField.RIGHT);
        displayField.setFont(new Font("Arial", Font.BOLD, 24)); // 글씨 크기 증가
        add(displayField, BorderLayout.NORTH);

        // 버튼 패널 생성
        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(5, 4));

        // 버튼 생성 및 추가
        buttons = new JButton[5][4];
        for (int i = 0; i < 5; i++) {
            for (int j = 0; j < 4; j++) {
                buttons[i][j] = new JButton(buttonLabels[i * 4 + j]);
                buttons[i][j].setBackground(Color.GRAY);
                buttons[i][j].setForeground(Color.WHITE);
                buttons[i][j].addActionListener(this);
                buttonPanel.add(buttons[i][j]);
            }
        }

        add(buttonPanel, BorderLayout.CENTER);
        setVisible(true);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        String command = e.getActionCommand();

        // 텍스트 필드에 버튼 클릭 내용을 표시
        if (command.equals("Backspace")) {
            String currentText = displayField.getText();
            if (currentText.length() > 0) {
                displayField.setText(currentText.substring(0, currentText.length() - 1));
                if (displayField.getText().isEmpty()) {
                    displayField.setText("0");
                }
            }
        } else if (command.equals("CE")) {
            displayField.setText("0");
        } else if (command.equals("C")) {
            currentOperator = null;
            firstOperand = 0;
            displayField.setText("0");
        } else if (command.equals("=")) {
            String secondOperandStr = displayField.getText();
            if (!secondOperandStr.equals("0")) {
                double secondOperand = Double.parseDouble(secondOperandStr);
                double result = 0;

                switch (currentOperator) {
                    case "+":
                        result = firstOperand + secondOperand;
                        break;
                    case "-":
                        result = firstOperand - secondOperand;
                        break;
                    case "x":
                        result = firstOperand * secondOperand;
                        break;
                    case "/":
                        if (secondOperand != 0) {
                            result = firstOperand / secondOperand;
                        } else {
                            JOptionPane.showMessageDialog(this, "Cannot divide by zero.");
                            return;
                        }
                        break;
                }
                displayField.setText(String.valueOf(result));
                currentOperator = null; // 연산자 초기화
            }
        } else if ("+".equals(command) || "-".equals(command) || "x".equals(command) || "/".equals(command)) {
            firstOperand = Double.parseDouble(displayField.getText());
            currentOperator = command.equals("x") ? "*" : command; // 'x'를 '*'로 변환
            displayField.setText("0");
        } else {
            if (displayField.getText().equals("0")) {
                displayField.setText(command);
            } else {
                displayField.setText(displayField.getText() + command);
            }
        }
    }

    public static void main(String[] args) {
        new Calculator();
    }
}
