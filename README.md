# Java Project - Calculate Monthly Loan Payments

- The Data Entry section prompts the user to enter values for the loan amount, yearly interest rate, and number of years. If the user doesn’t enter data that’s valid, this section displays an appropriate error message and prompts the user again.
- The Formatted Results section displays a formatted version of the user’s entries as well as the formatted result of the calculation.
- The application prompts the user to continue.  

![java console project](images/loanImg.png)  

***Specifications:***  
- Convert the Yearly Interest Rate to Monthly Interest Rate -```monthlyInterestRate = yearlyInterestRate/12/100```
- The formula for calculating monthly payment is - ```double monthlyPayment = loanAmount * monthlyInterestRate / (1 - 1/Math.pow(1 + monthlyInterestRate, months));```
- The application should accept decimal entries for the loan amount and interest rate entries.
- The application should only accept integer values for the years field.
- The application should only accept integer and decimal values within the following ranges:  
  Loan amount: 0 - 1,000,000  
  Yearly interest rate: 0 - 20  
  Years:  0 - 100  
- The application should only accept a value of “y” or “n” at the Continue prompt.
- If the user enters invalid data, the application should display an appropriate error message and prompt the user again until the user enters valid data.
```Java
import java.text.NumberFormat;
import java.util.Scanner;

/**
 * @author Faruk Dalbay
 */
public class Main {

    public static String welcome = "\nWelcome to the Loan Calculator\n";
    public static String header = "DATA ENTRY";
    public static String result = "\nFORMATTED RESULT";
    public static String error = "Error! ";
    public static String errorDecimal = error + "Invalid decimal value. Try again.";
    public static String errorZero = error + "Number must be greater than 0";
    public static double loanAmount = 0.0;
    public static double interestRate = 0.0;
    public static int years = 0;
    public static boolean valid = false;
    public static boolean choice = false;
    public static Scanner sc = new Scanner(System.in);

    public static void Display(String message) {
        System.out.println(message);
    }

    public static void Print(String messge) {
        System.err.print(messge);
    }

    // Accept user input for Years
    public static int getYears() {
        do {
            Print("Enter number of years:         ");
            try {
                years = sc.nextInt();
                valid = validateYear(years);
            } catch (Exception e) {
                Display(error + "Invalid integer value. Try again.");
            }
            sc.nextLine();
        } while (!valid);
        valid = false;
        return years;
    }

    // Validate Year input
    public static boolean validateYear(int years) {
        if (years < 0) {
            Display(errorZero);
            return false;
        }
        if (years > 100) {
            Display(error + "Number must be less than 100");
            return false;
        }
        return true;
    }

    // Accept user input for the Interest Rate
    public static double getInterestRate() {
        do {
            Print("Enter yearly interest rate: ");
            try {
                interestRate = sc.nextDouble();
                valid = validateInterestRate(interestRate);
            } catch (Exception e) {
                Display(errorDecimal);
            }
            sc.nextLine();
        } while (!valid);
        valid = false;
        return interestRate;
    }

    // Validate Interest Rate input
    public static boolean validateInterestRate(double interest) {
        if (interest < 0) {
            Display(errorZero);
            return false;
        }
        if (interest > 20) {
            Display(error + "Number must be less than 20");
            return false;
        }
        return true;
    }

    // Accept user input for the Loan Amount
    public static double getLoanAmount() {
        do {
            Print("Enter loan amount:        ");
            try {
                loanAmount = sc.nextDouble();
                valid = validateLoanAmount(loanAmount);
            } catch (Exception e) {
                Display(errorDecimal);
            }
            //discard any other data entered on the line (otherwise you end up in an infinite loop)
            sc.nextLine();
        } while (!valid);
        valid = false;
        return loanAmount;
    }

    // Validate Loan Amount input
    public static boolean validateLoanAmount(double amount) {
        if (amount < 0.0) {
            Display(error + errorZero + ".0");
            return false;
        }
        if (amount > 1000000) {
            Display(error + "Number must be less than 1000000.0");
            return false;
        }
        return true;
    }

    // Convert double input to string currency output
    public static String convertToCurrency(double value) {
        double money = value;
        NumberFormat formatter = NumberFormat.getCurrencyInstance();
        return formatter.format(money);
    }

    // Calculate the monthly payments
    public static double calculatePayments() {
        double monthlyInterestRate = interestRate / 12 / 100;
        return loanAmount * monthlyInterestRate / (1 - 1 / Math.pow(1 + monthlyInterestRate, years * 12));
    }

    // Continue app execution
    public static boolean Continue() {
        boolean leave = false;
        do {
            Print("Continue? (y/n): ");
            String selection = sc.nextLine();
            if (selection.isEmpty()) {
                Display(error + "This entry is required. Try again");
                continue;
            }
            if ((!selection.matches("y")) && (!selection.matches("n"))) {
                Display(error + "Entry must be 'y' or 'n'. Try again");
            } else {
                if (selection.matches("n")) {
                    leave = true;
                } else if (selection.matches("y")) {
                    return leave;
                }
            }
        } while (!leave);
        return leave;
    }

    // Display the Results
    public static void displayResults() {
        Display(result);
        Display("Loan amount:             " + convertToCurrency(loanAmount));
        Display("Yearly interest rate:    " + Double.toString(interestRate) + "%");
        Display("Number of Years:         " + Integer.toString(years));
        Display("Monthly payment:         " + convertToCurrency(calculatePayments()));
    }

    public static void main(String[] args) {
        do {
            Display(welcome);
            Display(header);
            loanAmount = getLoanAmount();
            interestRate = getInterestRate();
            years = getYears();
            displayResults();
            choice = Continue();
        } while (!choice);
        System.out.println("bye");
    }
}
```
