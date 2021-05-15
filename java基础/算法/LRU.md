### LRU    Least Recently used

缓存

用户信息当然是存在数据库里。但是由于我们对用户系统的性能要求比较高，显然不能每一次请求都去查询数据库。


所以，小灰在内存中创建了一个哈希表作为缓存，每次查找一个用户的时候先在哈希表中查询，以此提高访问性能。

![](../pics/LRU1.png)


内存溢出

用户量太大，Hash表把内存撑爆了


LRU:长期不被使用的数据，在未来使用到的数据的可能性也不大

当缓存数据占内存一定阈值时，移除掉最少被使用的数据

java 中的 LinkedHashMap   

Redis


leetcode:


    LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

	cache.put(1, 1);
	cache.put(2, 2);
	cache.get(1);       // 返回  1
	cache.put(3, 3);    // 该操作会使得密钥 2 作废
	cache.get(2);       // 返回 -1 (未找到)
	cache.put(4, 4);    // 该操作会使得密钥 1 作废
	cache.get(1);       // 返回 -1 (未找到)
	cache.get(3);       // 返回  3
	cache.get(4);       // 返回  4


	class LRUCache {

	    class LinkedNode{
	        private int key;
	        private int value;
	        LinkedNode pre;
	        LinkedNode next;
	        LinkedNode(){}
	        LinkedNode(int key,int value){
	            this.key=key;
	            this.value=value;
	        }
	        
	    }
	
	    HashMap<Integer,LinkedNode> map=new HashMap<>();
	    private int size;
	    private int capacity;
	    private LinkedNode head=new LinkedNode();
	    private LinkedNode tail=new LinkedNode();
	
	
	    public LRUCache(int capacity) {
	        this.capacity=capacity;
	        this.size=0;
	        head.next=tail;
	        tail.pre=head;
	
	    }
	    
	    public int get(int key) {
	        LinkedNode node=map.get(key);
	        if(node==null)  return -1;
	        moveNode(node);
	        return node.value;
	
	    }
	    
	    public void put(int key, int value) {
	        LinkedNode node=map.get(key);
	        if(node!=null){
	            node.value=value;
	            moveNode(node);
	        }else{
	            LinkedNode newNode=new LinkedNode(key,value);
	            map.put(key,newNode);
	            size++;
	            addToHead(newNode);
	            if(size>capacity){
	                LinkedNode last=removeLast();
	                map.remove(last.key);
	                size--;
	            }
	        }
	    }
	
	    public void moveNode(LinkedNode node){
	        removeNode(node);
	        addToHead(node);
	    }
	
	    public void addToHead(LinkedNode node){
	        node.next=head.next;
	        node.pre=head;
	        head.next=node;
	        node.next.pre=node;
	    }
	
	    public void removeNode(LinkedNode node){
	        node.pre.next=node.next;
	        node.next.pre=node.pre;
	    }
	
	    public LinkedNode removeLast(){
	        LinkedNode node=tail.pre;
	        node.pre.next=tail;
	        tail.pre=node.pre;
	        return node;
	    }
}

