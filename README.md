
# Step By Step Create SImple Rest API

## 1. Setup Database      

##### 1.1 Create daus_api_db    
* Go to your phpmyadmin    
* Click New Database    
* Set daus_api_db    
* Create    
	
##### 1.2 Create Tables     

* Create Categories Table    
	```sql
	CREATE TABLE IF NOT EXISTS `categories` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(256) NOT NULL,
	  `description` text NOT NULL,
	  `created` datetime NOT NULL,
	  `modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=19 ;

	```    
* Create Products Table    
	```sql    
	CREATE TABLE IF NOT EXISTS `products` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(32) NOT NULL,
	  `description` text NOT NULL,
	  `price` decimal(10,0) NOT NULL,
	  `category_id` int(11) NOT NULL,
	  `created` datetime NOT NULL,
	  `modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB  DEFAULT CHARSET=latin1 AUTO_INCREMENT=65 ;
	```     

##### 1.3  Dump Data For Tables     

* Dump Data Table Categories        
	```sql     
		INSERT INTO `categories` (`id`, `name`, `description`, `created`, `modified`) VALUES
		(1, 'Fashion', 'Category for anything related to fashion.', '2014-06-01 00:35:07', '2014-05-30 17:34:33'),
		(2, 'Electronics', 'Gadgets, drones and more.', '2014-06-01 00:35:07', '2014-05-30 17:34:33'),
		(3, 'Motors', 'Motor sports and more', '2014-06-01 00:35:07', '2014-05-30 17:34:54'),
		(5, 'Movies', 'Movie products.', '0000-00-00 00:00:00', '2016-01-08 13:27:26'),
		(6, 'Books', 'Kindle books, audio books and more.', '0000-00-00 00:00:00', '2016-01-08 13:27:47'),
		(13, 'Sports', 'Drop into new winter gear.', '2016-01-09 02:24:24', '2016-01-09 01:24:24');

	```     

* Dump Data Table Products     
	```sql
		INSERT INTO `products` (`id`, `name`, `description`, `price`, `category_id`, `created`, `modified`) VALUES
		(1, 'LG P880 4X HD', 'My first awesome phone!', '336', 3, '2014-06-01 01:12:26', '2014-05-31 17:12:26'),
		(2, 'Google Nexus 4', 'The most awesome phone of 2013!', '299', 2, '2014-06-01 01:12:26', '2014-05-31 17:12:26'),
		(3, 'Samsung Galaxy S4', 'How about no?', '600', 3, '2014-06-01 01:12:26', '2014-05-31 17:12:26'),
		(6, 'Bench Shirt', 'The best shirt!', '29', 1, '2014-06-01 01:12:26', '2014-05-31 02:12:21'),
		(7, 'Lenovo Laptop', 'My business partner.', '399', 2, '2014-06-01 01:13:45', '2014-05-31 02:13:39'),
		(8, 'Samsung Galaxy Tab 10.1', 'Good tablet.', '259', 2, '2014-06-01 01:14:13', '2014-05-31 02:14:08'),
		(9, 'Spalding Watch', 'My sports watch.', '199', 1, '2014-06-01 01:18:36', '2014-05-31 02:18:31'),
		(10, 'Sony Smart Watch', 'The coolest smart watch!', '300', 2, '2014-06-06 17:10:01', '2014-06-05 18:09:51'),
		(11, 'Huawei Y300', 'For testing purposes.', '100', 2, '2014-06-06 17:11:04', '2014-06-05 18:10:54'),
		(12, 'Abercrombie Lake Arnold Shirt', 'Perfect as gift!', '60', 1, '2014-06-06 17:12:21', '2014-06-05 18:12:11'),
		(13, 'Abercrombie Allen Brook Shirt', 'Cool red shirt!', '70', 1, '2014-06-06 17:12:59', '2014-06-05 18:12:49'),
		(26, 'Another product', 'Awesome product!', '555', 2, '2014-11-22 19:07:34', '2014-11-21 20:07:34'),
		(28, 'Wallet', 'You can absolutely use this one!', '799', 6, '2014-12-04 21:12:03', '2014-12-03 22:12:03'),
		(31, 'Amanda Waller Shirt', 'New awesome shirt!', '333', 1, '2014-12-13 00:52:54', '2014-12-12 01:52:54'),
		(42, 'Nike Shoes for Men', 'Nike Shoes', '12999', 3, '2015-12-12 06:47:08', '2015-12-12 05:47:08'),
		(48, 'Bristol Shoes', 'Awesome shoes.', '999', 5, '2016-01-08 06:36:37', '2016-01-08 05:36:37'),
		(60, 'Rolex Watch', 'Luxury watch.', '25000', 1, '2016-01-11 15:46:02', '2016-01-11 14:46:02');

	```      

## 2. Connect Database
##### 2.1 Create database.php
* Create new file named database.php    
	```php     
		<?php
		class Database{
		 
		    // specify your own database credentials
		    private $host = "localhost";
		    private $db_name = "api_db";
		    private $username = "root";
		    private $password = "";
		    public $conn;
		 
		    // get the database connection
		    public function getConnection(){
		 
		        $this->conn = null;
		 
		        try{
		            $this->conn = new PDO("mysql:host=" . $this->host . ";dbname=" . $this->db_name, $this->username, $this->password);
		            $this->conn->exec("set names utf8");
		        }catch(PDOException $exception){
		            echo "Connection error: " . $exception->getMessage();
		        }
		 
		        return $this->conn;
		    }
		}
		?>
	```     

