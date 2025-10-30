# PBLJ-Exp-2.3JAVA
import java.util.*;
import java.util.stream.*;
import java.util.Comparator;
import java.util.function.*;
import java.util.Map.Entry;

// -------------------- Employee Class --------------------
class Employee {
    String name;
    int age;
    double salary;

    Employee(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    @Override
    public String toString() {
        return String.format("Name: %-10s | Age: %d | Salary: %.2f", name, age, salary);
    }
}

// -------------------- Student Class --------------------
class Student {
    String name;
    double marks;

    Student(String name, double marks) {
        this.name = name;
        this.marks = marks;
    }

    @Override
    public String toString() {
        return String.format("Name: %-10s | Marks: %.2f", name, marks);
    }
}

// -------------------- Product Class --------------------
class Product {
    String name;
    double price;
    String category;

    Product(String name, double price, String category) {
        this.name = name;
        this.price = price;
        this.category = category;
    }

    @Override
    public String toString() {
        return String.format("Name: %-12s | Price: %.2f | Category: %s", name, price, category);
    }
}

// -------------------- Main Program --------------------
public class LambdaStreamDemo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice;
        do {
            System.out.println("\n===== Java Lambda & Stream Operations =====");
            System.out.println("1. Sort Employee Objects Using Lambda");
            System.out.println("2. Filter and Sort Students Using Streams");
            System.out.println("3. Stream Operations on Product Dataset");
            System.out.println("4. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1 -> partA_SortEmployees();
                case 2 -> partB_FilterStudents();
                case 3 -> partC_ProductStreams();
                case 4 -> System.out.println("Exiting... Goodbye!");
                default -> System.out.println("Invalid choice! Try again.");
            }
        } while (choice != 4);
        sc.close();
    }

    // -------------------- Part (a): Sorting Employees Using Lambda --------------------
    private static void partA_SortEmployees() {
        List<Employee> employees = Arrays.asList(
                new Employee("Abhi", 25, 55000),
                new Employee("Riya", 30, 72000),
                new Employee("Karan", 22, 45000),
                new Employee("Meena", 28, 65000)
        );

        System.out.println("\nOriginal Employee List:");
        employees.forEach(System.out::println);

        // Sort by name (alphabetically)
        employees.sort((e1, e2) -> e1.name.compareTo(e2.name));
        System.out.println("\nSorted by Name (Alphabetically):");
        employees.forEach(System.out::println);

        // Sort by age (ascending)
        employees.sort(Comparator.comparingInt(e -> e.age));
        System.out.println("\nSorted by Age (Ascending):");
        employees.forEach(System.out::println);

        // Sort by salary (descending)
        employees.sort((e1, e2) -> Double.compare(e2.salary, e1.salary));
        System.out.println("\nSorted by Salary (Descending):");
        employees.forEach(System.out::println);
    }

    // -------------------- Part (b): Filtering & Sorting Students Using Streams --------------------
    private static void partB_FilterStudents() {
        List<Student> students = Arrays.asList(
                new Student("Rohan", 68.5),
                new Student("Meera", 82.3),
                new Student("Arjun", 91.2),
                new Student("Divya", 74.6),
                new Student("Sanjay", 88.0)
        );

        System.out.println("\nAll Students:");
        students.forEach(System.out::println);

        // Filter students scoring above 75% and sort by marks
        List<String> topStudents = students.stream()
                .filter(s -> s.marks > 75)
                .sorted(Comparator.comparingDouble(s -> s.marks))
                .map(s -> s.name)
                .collect(Collectors.toList());

        System.out.println("\nStudents scoring above 75% (sorted by marks):");
        topStudents.forEach(System.out::println);
    }

    // -------------------- Part (c): Stream Operations on Product Dataset --------------------
    private static void partC_ProductStreams() {
        List<Product> products = Arrays.asList(
                new Product("Laptop", 70000, "Electronics"),
                new Product("Phone", 50000, "Electronics"),
                new Product("TV", 60000, "Electronics"),
                new Product("Shirt", 1500, "Clothing"),
                new Product("Jeans", 2500, "Clothing"),
                new Product("Jacket", 3500, "Clothing"),
                new Product("Rice", 1000, "Groceries"),
                new Product("Oil", 1200, "Groceries"),
                new Product("Sugar", 800, "Groceries")
        );

        System.out.println("\nAll Products:");
        products.forEach(System.out::println);

        // Group by category
        Map<String, List<Product>> grouped = products.stream()
                .collect(Collectors.groupingBy(p -> p.category));

        System.out.println("\nGrouped Products by Category:");
        grouped.forEach((cat, list) -> {
            System.out.println(cat + " -> " + list);
        });

        // Find the most expensive product in each category
        Map<String, Optional<Product>> maxByCategory = products.stream()
                .collect(Collectors.groupingBy(p -> p.category,
                        Collectors.maxBy(Comparator.comparingDouble(p -> p.price))));

        System.out.println("\nMost Expensive Product in Each Category:");
        maxByCategory.forEach((cat, prod) ->
                System.out.println(cat + " -> " + prod.get().name + " (₹" + prod.get().price + ")"));

        // Calculate the average price of all products
        double avgPrice = products.stream()
                .collect(Collectors.averagingDouble(p -> p.price));

        System.out.println("\nAverage Price of All Products: ₹" + avgPrice);
    }
}
