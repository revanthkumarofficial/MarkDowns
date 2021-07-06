# SPRING DATA AND SPRING DATA JPA

##### application.properties
```
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=test

spring.jpa.show-sql=true
```

<hr>

##### ID GENERATORS

__AUTO STRATEGY:__
```
HIBERNATE GO AND  TALK TO THE DATABASE FIGURE OUT WHAT IT SUPPORTS AND GENERATE PRIMARY KEY ID BASED ON DATABASE

SEQUENCE : ALMOST ALL DATABASE SUPPORTS SEQUENCE EXCEPT MYSQL
```

__IDENTITY STRATEGY:__
```
create table employee(
	id int PRIMARY KEY AUTO_INCREMENT,
	name varchar(20)
);

drop table employee;

select * from employee;

@Entity
public class Employee {

	
	@Id
	//@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	private String name;


// GETTERS AND SETTERS

}
```


__TABLE STRATEGY:__
```
create table employee(
	id int PRIMARY KEY AUTO_INCREMENT,
	name varchar(20)
);

drop table employee;

select * from employee;

delete from employee;

//  gen-name is the sequence name everytime a sequence gets generated that is a unique name that will be assigned ////  by the table generator
//  gen_val will be the value that is used from the employee table primary key id

create table id_gen(
	gen_name vaarchar(60) PRIMARY KEY
	gen_val int(20)
);

drop table id_gen;

select * from id_gen;

@Entity
public class Employee {

	@TableGenerator(name = "employee_gen", table = "id_gen", pkColumnName = "gen_name", valueColumnName = "gen_val",allocationSize=100)
	@GeneratedValue(strategy = GenerationType.TABLE,generator="employee_gen")
	@Id
	private Long id;
	private String name;


// GETTERS AND SETTERS

}
```

__CUSTOM RANDOM ID GENERATOR:__
```
public class CustomRandomIDGenerator implements IdentifierGenerator {


	@Override
	public Serializable generate(SessionImplementor si, Object obj) throws HibernateException {
		Random random = null;
		int id =0;
		random = new Random();
		id = random.nextInt(1000000);
		return new Long(id);
	}

}

// CONFIGURE AND USE CUSTOM RANDOM ID GENERATOR

@Entity
public class Employee {

	
	@GenericGenerator(name="emp_id",strategy="com.bharath.springdata.idgenerators.CustomRandomIDGenerator")
	@GeneratedValue(generator="emp_id")	
	@Id
	private Long id;
	private String name;

    // GETTERS AND SETTERS
	}

__REPO:__
public interface EmployeeRepository extends CrudRepository<Employee, Long> {

}
```

<br>

#### ASSOCIATIONS

### ONE TO ONE 
```
ENTITIES

@Entity
public class Person {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	private String firstName;
	private String lastName;
	private int age;
	@OneToOne(mappedBy = "person")
	private License license;
       // GETTERS AND SETTERS
}


@Entity
public class License {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	private String type;
	@Temporal(TemporalType.DATE)
	private Date validFrom;
	@Temporal(TemporalType.DATE)
	private Date validTo;
	@OneToOne(cascade = CascadeType.ALL)
	@JoinColumn(name="person_id")
	private Person person;
         // GETTERS AND SETTERS
	}



__REPO__ 
public interface LicenseRepository extends CrudRepository<License, Long> {

}
```

#### ONE TO MANY
```
@Entity
public class Customer {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private long id;
	private String name;
	@OneToMany(mappedBy = "customer", cascade = CascadeType.ALL,fetch=FetchType.EAGER)
	private Set<PhoneNumber> numbers;

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Set<PhoneNumber> getNumbers() {
		return numbers;
	}

	public void setNumbers(Set<PhoneNumber> numbers) {
		this.numbers = numbers;
	}

	public void addPhoneNumber(PhoneNumber number) {
		if (number != null) {
			if (numbers == null) {
				numbers = new HashSet<>();
			}
			number.setCustomer(this);
			numbers.add(number);
		}

	}

}

@Entity
public class PhoneNumber {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private long id;
	private String number;
	private String type;

	@ManyToOne
	@JoinColumn(name = "customer_id")
	private Customer customer;

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getNumber() {
		return number;
	}

	public void setNumber(String number) {
		this.number = number;
	}

	public String getType() {
		return type;
	}

	public void setType(String type) {
		this.type = type;
	}

	public Customer getCustomer() {
		return customer;
	}

	public void setCustomer(Customer customer) {
		this.customer = customer;
	}

}

REPO
public interface CustomerRepository extends CrudRepository<Customer, Long> {

}
```