* Save to your htdocs folder    

## 3. Read Database    
##### 3.1 Read Table    
* **Read Table Products**    
	* Create "Product.php" file    
		```PHP      
		<?php
		class Product{
		 
		    // database connection and table name
		    private $conn;
		    private $table_name = "products";
		 
		    // object properties
		    public $id;
		    public $name;
		    public $description;
		    public $price;
		    public $category_id;
		    public $category_name;
		    public $created;
		 
		    // constructor with $db as database connection
		    public function __construct($db){
		        $this->conn = $db;
		    }
		}
			```       
	* Create "read.php" file
		```PHP    
		<?php
		// required headers
		header("Access-Control-Allow-Origin: *");
		header("Content-Type: application/json; charset=UTF-8");
		 
		// include database and object files
		include_once 'database.php';
		include_once 'product.php';
		 
		// instantiate database and product object
		$database = new Database();
		$db = $database->getConnection();
		 
		// initialize object
		$product = new Product($db);
		 
		// query products
		$stmt = $product->read();
		$num = $stmt->rowCount();
		 
		// check if more than 0 record found
		if($num>0){
		 
		    // products array
		    $products_arr=array();
		    $products_arr["records"]=array();
		 
		    // retrieve our table contents
		    // fetch() is faster than fetchAll()
		    // http://stackoverflow.com/questions/2770630/pdofetchall-vs-pdofetch-in-a-loop
		    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)){
		        // extract row
		        // this will make $row['name'] to
		        // just $name only
		        extract($row);
		 
		        $product_item=array(
		            "id" => $id,
		            "name" => $name,
		            "description" => html_entity_decode($description),
		            "price" => $price,
		            "category_id" => $category_id,
		            "category_name" => $category_name
		        );
		 
		        array_push($products_arr["records"], $product_item);
		    }
		 
		    echo json_encode($products_arr);
		}
		 
		else{
		    echo json_encode(
		        array("message" => "No products found.")
		    );
		}
		?>
		```    
	* Add Product "read()" method inside "product.php"    
		```PHP    
		// read products
		function read(){
		 
		    // select all query
		    $query = "SELECT
		                c.name as category_name, p.id, p.name, p.description, p.price, p.category_id, p.created
		            FROM
		                " . $this->table_name . " p
		                LEFT JOIN
		                    categories c
		                        ON p.category_id = c.id
		            ORDER BY
		                p.created DESC";
		 
		    // prepare query statement
		    $stmt = $this->conn->prepare($query);
		 
		    // execute query
		    $stmt->execute();
		 
		    return $stmt;
		}
		```
* **Read Table Categories**    
	* Create "category.php"   
		```PHP
			<?php
			class Category{
			 
			    // database connection and table name
			    private $conn;
			    private $table_name = "categories";
			 
			    // object properties
			    public $id;
			    public $name;
			    public $description;
			    public $created;
			 
			    public function __construct($db){
			        $this->conn = $db;
			    }
			 
			    // used by select drop-down list
			    public function readAll(){
			        //select all data
			        $query = "SELECT
			                    id, name, description
			                FROM
			                    " . $this->table_name . "
			                ORDER BY
			                    name";
			 
			        $stmt = $this->conn->prepare( $query );
			        $stmt->execute();
			 
			        return $stmt;
			    }
			}
			?>
		```    
		* Create "read_category.php" file    
		```PHP
			<?php
			// required header
			header("Access-Control-Allow-Origin: *");
			header("Content-Type: application/json; charset=UTF-8");
			 
			// include database and object files
			include_once 'database.php';
			include_once 'category.php';
			 
			// instantiate database and category object
			$database = new Database();
			$db = $database->getConnection();
			 
			// initialize object
			$category = new Category($db);
			 
			// query categorys
			$stmt = $category->read();
			$num = $stmt->rowCount();
			 
			// check if more than 0 record found
			if($num>0){
			 
			    // products array
			    $categories_arr=array();
			    $categories_arr["records"]=array();
			 
			    // retrieve our table contents
			    // fetch() is faster than fetchAll()
			    // http://stackoverflow.com/questions/2770630/pdofetchall-vs-pdofetch-in-a-loop
			    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)){
			        // extract row
			        // this will make $row['name'] to
			        // just $name only
			        extract($row);
			 
			        $category_item=array(
			            "id" => $id,
			            "name" => $name,
			            "description" => html_entity_decode($description)
			        );
			 
			        array_push($categories_arr["records"], $category_item);
			    }
			 
			    echo json_encode($categories_arr);
			}
			 
			else{
			    echo json_encode(
			        array("message" => "No products found.")
			    );
			}
			?>
		```    
	* Add Category "read()" method inside "category.php"    
		```PHP   
			// used by select drop-down list
			public function read(){
			 
			    //select all data
			    $query = "SELECT
			                id, name, description
			            FROM
			                " . $this->table_name . "
			            ORDER BY
			                name";
			 
			    $stmt = $this->conn->prepare( $query );
			    $stmt->execute();
			 
			    return $stmt;
			}
		```

	##### 4. Output
