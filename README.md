controller/StudentsController.java
```java
@RestController
@RequestMapping("/api/students")
public class StudentsController {

    @RequestMapping(value = "", method = RequestMethod.GET)
    public ResponseEntity<?> hello()
    {
        return new ResponseEntity<>("Hello World!", HttpStatus.OK);
    }

}
```
commit - hello world
#### swagger
```
		<dependency>
			<groupId>org.springdoc</groupId>
			<artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
			<version>2.5.0</version>           <!-- ☑ Check for latest -->
		</dependency>
```



####
config/OpenApiConfig.java
```java
public class OpenApiConfig {                        // CHANGED: class renamed
  @Bean
  public OpenAPI BaseOpenAPI() {                  // CHANGED: return type now OpenAPI
    return new OpenAPI()
            .info(new Info()
                    .title("Hands-On Basic API") // CHANGED: add whatever metadata you like
                    .version("v1"));
  }
}
```
http://localhost:8080/swagger-ui.html#
<br>
commit - with swagger
### START DOCKER
```
docker run -d -p 5432:5432 -v postgresdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postgres postgres
docker ps
docker logs [containerid]
```


```
docker-compose.yml
```
version: "3"
services:
db:
image: postgres
environment:
POSTGRES_PASSWORD: postgres
ports:
- 5432:5432
volumes:
- ./postgresdata:/var/lib/postgresql/data
privileged: true
```
REMEMBER TO KILL THE DOCKER THAT WAS RUNNING BEFORE AS IT IS USING THE SAME PORT (5432) <br>
```
docker compose up -d
<br>
commit - with docker compose
#### Spring DATA
```
		<dependency> <!-- ☑ Spring Data JPA -->
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>

		<dependency> <!-- ☑ DB  -->
			<groupId>org.postgresql</groupId>
			<artifactId>postgresql</artifactId>
			<scope>runtime</scope>
		</dependency>

		<dependency> <!-- ☑ DB Relations -->
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
			<version>6.1.5.Final</version>
		</dependency>
		
```
try to run app, will not load
<br>
application.properties:
```
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.datasource.driver-class-name=org.postgresql.Driver

#JPA properties

#enable sql printing
spring.jpa.show-sql = true  

#update means that the database schema will be updated automatically, without deleteing the data
spring.jpa.hibernate.ddl-auto = update

#telling hibernate to use the PostgreSQL dialect
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.application.name=basic
```
app will run - still no tables
<br>


model/Student.java
```java
@Entity
@Table(name="student")
public class Student implements Serializable {
  private static final long serialVersionUID = 1L;

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @Column(nullable = false, updatable = false)
  private Instant createdAt;

  @NotEmpty
  @Size(max = 60)
  private String fullname;

  private LocalDate birthDate;

  @Min(100)
  @Max(800)
  private Integer satScore;

  @DecimalMin("30")
  @DecimalMax("110")
  private Double graduationScore;

  @Size(max = 20)
  private String phone;

  @Size(max = 500)
  private String profilePicture;

  @PrePersist
  private void onCreate() {
    this.createdAt = Instant.now();
  }

}
```

