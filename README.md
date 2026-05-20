# Java_Project_Global_Currency_Converter
A high-performance, zero-instantiation international financial conversion utility built using static class encapsulation.
# International Financial Conversion Utility (`ConverterEngine`)

An elegant, production-ready Java financial conversion utility that leverages static class members to provide global currency transformation logic. By employing a static-only architecture inside a nested framework, this utility eliminates unnecessary heap allocations and object instantiation overhead, securely encapsulating core financial exchange rates and parameters away from application workflows.

## 🏛️ Architectural Design

The core philosophy of this utility is **pure functional encapsulation within an object-oriented paradigm**. 

* **Zero-Instantiation Model:** The `ConverterEngine` is defined as a static nested class. It cannot and does not need to be instantiated. All states (exchange rates) and actions (transformation methods) live entirely on the class level.
* **Secure Encapsulation:** Exchange rates are maintained as immutable `static final` constants, preventing runtime modification, memory leaks, or race conditions during multi-threaded execution.
* **Decoupled Logic:** Separation of concerns ensures that currency computation logic, supported pairings, and conditional error routing are completely isolated from the main execution wrapper.

---

## ✨ Key Features

* **Static Memory Efficiency:** Maximizes performance by bypassing object lifecycle overhead and trash collection on the heap.
* **Thread-Safe by Design:** The use of `final` constants guarantees read-only static exchange rates, eliminating data corruption risks across parallel tasks.
* **Intuitive API Contract:** Simplifies calling syntax down to an explicit, self-documenting method call: `ConverterEngine.convert(amount, currency)`.
* **Graceful Error Handling:** Features built-in validation that catches unsupported ISO currency codes and returns a predictable fallback indicator (`-1`).

---

## 💻 Source Code Overview

The utility is structured using a static class containment pattern inside Java:

```java
import java.util.Scanner;

public class Main {

    // Securely encapsulated static conversion engine
    static class ConverterEngine {

        // Immutable static exchange rates (Base: USD) - Live Rates May 2026
        static final double USD_TO_INR = 96.57;
        static final double USD_TO_EUR = 0.86;
        static final double USD_TO_GBP = 0.75;
        static final double USD_TO_JPY = 159.04;

        /**
         * Transforms a given USD amount into a targeted currency.
         * @param amount Base currency value in USD
         * @param currencyType Target ISO currency string
         * @return Converted value, or -1 if the currency is unsupported
         */
        static double convert(double amount, String currencyType) {
            switch (currencyType.toUpperCase()) {
                case "INR": return amount * USD_TO_INR;
                case "EUR": return amount * USD_TO_EUR;
                case "GBP": return amount * USD_TO_GBP;
                case "JPY": return amount * USD_TO_JPY;
                default:    return -1;
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter amount in USD: ");
        double amount = sc.nextDouble();

        System.out.print("Enter target currency (INR / EUR / GBP / JPY): ");
        String currency = sc.next();

        // Seamless execution without 'new ConverterEngine()'
        double result = ConverterEngine.convert(amount, currency);

        if (result == -1) {
            System.out.println("Invalid currency.");
        } else {
            System.out.println(amount + " USD = " + result + " " + currency.toUpperCase());
        }

        sc.close();
    }
}
