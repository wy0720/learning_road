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

* common.php中的 &get_config方法实现

  在config.php中定义

  ```php
  <?php
  $config['intem1'] = 'asdf';
  $config['intem2'] = 'asdfas';
  ```

  在common.php中

  ````php
  if(!function_exists('get_config')){
      function &get_config(Array $replace = array()){
          static $config;
          //判断运行环境,一顿判断....
          ....
          inlude_once 'config.php';
          foreach($replace as $key=>$value){
              $config[$key] = $value;
          }
          
          return $config;//这里就将config配置都导出来了
      }
  }

  ````

* 在loader中有这样一段精彩的代码

  ```php
  protected function _ci_load($_ci_data)
  	{
  		// Set the default data variables
  		foreach (array('_ci_view', '_ci_vars', '_ci_path', '_ci_return') as $_ci_val)
  		{
  			$$_ci_val = isset($_ci_data[$_ci_val]) ? $_ci_data[$_ci_val] : FALSE;
  		}

  		$file_exists = FALSE;

  		// Set the path to the requested file
  		if (is_string($_ci_path) && $_ci_path !== '')
  		{
  			$_ci_x = explode('/', $_ci_path);
  			$_ci_file = end($_ci_x);
  		}
      ....
  }
  ```

  这里乍一看$_ci_path在方法中并没有定义，实则在foreach中的$$$_ci_val已经定义了,相当于下面这段代码

  ```php
  function demo (){
  	foreach (array('view','controller','model') as $key => $value) {
  		$$value = $key;
  	}
  	var_dump($view);
  	var_dump($controller);
  	var_dump($model);
  }
  demo();

  D:\demo\test\test.php:18:int 0
  D:\demo\test\test.php:19:int 1
  D:\demo\test\test.php:20:int 2
  ```

  ​