#### MANY TO MANY
```
@Entity
public class Programmer {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private int id;
	private String name;
	@Column(name = "salary")
	private int sal;
	@ManyToMany(cascade = CascadeType.ALL,fetch=FetchType.EAGER)
	@JoinTable(name = "programmers_projects", joinColumns = @JoinColumn(name = "programmer_id", referencedColumnName = "id"), inverseJoinColumns = @JoinColumn(name = "project_id", referencedColumnName = "id"))
	private Set<Project> projects;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getSal() {
		return sal;
	}

	public void setSal(int sal) {
		this.sal = sal;
	}

	public Set<Project> getProjects() {
		return projects;
	}

	public void setProjects(Set<Project> projects) {
		this.projects = projects;
	}

	@Override
	public String toString() {
		return "Programmer [id=" + id + ", name=" + name + ", sal=" + sal + "]";
	}

}


@Entity
public class Project {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private int id;
	private String name;
	@ManyToMany(mappedBy = "projects")
	private Set<Programmer> programmers;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Set<Programmer> getProgrammers() {
		return programmers;
	}

	public void setProgrammers(Set<Programmer> programmers) {
		this.programmers = programmers;
	}

	@Override
	public String toString() {
		return "Project [id=" + id + ", name=" + name + "]";
	}

}

REPO

public interface ProgrammerRepository extends CrudRepository<Programmer, Integer> {

}
```

#### COMPONENT MAPPING
```
@Entity
public class Employee {

	@Id
	private int id;
	private String name;
	@Embedded
	private Address address;

	// GETTERS AND SETTERS
}

@Embeddable
public class Address {

	private String streetaddress;
	private String city;
	private String state;
	private String zipcode;
	private String country;

// GETTERS AND SETTERS

}

public interface EmployeeRepository extends CrudRepository<Employee, Integer> {

}
```

<hr>

#### PAGING AND SORTING :
```
CRUDRepository <- PagingAndSortingRepository

Pageable <- PageRequest

Sorting 
1. Sort
2. Direction
3. Order
```
<hr>


##### EMBEDDED
```
@Entity
public class Customer {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private int id;
	private String name;
	private String email;
	@Embedded
	private Address address;

	// GETTERS AND SETTERS

	public Address getAddress() {
		return address;
	}

	public void setAddress(Address address) {
		this.address = address;
	}

	@Override
	public String toString() {
		return "Customer [id=" + id + ", name=" + name + ", email=" + email + "]";
	}
	}

@Embeddable
public class Address {

	private String streetAddress;
	private String city;
	private String state;
	private String zipcode;
	private String country;

	// GETTERS AND SETTERS

}


__REPO:__

public interface CustomerRepository extends CrudRepository<Customer, Integer> {

	List<Customer> findByNameAndEmail(String name, String email);

	List<Customer> findByEmailLike(String email);

	List<Customer> findByIdIn(List<Integer> ids);

	@Modifying
	@Query("update Customer cust set cust.email = :email where cust.id=:id")
	void updateEmail(@Param("id") int id, @Param("email") String email);
	}
```

<br>

##### HIBERNATE INHERITANCE:
```
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class Payment {

	@Id
	private int id;
	private double amount;

	// GETTERS AND SETTERS
	}

@Entity
@Table(name="card")
@PrimaryKeyJoinColumn(name="id")
public class CreditCard extends Payment{

	private String cardnumber;

	// GETTERA AND SETTERS
	}

@Entity
@Table(name = "bankcheck")
@PrimaryKeyJoinColumn(name = "id")
public class Check extends Payment {

	private String checknumber;

	// GETTERS AND SETTERS
	}

__REPO:__

public interface PaymentRepository extends CrudRepository<Payment, Integer> {

}
```
<br>

