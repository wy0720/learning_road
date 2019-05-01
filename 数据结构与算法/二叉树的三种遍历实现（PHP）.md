### 二叉树的三种遍历实现（PHP）

* 二叉树的三种遍历：先序遍历，中序遍历，后序遍历

* 深度优先遍历：

  前序遍历：根结点 ---> 左子树 ---> 右子树

  中序遍历：左子树---> 根结点 ---> 右子树

  后序遍历：左子树 ---> 右子树 ---> 根结点

* 广度优先遍历：

  层次遍历：仅仅需按层次遍历就可以（从左往右）

面向对象的递归版实现突然感觉好简单，捂脸。。。。。

```php
class Btree
{
	public $lChild = null;
	public $data;
	public $rChild = null;

	public function __construct($data)
	{
		$this->data = $data;
	}

	public function binaryTree($lChild, $rChild){

		if ($lChild) {
			# code...
			$this->lChild = $lChild;
		}

		if ($rChild) {
			# code...
			$this->rChild = $rChild;
		}
	}

	/**
	 * 先序遍历
	 *
	 * @param      <type>  $tree   The tree
	 */
	public function preTravel($tree)
	{
		echo "当前值为：".$tree->data;
		if ($tree->lChild) {
			$this->preTravel($tree->lChild);
		}

		if ($tree->rChild) {
			$this->preTravel($tree->rChild);
		}
	}

	/**
	 * 中序遍历
	 *
	 * @param      <type>  $tree   The tree
	 */
	public function centTravel($tree)
	{
		if ($tree->lChild) {
			$this->centTravel($tree->lChild);
		}

		echo '当前值为：'.$tree->data;

		if ($tree->rChild) {
			$this->centTravel($tree->rChild);
		}
	}

	/**
	 * 后序遍历
	 *
	 * @param      <type>  $tree   The tree
	 */
	public function lastTravel($tree)
	{
		if ($tree->lChild) {
			$this->lastTravel($tree->lChild);
		}

		if ($tree->rChild) {
			$this->lastTravel($tree->rChild);
		}

		echo '当前值为：'.$tree->data;
	}
    
    /**
	 * 层次遍历
	 *
	 * @param      <type>  $tree   The tree
	 */
	public function leavelTravel($tree)
	{
		// $node = [$tree->data];/
		$queue = [];
		array_push($queue, $tree);

		while ($node = array_shift($queue)) {
			echo '当前值为：'.$node->data;
			if ($node->lChild) {
				array_push($queue, $node->lChild);
			}

			if ($node->rChild) {
				array_push($queue, $node->rChild);
			}
		}
	}
}

$d = new Btree('D'); 
$e = new Btree('E'); 
$f = new Btree('F'); 
$c = new Btree('C'); 
$b = new Btree('B'); 
$a = new Btree('A'); 
$a->binaryTree($b,$c); 
$b->binaryTree($d,$e); 
$c->binaryTree(NULL,$f);

var_dump($a);//		A
/**

		A
	B		C
  D	  E		  F

*/				

// $a->preTravel($a);//a,b,d,e,c,f
// $a->centTravel($a);//d,b,e,a,c,f
$a->lastTravel($a);//d,e,b,f,c,a
$a->leavelTravel($a);
```

