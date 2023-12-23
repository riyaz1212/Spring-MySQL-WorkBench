**Spring Boot: Accessing data from MySql Workbench
**


Steps 1: Setup MySQL Database
Steps 2: Create a Spring Boot Project
Steps 3: Configuration: application.properties: starter-web, data-jpa, thymeleaf, devtools, mysql-connector
Steps 4: application.properties

spring.datasource.url=jdbc:mysql://localhost:3306/your_database_name
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

Steps 5: Create a Repository Interface:
JpaRepository: This interface will provide basic CRUD operations.

import org.springframework.data.jpa.repository.JpaRepository;

public interface YourEntityRepository extends JpaRepository<YourEntity, Long> {
    // Define custom queries if needed
}


Steps 6: Service Layer: Create a service layer to handle business logic, which interacts with the repository interfaces.

Steps 7: Repository :
@Service
public class YourEntityService {

    private final YourEntityRepository repository;

    @Autowired
    public YourEntityService(YourEntityRepository repository) {
        this.repository = repository;
    }

    public List<YourEntity> getAllEntities() {
        return repository.findAll();
    }

    // Other methods to manipulate data
}


Steps 8: Create .html file under templates
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    <title>Book Info</title>
  </head>
  <body>
    <div class="container">  
    <h1>Book Info</h1>
    <a href="#">Add New Book</a>
    
    
    <table class="table table-bordered">
	
	<thead>
		<tr>
			<th>Book Id</th>
			<th>Book Name</th>
			<th>Book Price</th>
			<th>Action</th>
		</tr>		
		</thead>
	
	<tbody>
		<tr th:each="book : ${books}"> 
		
		<td th:text="${book.book_Id}"></td>
		<td th:text="${book.book_Name}"></td>
		<td th:text="${book.book_Price}"></td>
		 	 
		 	 <td>
			<a href="#">Edit</a>
			<a href="#">Delete</a>
			
			 </td>
			
		</tr>
	</tbody>
	
	</table>
    
	</div>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.12.9/dist/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
  </body>
</html>	

Controller: 
package in.simplified.Controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.servlet.ModelAndView;

import in.simplified.entity.Book;
import in.simplified.service.BookService;

@Controller
public class BookController {
	
@Autowired	
private BookService service;

@GetMapping("/books")
public ModelAndView getBooks() {
	
	ModelAndView mav = new ModelAndView();
	List<Book> allBooks= service.getAllBooks();
	
	mav.addObject("books", allBooks);
	mav.setViewName("bookView");
	return mav;
}
}

entity: 

package in.simplified.entity;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Book {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	
	private Integer book_Id;
	private String book_Name;
	private Integer book_Price;
	public Integer getBook_Id() {
		return book_Id;
	}
	public void setBook_Id(Integer book_Id) {
		this.book_Id = book_Id;
	}
	public String getBook_Name() {
		return book_Name;
	}
	public void setBook_Name(String book_Name) {
		this.book_Name = book_Name;
	}
	public Integer getBook_Price() {
		return book_Price;
	}
	public void setBook_Price(Integer book_Price) {
		this.book_Price = book_Price;
	}
	
	
	

}

Repository: 
package in.simplified.repo;

import org.springframework.data.jpa.repository.JpaRepository;

import in.simplified.entity.Book;

public interface BookRepository extends JpaRepository<Book, Integer>{

}


Service: 
package in.simplified.service;

import java.util.List;

import in.simplified.entity.Book;

public interface BookService {
public List<Book> getAllBooks(); 

}


package in.simplified.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import in.simplified.entity.Book;
import in.simplified.repo.BookRepository;

@Service
public class BookServiceImpl implements BookService {

	@Autowired
	private BookRepository bookRepo;
	
	@Override
	public List<Book> getAllBooks() {


		return bookRepo.findAll();


				
	}

}


  [!CAUTION]
  Advises about risks or negative outcomes of certain actions.
