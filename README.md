# Lab10Web

# Praktikum 10: PHP OOP – Modularisasi, Class, dan Object

Repository ini berisi hasil pengerjaan **Praktikum 10 Pemrograman Web** dengan materi utama:

* Konsep dasar OOP (Object-Oriented Programming)
* Pembuatan Class dan Object
* Menggunakan Class Library (Form)
* Modularisasi program
* Koneksi database menggunakan Class Database

---

## Struktur Folder

```
lab10_php_oop/
│
├── mobil.php
├── form.php
├── form_input.php
├── database.php
├── config.php
└── README.md
```

---

## Praktikum: Class & Object (mobil.php)

Pada bagian ini dibuat class sederhana bernama **Mobil**, yang memiliki atribut dan method:

* Atribut: `warna`, `merk`, `harga`
* Method: constructor, `gantiWarna()`, `tampilWarna()`

Program kemudian membuat dua objek mobil dan menampilkan perubahan warnanya.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8495bd13-8ca6-402e-bfc6-5669a634372f" />


---

## Class Library (form.php)

File **form.php** berisi class **Form** yang digunakan untuk membuat form input secara dinamis.
Class ini memiliki fitur:

* Menambah field input (`addField()`)
* Menampilkan form (`displayForm()`)

Ini merupakan implementasi konsep **modularisasi**, di mana class dibuat terpisah agar bisa digunakan di file lain.


    <?php 
    /** 
    * Nama Class: Form 
    * Deskripsi: CLass untuk membuat form inputan text sederhan 
    **/ 
      
    class Form 
    { 
       private $fields = array(); 
       private $action; 
       private $submit = "Submit Form"; 
       private $jumField = 0; 
      
       public function __construct($action, $submit) 
       { 
           $this->action = $action; 
           $this->submit = $submit; 
       } 
      
       public function displayForm() 
       { 
           echo "<form action='".$this->action."' method='POST'>"; 
           echo '<table width="100%" border="0">'; 
           for ($j=0; $j<count($this->fields); $j++) { 
               echo "<tr><td 
    align='right'>".$this->fields[$j]['label']."</td>"; 
               echo "<td><input type='text' 
    name='".$this->fields[$j]['name']."'></td></tr>"; 
           } 
           echo "<tr><td colspan='2'>"; 
           echo "<input type='submit' value='".$this->submit."'></td></tr>"; 
           echo "</table>"; 
       } 
      
       public function addField($name, $label) 
       { 
           $this->fields [$this->jumField]['name'] = $name; 
           $this->fields [$this->jumField]['label'] = $label; 
           $this->jumField ++; 
       } 
    } 
    ?>

---


## Implementasi Form (form_input.php)
    
File **form_input.php** melakukan:
    
* `include "form.php"`
* Membuat objek Form
* Menambah field seperti NIM, Nama, Alamat
* Menampilkan form ke browser
    
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/482e925a-1ef9-4326-9d7b-fc3226a9b4a3" />


---

## Class Database (database.php)

File ini menunjukkan contoh bagaimana melakukan modularisasi untuk database.
Fungsinya:

* Koneksi ke database melalui `config.php`
* Query (`query()`)
* Insert (`insert()`)
* Update (`update()`)
* Delete (`delete()`)
* Get data (`get()`)

      <?php 
       
      class Database { 
         protected $host; 
         protected $user; 
         protected $password; 
         protected $db_name; 
         protected $conn; 
          
         public function __construct() { 
            $this->getConfig(); 
            $this->conn = new mysqli($this->host, $this->user, $this->password, 
      $this->db_name); 
            if ($this->conn->connect_error) { 
               die("Connection failed: " . $this->conn->connect_error); 
            } 
         } 
          
         private function getConfig() { 
            include_once("config.php"); 
            $this->host = $config['host']; 
            $this->user = $config['username']; 
            $this->password = $config['password']; 
            $this->db_name = $config['db_name']; 
         } 
          
         public function query($sql) { 
            return $this->conn->query($sql); 
         } 
          
         public function get($table, $where=null) { 
            if ($where) { 
                $where = " WHERE ".$where; 
            } 
            $sql = "SELECT * FROM ".$table.$where;           
            $sql = $this->conn->query($sql); 
            $sql = $sql->fetch_assoc(); 
            return $sql; 
         } 
          
         public function insert($table, $data) { 
            if (is_array($data)) { 
               foreach($data as $key => $val) { 
                  $column[] = $key; 
                  $value[] = "'{$val}'"; 
               } 
               $columns = implode(",", $column); 
               $values = implode(",", $value); 
            } 
            $sql = "INSERT INTO ".$table." (".$columns.") VALUES (".$values.")";         
            $sql = $this->conn->query($sql); 
            if ($sql == true) { 
               return $sql; 
            } else { 
               return false; 
            } 
         } 
          
         public function update($table, $data, $where) { 
            $update_value = ""; 
            if (is_array($data)) { 
               foreach($data as $key => $val) { 
                  $update_value[] = "$key='{$val}'" 
               } 
               $update_value = implode(",", $update_value); 
            } 
          
            $sql = "UPDATE ".$table." SET ".$update_value." WHERE ".$where;         
            $sql = $this->conn->query($sql); 
            if ($sql == true) { 
               return true; 
            } else { 
      return false; 
      } 
      } 
      public function delete($table, $filter) { 
      $sql =  "DELETE FROM ".$table." ".$filter;   
      $sql = $this->conn->query($sql); 
      if ($sql == true) { 
      return true; 
      } else { 
      return false; 
      } 
      } 
      } 
      ?>
  
---

## config.php

Berisi konfigurasi database:

```
$config = [
    'host' => 'localhost',
    'username' => 'root',
    'password' => '',
    'db_name' => 'nama_database_kamu'
];
```

---

## ▶ Cara Menjalankan

1. Simpan folder ke dalam:

   * XAMPP: `htdocs/lab10_php_oop/`

2. Start Apache (dan MySQL jika menguji database)

3. Jalankan di browser:

```
http://localhost/lab10_php_oop/mobil.php
http://localhost/lab10_php_oop/form_input.php
```

