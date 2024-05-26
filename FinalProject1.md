# FinalProject
Address Class :

public class Address {
    private int streetNo;
    private String street;
    private String city;
    private String province;
    private String postalCode;
    private String country;

    public Address(int streetNo, String street, String city, String province, String postalCode, String country) {
        if (isPostalCodeValid(postalCode)) {
            this.streetNo = streetNo;
            this.street = street;
            this.city = city;
            this.province = province;
            this.postalCode = postalCode;
            this.country = country;
        } else {
            // Invalidate all fields if postal code is not valid
            this.streetNo = 0;
            this.street = null;
            this.city = null;
            this.province = null;
            this.postalCode = null;
            this.country = null;
        }
    }

    public static boolean isPostalCodeValid(String postalCode) {
        postalCode = postalCode.replace(" ", "").toUpperCase();
        return postalCode.matches("[A-Z]\\d[A-Z]\\d[A-Z]\\d") || postalCode.matches("[A-Z]\\d[A-Z]\\s\\d[A-Z]\\d");
    }

    public int getStreetNo() {
        return streetNo;
        }
    public String getStreet() {
        return street;
        }
    public String getCity() {
        return city;
        }
    public String getProvince() {
        return province;
        }
    public String getPostalCode() {
        return postalCode;
        }
    public String getCountry() {
        return country;
        }

    public void setStreetNo(int streetNo) {
        this.streetNo = streetNo;
        }
    public void setStreet(String street) {
        this.street = street;
        }
    public void setCity(String city) {
        this.city = city;
        }
    public void setProvince(String province) {
        this.province = province;
        }
    public void setPostalCode(String postalCode) {
        this.postalCode = postalCode;
        }
    public void setCountry(String country) {
        this.country = country;
        }
}

Address Unit Test :

import org.junit.Test;
import static org.junit.Assert.*;

public class AddressTest {
    @Test
    public void testPostalCodeValidation() {
        assertTrue(Address.isPostalCodeValid("A1B2C3"));
        assertTrue(Address.isPostalCodeValid("A1B 2C3"));
        assertFalse(Address.isPostalCodeValid("123 ABC"));
        assertFalse(Address.isPostalCodeValid("A1 1B1"));
    }

    @Test
    public void testInvalidPostalCodeSetsNull() {
        Address address = new Address(123, "Main St", "Metropolis", "State", "123 ABC", "Country");
        assertNull(address.getStreet());
        assertNull(address.getCity());
        assertNull(address.getProvince());
        assertNull(address.getPostalCode());
        assertNull(address.getCountry());
    }
}

Department Class :

public class Department {
    private String departmentId;
    private String departmentName;
    private static int nextId = 1;

    public Department(String departmentName) {
        if (validateDepartmentName(departmentName)) {
            this.departmentId = "D" + nextId++;
            this.departmentName = departmentName;
        } else {
            this.departmentId = null;
            this.departmentName = null;
        }
    }

    public static boolean validateDepartmentName(String departmentName) {
        return departmentName.matches("[A-Za-z ]+");
    }

    public String getDepartmentId() {
        return departmentId;
        }
    public String getDepartmentName() {
        return departmentName;
        }

    public void setDepartmentName(String departmentName) {
        if (validateDepartmentName(departmentName)) {
            this.departmentName = departmentName;
        }
    }
}

Department Unit Test :

import org.junit.Test;
import static org.junit.Assert.*;

public class DepartmentTest {
    @Test
    public void testValidDepartmentName() {
        Department dept = new Department("History");
        assertNotNull(dept.getDepartmentName());
        assertEquals("History", dept.getDepartmentName());
    }

    @Test
    public void testInvalidDepartmentName() {
        Department dept = new Department("History123");
        assertNull(dept.getDepartmentName());
    }
}

Student Class :

import java.util.ArrayList;

public class Student {
    private String studentId;
    private String studentName;
    private Gender gender;
    private Address address;
    private Department department;
    private ArrayList<Course> registeredCourses;
    private static int nextId = 1;

