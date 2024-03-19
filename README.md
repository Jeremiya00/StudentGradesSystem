using System;

class Student
{	//member variables
    private string name;
    private int[] grades;
    private int numSubjects;

    public Student(string name, int numSubjects)
    {
        this.name = name;
        this.numSubjects = numSubjects;
        grades = new int[numSubjects];
    }

    public void SetGrade(int subjectIndex, int grade)
    {
        if (subjectIndex >= 0 && subjectIndex < numSubjects)
        {
            grades[subjectIndex] = grade;
        }
        else
        {
            Console.WriteLine("Invalid subject index.");
        }
    }

    public double GetAverageGrade()
    {
        double sum = 0;
        foreach (int grade in grades)
        {
            sum += grade;
        }
        return sum / numSubjects;
    }

    public string GetName()
    {
        return name;
    }

    public int[] GetGrades()
    {
        return grades;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Welcome to the Student Grades System");
        Console.Write("Enter the total number of students: ");
        int totalStudents = Convert.ToInt32(Console.ReadLine());

        Student[] students = new Student[totalStudents];
        int choice;

        do
        {
            Console.WriteLine("\nMenu:");
            Console.WriteLine("[1] Enroll Students");
            Console.WriteLine("[2] Enter Student Grades");
            Console.WriteLine("[3] Show Student Grades");
            Console.WriteLine("[4] Show Top Student");
            Console.WriteLine("[5] Exit");

            Console.Write("Enter your choice[1-5]: ");
            choice = Convert.ToInt32(Console.ReadLine());

            if (choice == 1)
            {
                EnrollStudents(students, totalStudents);
            }
            else if (choice == 2)
            {
                EnterStudentGrades(students);
            }
            else if (choice == 3)
            {
                ShowStudentGrades(students);
            }
            else if (choice == 4)
            {
                ShowTopStudent(students);
            }
            else if (choice == 5)
            {
                Console.WriteLine("Exiting program...");
            }
            else
            {
                Console.WriteLine("Invalid choice. Please enter a number between 1 and 5.");
            }
        } while (choice != 5);
    }

    static void EnrollStudents(Student[] students, int totalStudents)
    {
        for (int i = 0; i < totalStudents; i++)
        {
            if (students[i] == null)
            {
                Console.Write("Enter student name: ");
                string name = Console.ReadLine();
                students[i] = new Student(name, 3); // assuming 3 subjects for simplicity

                if (i < totalStudents - 1)
                {
                    Console.Write("Enter Again [Y/N]: ");
                    char choice = Console.ReadLine().ToUpper()[0];
                    if (choice != 'Y')
                        break;
                }
            }
            else
            {
                Console.WriteLine("Cannot enroll more students. Maximum number of students reached.");
                break;
            }
        }
    }

    static void EnterStudentGrades(Student[] students)
    {
        foreach (var student in students)
        {
            if (student != null)
            {
                Console.WriteLine($"\nEnter grades for {student.GetName()}:");
                for (int i = 0; i < student.GetGrades().Length; i++)
                {
                    Console.Write($"Enter grade for subject {i + 1}: ");
                    int grade = Convert.ToInt32(Console.ReadLine());
                    student.SetGrade(i, grade);
                }
                Console.Write("Enter Again [Y/N]: ");
                char choice = Console.ReadLine().ToUpper()[0];
                if (choice != 'Y')
                    break;
            }
        }
    }

    static void ShowStudentGrades(Student[] students)
    {
        Console.WriteLine("\nStudent Grades");
        Console.WriteLine("Name\t\tScience\t\tMath\t\tEnglish\t\tAverage");
        foreach (var student in students)
        {
            if (student != null)
            {
                Console.WriteLine($"{student.GetName()}\t\t" +
                    $"{student.GetGrades()[0]}\t\t" +
                    $"{student.GetGrades()[1]}\t\t" +
                    $"{student.GetGrades()[2]}\t\t" +
                    $"{student.GetAverageGrade()}");
            }
        }
    }

    static void ShowTopStudent(Student[] students)
    {
        Student topStudent = null;
        double topAverage = 0;

        foreach (var student in students)
        {
            if (student != null)
            {
                double avg = student.GetAverageGrade();
                if (avg > topAverage)
                {
                    topAverage = avg;
                    topStudent = student;
                }
            }
        }

        if (topStudent != null)
        {
            Console.WriteLine($"\nTop Student: {topStudent.GetName()}, Average Grade: {topAverage}");
        }
        else
        {
            Console.WriteLine("No students enrolled yet.");
        }
    }
}
