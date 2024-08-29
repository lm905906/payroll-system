# payroll-system





package com.nit.lab36;

//Employee.java
class Employee {
    private String employeeName;
    private int employeeId;
    private double basicSalary;
    private double HRAPer;
    private double DAPer;

    public Employee(String employeeName, int employeeId, double basicSalary, double HRAPer, double DAPer) {
        this.employeeName = employeeName;
        this.employeeId = employeeId;
        this.basicSalary = basicSalary;
        this.HRAPer = HRAPer;
        this.DAPer = DAPer;
    }

    public String getEmployeeName() {
        return employeeName;
    }

    public int getEmployeeId() {
        return employeeId;
    }

    public double getBasicSalary() {
        return basicSalary;
    }

    public double getHRAPer() {
        return HRAPer;
    }

    public double getDAPer() {
        return DAPer;
    }

    public double calculateGrossSalary() {
        return 0.0;
    }

    @Override
    public String toString() {
        return "Employee Name: " + employeeName + ", Employee ID: " + employeeId;
    }

}

// Devloper.java

class Developer extends Employee {

    public Developer(String employeeName, int employeeId, double basicSalary, double HRAPer, double DAPer) {
        super(employeeName, employeeId, basicSalary, HRAPer, DAPer);
    }

    @Override
    public double calculateGrossSalary() {
        return getBasicSalary() + getHRAPer() + getDAPer();
    }

    // here we use super.toString() because it is printing the super class
    // properties and along with that we have to print subclass properties in this
    // class
    @Override
    public String toString() {

        return super.toString() + ", Gross Salary: " + calculateGrossSalary();
    }
}

// Manager.java

class Manager extends Employee {
    private double projectAllowance;

    public Manager(String employeeName, int employeeId, double basicSalary, double HRAPer, double DAPer,
            double projectAllowance) {
        super(employeeName, employeeId, basicSalary, HRAPer, DAPer);
        this.projectAllowance = projectAllowance;
    }

    public double getProjectAllowance() {
        return projectAllowance;
    }

    @Override
    public double calculateGrossSalary() {
        return getBasicSalary() + getHRAPer() + getDAPer() + projectAllowance;
    }

    @Override
    public String toString() {
        return super.toString() + ", Gross Salary: " + calculateGrossSalary();
    }
}

// Trainer.java

class Trainer extends Employee {
    private int batchCount;
    private double perkPerBatch;

    public Trainer(String employeeName, int employeeId, double basicSalary, double HRAPer, double DAPer, int batchCount,
            double perkPerBatch) {
        super(employeeName, employeeId, basicSalary, HRAPer, DAPer);
        this.batchCount = batchCount;
        this.perkPerBatch = perkPerBatch;
    }

    public int getBatchCount() {
        return batchCount;
    }

    public double getPerkPerBatch() {
        return perkPerBatch;
    }

    @Override
    public double calculateGrossSalary() {
        return getBasicSalary() + getHRAPer() + getDAPer() + (batchCount * perkPerBatch);
    }

    @Override
    public String toString() {
        return super.toString() + ", Gross Salary: " + calculateGrossSalary();
    }
}

//Sourcing.java

class Sourcing extends Employee {
    private int enrollmentTarget;
    private int enrollmentReached;
    private double perkPerEnrollment;

    public Sourcing(String employeeName, int employeeId, double basicSalary, double HRAPer, double DAPer,
            int enrollmentTarget, int enrollmentReached, double perkPerEnrollment) {
        super(employeeName, employeeId, basicSalary, HRAPer, DAPer);
        this.enrollmentTarget = enrollmentTarget;
        this.enrollmentReached = enrollmentReached;
        this.perkPerEnrollment = perkPerEnrollment;
    }

    public int getEnrollmentTarget() {
        return enrollmentTarget;
    }

    public int getEnrollmentReached() {
        return enrollmentReached;
    }

    public double getPerkPerEnrollment() {
        return perkPerEnrollment;
    }

    @Override
    public double calculateGrossSalary() {
        double percentage = (double) enrollmentReached / enrollmentTarget * 100;
        return getBasicSalary() + getHRAPer() + getDAPer() + (percentage * perkPerEnrollment);
    }

    @Override
    public String toString() {
        return super.toString() + ", Gross Salary: " + calculateGrossSalary();
    }
}

// TaxUtil.java

/*class TaxUtil {

    public double calculateTax(Developer e) {
        double grossSalary = e.calculateGrossSalary();
        return grossSalary > 50000 ? grossSalary * 0.20 : grossSalary * 0.05;
    }
    public double calculateTax(Trainer e) {
        double grossSalary = e.calculateGrossSalary();
        return grossSalary > 50000 ? grossSalary * 0.20 : grossSalary * 0.05;
    }
   
    public double calculateTax(Manager e) {
        double grossSalary = e.calculateGrossSalary();
        return grossSalary > 50000 ? grossSalary * 0.20 : grossSalary * 0.05;
    }
    public double calculateTax(Sourcing e) {
        double grossSalary = e.calculateGrossSalary();
        return grossSalary > 50000 ? grossSalary * 0.20 : grossSalary * 0.05;
    }
}*/

// this is what we developed but it's not recommended why or every object we are
// creating that same method again and again

// so for better programming practice we have take superclass as a parameter of
// the calculate tax which can accept all subclass object
// that method is called as polymerphic method
// this type of devlopment is called Loosely coupled runtime Polymerphism(LCRP)
// TaxUtil.java
class TaxUtil {

    public double calculateTax(Employee e) {
        double grossSalary = e.calculateGrossSalary();
        return grossSalary > 50000 ? grossSalary * 0.20 : grossSalary * 0.05;
    }
}

// TaxCalculation.java [ELC Class]

public class TaxCalculation {
    public static void main(String[] args) {
        Developer dev = new Developer("Virat", 10, 55000, 5000, 4000);
        Manager mgr = new Manager("Dhoni", 11, 65000, 3000, 1000, 8000);
        Trainer trainer = new Trainer("Rohit", 12, 10000, 5000, 3000, 3, 1500);
        Sourcing sourcing = new Sourcing("Hardik", 13, 20000, 9000, 3000, 50, 40, 100);

        TaxUtil taxUtil = new TaxUtil();

        System.out.println(dev + ", Tax: " + taxUtil.calculateTax(dev));
        System.out.println(mgr + ", Tax: " + taxUtil.calculateTax(mgr));
        System.out.println(trainer + ", Tax: " + taxUtil.calculateTax(trainer));
        System.out.println(sourcing + ", Tax: " + taxUtil.calculateTax(sourcing));
    }
}                                       