    public Student(String studentName, Gender gender, Address address, Department department) {
        this.studentId = "S" + nextId++;
        this.studentName = studentName;
        this.gender = gender;
        this.address = address;
        this.department = department;
        this.registeredCourses = new ArrayList<>();
    }

    public boolean registerCourse(Course course) {
        if (!registeredCourses.contains(course)) {
            registeredCourses.add(course);
            course.addStudent(this); // Assuming there's a method in Course to add a student
            return true;
        }
        return false;
    }

    public boolean dropCourse(Course course) {
        if (registeredCourses.contains(course)) {
            registeredCourses.remove(course);
            course.removeStudent(this); // Assuming there's a method in Course to remove a student
            return true;
        }
        return false;
    }

    public String getStudentId() {
        return studentId;
        }
    public String getStudentName() {
        return studentName;
        }
    public Gender getGender() {
        return gender;
        }
    public Address getAddress() {
        return address;
        }
    public Department getDepartment() {
        return department;
        }
    public ArrayList<Course> getRegisteredCourses() {
        return registeredCourses;
        }

    public void setStudentName(String studentName) {
        this.studentName = studentName;
        }
    public void setGender(Gender gender) {
        this.gender = gender;
        }
    public void setAddress(Address address) {
        this.address = address;
        }
    public void setDepartment(Department department) {
        this.department = department;
        }
}

Student Unit Test:


import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.*;

public class StudentTest {
    private Student student;
    private Course course1;
    private Course course2;

    @Before
    public void setUp() {
        Address address = new Address(100, "Main St", "Springfield", "SP", "A1B2C3", "USA");
        Department department = new Department("Computer Science");
        student = new Student("John Doe", Gender.MALE, address, department);
        course1 = new Course("CSC101", "Introduction to Computer Science", 3.0, department);
        course2 = new Course("CSC102", "Data Structures", 3.0, department);
    }

    @Test
    public void testRegisterCourse() {
        assertTrue(student.registerCourse(course1));
        assertEquals(1, student.getRegisteredCourses().size());
        assertTrue(student.getRegisteredCourses().contains(course1));
        
        // Test registering the same course should fail
        assertFalse(student.registerCourse(course1));
    }

    @Test
    public void testDropCourse() {
        student.registerCourse(course1);
        assertTrue(student.dropCourse(course1));
        assertFalse(student.getRegisteredCourses().contains(course1));
        
        // Test dropping a course not registered should fail
        assertFalse(student.dropCourse(course2));
    }
}

Gender Enum :

public enum Gender {
    FEMALE, MALE
}

Gender Enum Unit Test:

import org.junit.Test;
import static org.junit.Assert.*;

public class GenderTest {
    @Test
    public void testEnumValues() {
        assertEquals("FEMALE", Gender.FEMALE.name());
        assertEquals("MALE", Gender.MALE.name());
    }
}

Util Class:

public class Util {
    public static String toTitleCase(String input) {
        String[] words = input.split(" ");
        StringBuilder titleCase = new StringBuilder();
        for (String word : words) {
            titleCase.append(Character.toUpperCase(word.charAt(0)))
                     .append(word.substring(1).toLowerCase())
                     .append(" ");
        }
        return titleCase.toString().trim();
    }
}

Util Class Unit Test:

import org.junit.Test;
import static org.junit.Assert.*;

public class UtilTest {
    @Test
    public void testToTitleCase() {
        assertNull(Util.toTitleCase(null));
        assertEquals("", Util.toTitleCase(""));
        assertEquals("Hello World", Util.toTitleCase("hello world"));
        assertEquals("Hello World", Util.toTitleCase("HELLO WORLD"));
        assertEquals("Hello World", Util.toTitleCase("hElLo wOrLd"));
        assertEquals("A B C", Util.toTitleCase("a b c"));
        assertEquals("Java Programming", Util.toTitleCase("java programming"));
        assertEquals("Java  Programming", Util.toTitleCase("java  programming")); // Note double space in middle
    }
}
