# EXP :  04 - Spring-Boot-with-REST-API-and-Hibernate-Integration
## Name: Yogesh D

## Reg.no: 212224040371

## AIM:
To develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it via REST APIs.

## ALGORITHM:
Create Spring Boot project with dependencies:

Spring Web

Spring Data JPA

H2 or MySQL Database

Configure application.properties with DB connection and JPA settings.

Create Movie entity with fields like id, title, genre, rating, and year.

Create MovieRepository interface extending JpaRepository.

Create MovieController to define REST endpoints for CRUD operations:

GET /movies

GET /movies/{id}

POST /movies

PUT /movies/{id}

DELETE /movies/{id}


## PROGRAM CODE (Main Files):

### application.properties
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/moviedb
spring.datasource.username=root
spring.datasource.password=root123

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

server.port=8081
```

### pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
		 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

	<modelVersion>4.0.0</modelVersion>

	<!-- Parent -->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.5.0</version>
		<relativePath/>
	</parent>

	<!-- Project Details -->
	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demo</name>
	<description>Spring Boot REST API with Hibernate</description>

	<!-- Java Version -->
	<properties>
		<java.version>17</java.version>
	</properties>

	<!-- Dependencies -->
	<dependencies>

		<!-- Spring Boot Web -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<!-- Spring Boot JPA -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>

		<!-- H2 Database -->
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>

		<!-- Spring Boot Test -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

	</dependencies>

	<!-- Build -->
	<build>
		<plugins>

			<!-- Spring Boot Maven Plugin -->
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>

		</plugins>
	</build>

</project>
```

### Movie.java

```java
package com.example.movie.entity;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Movie {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String director;
    private int year;
    private double rating;

    public Movie() {
    }

    public Movie(String title, String director, int year, double rating) {
        this.title = title;
        this.director = director;
        this.year = year;
        this.rating = rating;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getDirector() {
        return director;
    }

    public void setDirector(String director) {
        this.director = director;
    }

    public int getYear() {
        return year;
    }

    public void setYear(int year) {
        this.year = year;
    }

    public double getRating() {
        return rating;
    }

    public void setRating(double rating) {
        this.rating = rating;
    }
}
```
### MovieRepository.java

```java
package com.example.movie.repository;

import com.example.movie.entity.Movie;
import org.springframework.data.jpa.repository.JpaRepository;

public interface MovieRepository extends JpaRepository<Movie, Long> {
}
```

### MovieController.java

```java
package com.example.movie.controller;

import com.example.movie.entity.Movie;
import com.example.movie.repository.MovieRepository;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/movies")
public class MovieController {

    private final MovieRepository movieRepository;

    public MovieController(MovieRepository movieRepository) {
        this.movieRepository = movieRepository;
    }

    @GetMapping
    public List<Movie> getAllMovies() {
        return movieRepository.findAll();
    }

    @GetMapping("/{id}")
    public Movie getMovieById(@PathVariable Long id) {
        return movieRepository.findById(id).orElse(null);
    }

    @PostMapping
    public Movie addMovie(@RequestBody Movie movie) {
        return movieRepository.save(movie);
    }

    @PutMapping("/{id}")
    public Movie updateMovie(@PathVariable Long id,
                             @RequestBody Movie movie) {

        Movie existingMovie = movieRepository.findById(id).orElse(null);

        if (existingMovie != null) {
            existingMovie.setTitle(movie.getTitle());
            existingMovie.setDirector(movie.getDirector());
            existingMovie.setYear(movie.getYear());
            existingMovie.setRating(movie.getRating());

            return movieRepository.save(existingMovie);
        }

        return null;
    }

    @DeleteMapping("/{id}")
    public String deleteMovie(@PathVariable Long id) {

        movieRepository.deleteById(id);

        return "Movie deleted successfully";
    }
}
```

### MovieApiApplication.java
```java
package com.example.movie;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MovieApplication {

	public static void main(String[] args) {
		SpringApplication.run(MovieApplication.class, args);
	}

}
```

### Output

<img width="1917" height="1017" alt="image" src="https://github.com/user-attachments/assets/0830b486-9188-4e75-b92a-6aef50037d80" />

<img width="1917" height="1145" alt="image" src="https://github.com/user-attachments/assets/e117f953-b752-411c-b89d-b772c108df88" />


<img width="1917" height="1150" alt="image" src="https://github.com/user-attachments/assets/43b913af-a235-480c-aac1-f3f49ef5abe8" />

<img width="1917" height="1143" alt="image" src="https://github.com/user-attachments/assets/5618b629-242d-4b13-8e12-f35b8ad0ab89" />

<img width="1917" height="1143" alt="image" src="https://github.com/user-attachments/assets/e2079689-0fed-4061-840e-98e385ad1a65" />



### Result
Thus, the Spring Boot application using Hibernate and REST APIs was successfully developed and executed for performing CRUD operations on the Movies database.
The application successfully stores, retrieves, updates, and deletes movie records using Spring Data JPA and H2/MySQL database.
