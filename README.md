import java.util.ArrayList;
import java.util.Scanner;

 Model Classes
class Patient {
    int id;
    String name;
    int age;
    String disease;

    public Patient(int id, String name, int age, String disease) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.disease = disease;
    }

    @Override
    public String toString() {
        return "Patient ID: " + id + ", Name: " + name + ", Age: " + age + ", Disease: " + disease;
    }
}

class Doctor {
    int id;
    String name;
    String specialization;

    public Doctor(int id, String name, String specialization) {
        this.id = id;
        this.name = name;
        this.specialization = specialization;
    }

    @Override
    public String toString() {
        return "Doctor ID: " + id + ", Name: " + name + ", Specialization: " + specialization;
    }
}

class Appointment {
    int id;
    Patient patient;
    Doctor doctor;

    public Appointment(int id, Patient patient, Doctor doctor) {
        this.id = id;
        this.patient = patient;
        this.doctor = doctor;
    }

    @Override
    public String toString() {
        return "Appointment ID: " + id + "\n" +
                "Patient: " + patient.name + "\n" +
                "Doctor: " + doctor.name;
    }
}

// Main Class
public class HospitalManagementSystem {
    private static ArrayList<Patient> patients = new ArrayList<>();
    private static ArrayList<Doctor> doctors = new ArrayList<>();
    private static ArrayList<Appointment> appointments = new ArrayList<>();
    private static int patientCounter = 1;
    private static int doctorCounter = 1;
    private static int appointmentCounter = 1;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("\n--- Hospital Management System ---");
            System.out.println("1. Manage Patients");
            System.out.println("2. Manage Doctors");
            System.out.println("3. Schedule Appointment");
            System.out.println("4. List Appointments");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1 -> managePatients(scanner);
                case 2 -> manageDoctors(scanner);
                case 3 -> scheduleAppointment(scanner);
                case 4 -> listAppointments();
                case 5 -> {
                    System.out.println("Exiting system...");
                    return;
                }
                default -> System.out.println("Invalid choice! Please try again.");
            }
        }
    }

    private static void managePatients(Scanner scanner) {
        System.out.println("\n--- Manage Patients ---");
        System.out.println("1. Add Patient");
        System.out.println("2. View All Patients");
        System.out.print("Enter your choice: ");
        int choice = scanner.nextInt();

        switch (choice) {
            case 1 -> {
                scanner.nextLine(); // Consume leftover newline
                System.out.print("Enter name: ");
                String name = scanner.nextLine();
                System.out.print("Enter age: ");
                int age = scanner.nextInt();
                scanner.nextLine(); // Consume leftover newline
                System.out.print("Enter disease: ");
                String disease = scanner.nextLine();
                patients.add(new Patient(patientCounter++, name, age, disease));
                System.out.println("Patient added successfully!");
            }
            case 2 -> {
                if (patients.isEmpty()) {
                    System.out.println("No patients available.");
                } else {
                    System.out.println("\n--- List of Patients ---");
                    for (Patient patient : patients) {
                        System.out.println(patient);
                    }
                }
            }
            default -> System.out.println("Invalid choice! Please try again.");
        }
    }

    private static void manageDoctors(Scanner scanner) {
        System.out.println("\n--- Manage Doctors ---");
        System.out.println("1. Add Doctor");
        System.out.println("2. View All Doctors");
        System.out.print("Enter your choice: ");
        int choice = scanner.nextInt();

        switch (choice) {
            case 1 -> {
                scanner.nextLine(); // Consume leftover newline
                System.out.print("Enter name: ");
                String name = scanner.nextLine();
                System.out.print("Enter specialization: ");
                String specialization = scanner.nextLine();
                doctors.add(new Doctor(doctorCounter++, name, specialization));
                System.out.println("Doctor added successfully!");
            }
            case 2 -> {
                if (doctors.isEmpty()) {
                    System.out.println("No doctors available.");
                } else {
                    System.out.println("\n--- List of Doctors ---");
                    for (Doctor doctor : doctors) {
                        System.out.println(doctor);
                    }
                }
            }
            default -> System.out.println("Invalid choice! Please try again.");
        }
    }

    private static void scheduleAppointment(Scanner scanner) {
        System.out.println("\n--- Schedule Appointment ---");

        if (patients.isEmpty()) {
            System.out.println("No patients available. Please add patients first.");
            return;
        }
        if (doctors.isEmpty()) {
            System.out.println("No doctors available. Please add doctors first.");
            return;
        }

        System.out.print("Enter Patient ID: ");
        int patientId = scanner.nextInt();
        System.out.print("Enter Doctor ID: ");
        int doctorId = scanner.nextInt();

        Patient patient = patients.stream().filter(p -> p.id == patientId).findFirst().orElse(null);
        Doctor doctor = doctors.stream().filter(d -> d.id == doctorId).findFirst().orElse(null);

        if (patient == null || doctor == null) {
            System.out.println("Invalid Patient or Doctor ID!");
            return;
        }

        appointments.add(new Appointment(appointmentCounter++, patient, doctor));
        System.out.println("Appointment scheduled successfully!");
    }

    private static void listAppointments() {
        System.out.println("\n--- List of Appointments ---");
        if (appointments.isEmpty()) {
            System.out.println("No appointments available.");
        } else {
            for (Appointment appointment : appointments) {
                System.out.println(appointment);
            }
        }
    }
}
