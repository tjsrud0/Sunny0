# Sunny0
package Java;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Calculator extends JFrame implements ActionListener {
    private JTextField textField;
    private JButton[][] buttons;
    private String[] buttonLabels = {
        "7", "8", "9", "/",
        "4", "5", "6", "x",
        "1", "2", "3", "-",
        "0", ".", "+", "=",
        "C", "CE", "Backspace", ""
    };
    private String operator;
    private double firstNumber;

    public Calculator() {
        // 프레임 설정
        setTitle("계산기");
        setSize(400, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // 텍스트 필드 생성
        textField = new JTextField("0");
        textField.setEditable(false);
        textField.setBackground(Color.WHITE);
        textField.setHorizontalAlignment(JTextField.RIGHT);
        add(textField, BorderLayout.NORTH);

        // 버튼 패널 생성
        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(5, 4));

        // 버튼 생성 및 추가
        buttons = new JButton[5][4];
        for (int i = 0; i < 5; i++) {
            for (int j = 0; j < 4; j++) {
                buttons[i][j] = new JButton(buttonLabels[i * 4 + j]);
                buttons[i][j].setBackground(Color.LIGHT_GRAY);
                buttons[i][j].setForeground(Color.BLACK);
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
            String currentText = textField.getText();
            if (currentText.length() > 0) {
                textField.setText(currentText.substring(0, currentText.length() - 1));
                if (textField.getText().isEmpty()) {
                    textField.setText("0");
                }
            }
        } else if (command.equals("CE")) {
            textField.setText("0");
        } else if (command.equals("C")) {
            operator = null;
            firstNumber = 0;
            textField.setText("0");
        } else if (command.equals("=")) {
            String secondNumberStr = textField.getText();
            if (!secondNumberStr.equals("0")) {
                double secondNumber = Double.parseDouble(secondNumberStr);
                double result = 0;

                switch (operator) {
                    case "+":
                        result = firstNumber + secondNumber;
                        break;
                    case "-":
                        result = firstNumber - secondNumber;
                        break;
                    case "x":
                        result = firstNumber * secondNumber;
                        break;
                    case "/":
                        if (secondNumber != 0) {
                            result = firstNumber / secondNumber;
                        } else {
                            JOptionPane.showMessageDialog(this, "0으로 나눌 수 없습니다.");
                            return;
                        }
                        break;
                }
                textField.setText(String.valueOf(result));
                operator = null; // 연산자 초기화
            }
        } else if ("+".equals(command) || "-".equals(command) || "x".equals(command) || "/".equals(command)) {
            firstNumber = Double.parseDouble(textField.getText());
            operator = command.equals("x") ? "*" : command; // 'x'를 '*'로 변환
            textField.setText("0");
        } else {
            if (textField.getText().equals("0")) {
                textField.setText(command);
            } else {
                textField.setText(textField.getText() + command);
            }
        }
    }

    public static void main(String[] args) {
        new Calculator();
    }
}
