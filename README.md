# -Develop-Java-Programs-Using-Autoboxing-Serialization-File-Handling
Part A shows autoboxing and unboxing by storing integers in an ArrayList and summing them. Part B covers serialization and deserialization of a Student object to save and restore data. Part C builds a menu-driven Employee Management System using file handling to add, display, search, update, and delete employee records with data persistence.


import java.io.*;
import java.util.*;

class Student implements Serializable {
    private static final long serialVersionUID = 1L;
    int id;
    String name;
    double marks;

    public Student(int id, String name, double marks) {
        this.id = id;
        this.name = name;
        this.marks = marks;
    }

    public void display() {
        System.out.println("ID: " + id + ", Name: " + name + ", Marks: " + marks);
    }
}

class Employee implements Serializable {
    private static final long serialVersionUID = 1L;
    int id;
    String name;
    double salary;

    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    public void display() {
        System.out.println("ID: " + id + ", Name: " + name + ", Salary: " + salary);
    }
}

public class CombinedProgram {
    static final String EMPLOYEE_FILE = "employees.dat";

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int choice;

        do {
            System.out.println("\n--- Main Menu ---");
            System.out.println("1. Sum Integers (Autoboxing)");
            System.out.println("2. Serialize/Deserialize Student");
            System.out.println("3. Employee Management System");
            System.out.println("4. Exit");
            System.out.print("Enter choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1:
                    sumIntegers();
                    break;
                case 2:
                    studentSerialization();
                    break;
                case 3:
                    employeeMenu(sc);
                    break;
                case 4:
                    System.out.println("Program Ended.");
                    break;
                default:
                    System.out.println("Invalid choice.");
            }
        } while (choice != 4);

        sc.close();
    }

    static void sumIntegers() {
        List<Integer> numbers = new ArrayList<>();
        int sum = 0;
        for (int i = 1; i <= 5; i++) {
            numbers.add(i);
        }
        for (Integer num : numbers) {
            sum += num;
        }
        System.out.println("Sum of Integers using Autoboxing and Unboxing: " + sum);
    }

    static void studentSerialization() {
        Student s1 = new Student(101, "Alice", 89.5);
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.ser"))) {
            oos.writeObject(s1);
            System.out.println("Student object serialized successfully.");
        } catch (IOException e) {
            e.printStackTrace();
        }

        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.ser"))) {
            Student s2 = (Student) ois.readObject();
            System.out.println("Deserialized Student:");
            s2.display();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    static void employeeMenu(Scanner sc) {
        int choice;
        do {
            System.out.println("\n--- Employee Menu ---");
            System.out.println("1. Add Employee");
            System.out.println("2. View Employees");
            System.out.println("3. Back to Main Menu");
            System.out.print("Enter choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1:
                    addEmployee(sc);
                    break;
                case 2:
                    viewEmployees();
                    break;
                case 3:
                    break;
                default:
                    System.out.println("Invalid choice.");
            }
        } while (choice != 3);
    }

    static void addEmployee(Scanner sc) {
        try {
            System.out.print("Enter ID: ");
            int id = sc.nextInt();
            sc.nextLine();
            System.out.print("Enter Name: ");
            String name = sc.nextLine();
            System.out.print("Enter Salary: ");
            double salary = sc.nextDouble();

            Employee emp = new Employee(id, name, salary);
            List<Employee> employees = readAllEmployees();
            employees.add(emp);
            writeAllEmployees(employees);
            System.out.println("Employee added.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void viewEmployees() {
        List<Employee> employees = readAllEmployees();
        if (employees.isEmpty()) {
            System.out.println("No employee records found.");
        } else {
            for (Employee e : employees) {
                e.display();
            }
        }
    }

    static List<Employee> readAllEmployees() {
        List<Employee> employees = new ArrayList<>();
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(EMPLOYEE_FILE))) {
            employees = (List<Employee>) ois.readObject();
        } catch (FileNotFoundException e) {
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
        return employees;
    }

    static void writeAllEmployees(List<Employee> employees) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(EMPLOYEE_FILE))) {
            oos.writeObject(employees);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