##### JPQL AND NATIVE SQL
```
@Entity
public class Student {
	
	@Id
	@GeneratedValue(strategy=GenerationType.AUTO)
	private Long id;
	@Column(name="fname")
	private String firstName;
	@Column(name="lname")
	private String lastName;
	private int score;

	// GETTERS AND SETTERS

	@Override
	public String toString() {
		return "Student [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", score=" + score + "]";
	}
	}

__REPO:__

public interface StudentRepository extends CrudRepository<Student, Long> {

	@Query("from Student")
	List<Student> findAllStudents(Pageable pageable);

	@Query("select st.firstName,st.lastName from Student st")
	List<Object[]> findAllStudentsPartialData();

	@Query("from Student where firstName=:firstName")
	List<Student> findAllStudentsByFirstName(@Param("firstName") String firstName);

	@Query("from Student where score>:min and score<:max")
	List<Student> findStudentsForGivenScores(@Param("min") int min, @Param("max") int max);

	@Modifying
	@Query("delete from Student where firstName = :firstName")
	void deleteStudentsByFirstName(@Param("firstName") String firstName);

	@Query(value = "select * from student", nativeQuery = true)
	List<Student> findAllStudentNQ();

	@Query(value = "select * from student where fname=:firstName", nativeQuery = true)
	List<Student> findByFirstNQ(@Param("firstName")String firstName);

}
```

##### PATIENTS SCHEDULING
```
@Entity
public class Doctor {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	private String firstName;
	private String lastName;
	private String speciality;

	@ManyToMany(mappedBy = "doctors")
	private List<Patient> patients;

	@OneToMany
	private List<Appointment> appointments;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public String getSpeciality() {
		return speciality;
	}

	public void setSpeciality(String speciality) {
		this.speciality = speciality;
	}

	@Override
	public String toString() {
		return "Doctor [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", speciality=" + speciality
				+ "]";
	}

	public List<Patient> getPatients() {
		return patients;
	}

	public void setPatients(List<Patient> patients) {
		this.patients = patients;
	}

	public List<Appointment> getAppointments() {
		return appointments;
	}

	public void setAppointments(List<Appointment> appointments) {
		this.appointments = appointments;
	}

}


@Entity
public class Patient {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	private String firstName;
	private String lastName;
	private String phone;
	@Embedded
	private Insurance insurance;

	@ManyToMany(fetch = FetchType.EAGER)
	@JoinTable(name = "patients_doctors", joinColumns = @JoinColumn(name = "patient_id", referencedColumnName = "id"), inverseJoinColumns = @JoinColumn(name = "doctor_id", referencedColumnName = "id"))
	private List<Doctor> doctors;

	@OneToMany
	private List<Appointment> appointments;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public String getPhone() {
		return phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public Insurance getInsurance() {
		return insurance;
	}

	public void setInsurance(Insurance insurance) {
		this.insurance = insurance;
	}

	@Override
	public String toString() {
		return "Patient [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", phone=" + phone
				+ ", insurance=" + insurance + "]";
	}

	public List<Doctor> getDoctors() {
		return doctors;
	}

	public void setDoctors(List<Doctor> doctors) {
		this.doctors = doctors;
	}

	public List<Appointment> getAppointments() {
		return appointments;
	}

	public void setAppointments(List<Appointment> appointments) {
		this.appointments = appointments;
	}

}


@Entity
public class Appointment {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	private Timestamp appointmentTime;
	private boolean started;
	private boolean ended;
	private String reason;

	@ManyToOne
	private Patient patient;

	@ManyToOne
	private Doctor doctor;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public Timestamp getAppointmentTime() {
		return appointmentTime;
	}

	public void setAppointmentTime(Timestamp appointmentTime) {
		this.appointmentTime = appointmentTime;
	}

	public boolean isStarted() {
		return started;
	}

	public void setStarted(boolean started) {
		this.started = started;
	}

	public boolean isEnded() {
		return ended;
	}

	public void setEnded(boolean ended) {
		this.ended = ended;
	}

	public String getReason() {
		return reason;
	}

	public void setReason(String reason) {
		this.reason = reason;
	}

	@Override
	public String toString() {
		return "Appointment [id=" + id + ", appointmentTime=" + appointmentTime + ", started=" + started + ", ended="
				+ ended + ", reason=" + reason + "]";
	}

	public Patient getPatient() {
		return patient;
	}

	public void setPatient(Patient patient) {
		this.patient = patient;
	}

	public Doctor getDoctor() {
		return doctor;
	}

	public void setDoctor(Doctor doctor) {
		this.doctor = doctor;
	}

}


@Embeddable
public class Insurance {
	
	private String providerName;
	private double copay;

	public String getProviderName() {
		return providerName;
	}

	public void setProviderName(String providerName) {
		this.providerName = providerName;
	}

	public double getCopay() {
		return copay;
	}

	public void setCopay(double copay) {
		this.copay = copay;
	}

	@Override
	public String toString() {
		return "Insurance [providerName=" + providerName + ", copay=" + copay + "]";
	}
	

}

public interface AppointmentRepository extends CrudRepository<Appointment, Long> {

}

public interface DoctorRepository extends CrudRepository<Doctor, Long> {

}

public interface PatientRepository extends CrudRepository<Patient, Long> {

}
```

