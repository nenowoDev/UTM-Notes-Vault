

## Java Database Connectivity DBC
- Load driver
- Establish connection
- Start JDBC statement
- Execute SQL statement
- Get Result
- Close Statement
- Close connection
```java
Class.forName("com.mysql.cj.jdbc.Driver");

Connection conn=DriverManager.getConnection("jdbc:mysql://localhost:3306/springdb","root","pass");

Statement stmt = conn.prepareStatement("DROP *");

stmt.executeQuery();

result

stmt.close();
conn.close();
```



## Hibernate ORM (object relation mapping)
- can access/manipulate database entities via java object
- implement java persistent API (JPA) to access/manage/persist data between object and relational database

### Architecture
- Configuration
	- mapping document/create config instance/read config file
	- representation of apps domain model
- Session Factory
	- factory of session
	- heavyweight
	- thread-safe
	- initialized only once (its a factory)
	- manage hibernate config
	- open session
- Session
	- lightweight , single threaded unit of work
	- wraps JDBC connection
	- perform database CRUD


### Role and relationships
- config : initiate hibernate setting
- session factory : create session based on config
- session : perform database operation

### extra
- Transaction management
	- set of operation as single unit of work
	- ensure consistency by roll back / commit when fail/success
- Hibernate Query language
	- object oriented query language
	- same like sql but for java objects
- Associate mapping
	- can define (one/many)-to-(one/many) between entities
	- @OneToOne
	- @ManyToOne
### Steps
- Add dependencies in pom.xml
```xml
<dependency>
	<groupId>org.hibernate</groupId>
	<artifactId>hibernate-core</artifactId>
	<version>5.6.15.Final</version>
</dependency>

<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-orm</artifactId>
	<version>5.3.30</version>
</dependency>

<dependency>
	<groupId>org.hibernate</groupId>
	<artifactId>hibernate-entitymanager</artifactId>
	<version>5.6.15.Final</version>
</dependency>
```
- Config in HibernateConfig.java
```java
@Configuration
@EnableWebMVC
@EnableTransactionManagement
@ComponentScan(basePackages={"config","controller","service","entity","util"})
public class HibernateConfig implements WebMvcConfigurer{

	@Bean
	public DataSource dataSource(){
		DriverManagerDataSource dataSource = new DriverManagerDataSource();
		
		dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
		dataSource.setUrl("jdbc:mysql://localhost:3306/db");
		dataSource.setUsername("");
		dataSource.setPassword("");
		return dataSource;
		
	}

	@Bean
	public LocalSessionFactoryBean sessionFactory(){
		LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
		
		sessionFactory.setDataSource(dataSource());
		sessionFactory.setPackageToScan("com.example.entity");
		
		Properties hibernateProperties = new Properties();
		hibernateProperties.setProperty("hibernate.dialect",
						 "org.hibernate.dialect.MySQL8Dialect");
		hibernateProperties.setProperty("hibernate.show_sql","true");
		hibernateProperties.setProperty("hibernate.hbm2ddl.auto","update");
		

		sessionFactory.setProperties(hibernateProperties);
		
		return sessionFactory;
	}

	@Bean
	public HibernateTransactionManager transactionManager(){
		return new HibernateTransactionManager(sessionFactory.getObject());
	}

}
```



- Entity class
```java
@Entity
@Table(name="user")
public class User{
	
	@Id   //Primary key
	@GeneratedValue(strategy=GenerationType.IDENTITY)  //auto-increment
	@Column(name="id")
	private int pKey;

	@Column(name="name",nullable="false",length="100",unique="false")
	private String name;
	
	@Column(name="age",nullable="true",unique="false")
	private int age;

	@OneToOne
	@JoinColumn(name="schoolID",referencedColumnName="id") // foreign key of school is here
	private School schoolId;

//CONSTRUCTORS, GETTERS, SETTERS
// ... 
}

public class School{
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;

	@OneToOne(mappedBy="user") //this mean there wont be any column,, this is kinda useless really , it means this is the non owning side
	private User user;
	
}
```


- DAO
```java
@Service
public class UserDAO{

	@AutoWired
	private SessionFactory sessionFactory;
	
	public void add(User u){
		sessionFactory.getCurrentSession()
		.save(u);
	}

	public User getById(int id){
		return
		sessionFactory.getCurrentSession()
		.get(User.class,id);
		
	}

	public List<User> getAll(){
		return
		sessionFactory.getCurrentSession()
		.createQuery("From user")
		.getResultList();
	}

	@Transactional
	public void update(int id,User u){
		User currentUser=getById(id);
		if(currentUser!=null){
			currentUser.
			.setName(u.getName())
			.setAge(u.getAge());

			sessionFactory.getCurrentSession()
			.merge(currentUser);
		}
	}

	public void delete(int id){
	
		User toDelete=getById(id);
		
		if(toDelete!=null)
			sessionFactory.getCurrentSession()
			.delete(toDelete)
		}
	}

}
```


- Controller invocation
```java
@Controller
@RequestMapping('/customer')
public String UserControl(){
	@AutoWired
	private UserDAO userdao;
	
	@RequestMapping('add')
	@ResponseBody
	public String Add(User user){
		userdao.add(user)
		return "user added";
	}
}
```