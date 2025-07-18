import java.util.*;

class BankAccount {
    private static int idCounter = 1000;
    private int accountId;
    private String accountNumber;
    private String holderName;
    private String email;
    private String phone;
    private String bankName;
    private double balance;

    public BankAccount(String holderName, String email, String phone, String bankName) {
        this.accountId = idCounter++;
        this.accountNumber = generateFormattedAccountNumber(bankName);
        this.holderName = holderName.trim();
        this.email = email.trim();
        this.phone = phone.trim();
        this.bankName = bankName.trim();
        this.balance = 0.0;
    }

    public int getAccountId() {
        return accountId;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public String getHolderName() {
        return holderName;
    }

    public double getBalance() {
        return balance;
    }

    private String generateFormattedAccountNumber(String bankName) {
        String branch = bankName.replaceAll("[^A-Za-z]", "").toUpperCase();
        if (branch.length() < 4) {
            branch = (branch + "XXXX").substring(0, 4);
        } else {
            branch = branch.substring(0, 4);
        }
        Random rand = new Random();
        int number = 100000000 + rand.nextInt(900000000); // 9-digit number
        return branch + number;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("₹" + String.format("%.2f", amount) + " deposited successfully.");
        } else {
            System.out.println("Invalid deposit amount.");
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("₹" + String.format("%.2f", amount) + " withdrawn successfully.");
        } else {
            System.out.println("Invalid or insufficient balance.");
        }
    }

    public void displayInfo() {
        System.out.println("----------- Account Details -----------");
        System.out.println("ID            : " + accountId);
        System.out.println("Number        : " + accountNumber);
        System.out.println("Name          : " + holderName);
        System.out.println("Email         : " + email);
        System.out.println("Phone         : " + phone);
        System.out.println("Bank          : " + bankName);
        System.out.println("Current Balance: ₹" + String.format("%.2f", balance));
    }
}

public class Main {
    static Scanner sc = new Scanner(System.in);
    static List<BankAccount> accounts = new ArrayList<>();

    public static void main(String[] args) {
        int choice;
        do {
            System.out.println("\n====== Account Management System ======");
            System.out.println("1. Create Account");
            System.out.println("2. Deposit");
            System.out.println("3. Withdraw");
            System.out.println("4. View Details");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");

            while (!sc.hasNextInt()) {
                System.out.print("Invalid input. Enter a number between 1-5: ");
                sc.next();
            }

            choice = sc.nextInt();
            sc.nextLine(); // Clear buffer

            switch (choice) {
                case 1:
                    createAccount();
                    break;
                case 2:
                    depositMoney();
                    break;
                case 3:
                    withdrawMoney();
                    break;
                case 4:
                    viewDetails();
                    break;
                case 5:
                    System.out.println("Exiting system. Thank you!");
                    break;
                default:
                    System.out.println("Invalid option. Choose between 1-5.");
            }
        } while (choice != 5);
    }

    public static BankAccount findAccountById(int id) {
        for (BankAccount acc : accounts) {
            if (acc.getAccountId() == id) {
                return acc;
            }
        }
        return null;
    }

    public static void createAccount() {
        System.out.print("Enter Name: ");
        String name = sc.nextLine();
        System.out.print("Enter Email: ");
        String email = sc.nextLine();
        System.out.print("Enter Phone: ");
        String phone = sc.nextLine();
        System.out.print("Enter Bank Name: ");
        String bank = sc.nextLine();

        BankAccount acc = new BankAccount(name, email, phone, bank);
        accounts.add(acc);
        System.out.println("Account created successfully!");
        System.out.println("Your Account ID: " + acc.getAccountId());
        System.out.println("Your Account Number: " + acc.getAccountNumber());
    }

    public static void depositMoney() {
        System.out.print("Enter Account ID: ");
        if (!sc.hasNextInt()) {
            System.out.println("Account ID must be a number.");
            sc.nextLine();
            return;
        }
        int id = sc.nextInt();
        BankAccount acc = findAccountById(id);
        if (acc != null) {
            System.out.print("Enter Amount to Deposit: ₹");
            if (!sc.hasNextDouble()) {
                System.out.println("Amount must be a valid number.");
                sc.nextLine();
                return;
            }
            double amount = sc.nextDouble();
            acc.deposit(amount);
        } else {
            System.out.println("Account not found.");
        }
        sc.nextLine(); // Clear buffer
    }

    public static void withdrawMoney() {
        System.out.print("Enter Account ID: ");
        if (!sc.hasNextInt()) {
            System.out.println("Account ID must be a number.");
            sc.nextLine();
            return;
        }
        int id = sc.nextInt();
        BankAccount acc = findAccountById(id);
        if (acc != null) {
            System.out.print("Enter Amount to Withdraw: ₹");
            if (!sc.hasNextDouble()) {
                System.out.println("Amount must be a valid number.");
                sc.nextLine();
                return;
            }
            double amount = sc.nextDouble();
            acc.withdraw(amount);
        } else {
            System.out.println("Account not found.");
        }
        sc.nextLine(); // Clear buffer
    }

    public static void viewDetails() {
        System.out.print("Enter Account ID: ");
        if (!sc.hasNextInt()) {
            System.out.println("Account ID must be a number.");
            sc.nextLine();
            return;
        }
        int id = sc.nextInt();
        BankAccount acc = findAccountById(id);
        if (acc != null) {
            acc.displayInfo();
        } else {
            System.out.println("Account not found.");
        }
        sc.nextLine(); // Clear buffer
    }
}