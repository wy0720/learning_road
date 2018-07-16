### CI框架

* ci框架中的$this->load->var()的解析

  参考：http://phpff.com/592.html

  ```php
  <?php
   
   class ci{
       
       public function __construct(){
           $this->load=new view();         
       }
        
   }
    
   class view{
      public function a(){
               
          echo 1;
      }
       
      public function b(){
               
          echo 2;
      }
   
   
   }
    
   $ci=new ci();
   $ci->load->a(); 
  //输出1
   $ci->load->b(); 
  //输出2
  ?>
  ```

  ​