##### PRODUCT
```
@Entity
@Table
@Cache(usage = CacheConcurrencyStrategy.READ_ONLY)
public class Product implements Serializable{


	private static final long serialVersionUID = 1L;
	@Id
	private int id;
	private String name;
	@Column(name = "description")
	private String desc;
	private Double price;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getDesc() {
		return desc;
	}

	public void setDesc(String desc) {
		this.desc = desc;
	}

	public Double getPrice() {
		return price;
	}

	public void setPrice(Double price) {
		this.price = price;
	}
}

__REPOS:__

public interface ProductRepository extends CrudRepository<Product, Integer> {

	List<Product> findByName(String name);

	List<Product> findByNameAndDesc(String name, String desc);

	List<Product> findByPriceGreaterThan(Double price);

	List<Product> findByDescContains(String desc);

	List<Product> findByPriceBetween(Double price1, Double price2);

	List<Product> findByDescLike(String desc);
	
	List<Product> findByIdIn(List<Integer> ids);

}
```

##### TRANSACTION MANAGEMENT
```
@Entity
public class BankAccount {

	@Id
	private int accno;
	private String firstName;
	private String lastName;
	private int bal;

	// GETTERS AND SETTERS

	@Override
	public String toString() {
		return "BankAccount [accno=" + accno + ", firstName=" + firstName + ", lastName=" + lastName + ", bal=" + bal
				+ "]";
	}

}

__REPOS:__

public interface BankAccountRepository extends CrudRepository<BankAccount, Integer> {

}

__SERVICE LAYER__

public interface BankAccountService {

	
	void transfer(int amount) throws IOException;

}

@Service
public class BankAccountServiceImpl implements BankAccountService {

	@Autowired
	BankAccountRepository repository;

	@Override
	@Transactional(rollbackFor = Exception.class)
	public void transfer(int amount) throws IOException {

		BankAccount obamasAccount = repository.findOne(1);
		obamasAccount.setBal(obamasAccount.getBal() - amount);
		repository.save(obamasAccount);

		if (true) {
			throw new IOException();
		}

		BankAccount trumpsAccount = repository.findOne(2);
		trumpsAccount.setBal(trumpsAccount.getBal() + amount);
		repository.save(trumpsAccount);

	}

}


@SpringBootApplication
public class TransactionmanagementApplication {

	public static void main(String[] args) {
		SpringApplication.run(TransactionmanagementApplication.class, args);
	}
}
```




