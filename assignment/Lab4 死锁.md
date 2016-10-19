# Lab4 死锁



## 1.死锁停留的截图（第41次）

![](http://i1.piimg.com/567571/2aade3452376eff9.png)



## 2.死锁的四个必要条件

- 互斥条件：一个资源每次只能被一个进程使用
- 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放
- 不剥夺条件：进程已获得的资源，在未使用完之前，不能强行剥夺
- 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系



## 3.上述程序产生死锁的解释

当我们执行Deadlock.java文件的时候，会调用构造函数

```java
class Deadlock implements Runnable{
	A a=new A();
	B b=new B();

	Deadlock(){
		Thread t = new Thread(this);
		int count = 2000;
		
		t.start();
		while(count-->0);
		a.methodA(b);
	}
	public void run(){
		b.methodB(a);
	}
	
	public static void main(String args[]){
		new Deadlock();
	}
}
```

从Deadlock()中可以看出，t.start()的时候执行当前线程，此线程会调用run()函数，从而调用B类中的methodB(a)函数，此函数用synchronized修饰，表示能够保证在同一时刻最多只有一个线程执行该段代码，若该线程运行此函数，则阻塞其他synchronized方式访问的函数执行。

```java
class B{
	synchronized void methodB(A a){
		a.last();
	}
	
	synchronized void last(){
		System.out.println("Inside B.last()");
	}

}
```

同样，对于A类，同样有synchronized修饰函数。

```java
class A{
	synchronized void methodA(B b){
		b.last();
	}
	
	synchronized void last(){
		System.out.println("Inside A.last()");
	}

}
```

所以在Deadlock函数运行到count减为0时，可能线程优先级相同，实行轮转法运行，可能时间片用完了，就退出cpu，而没有释放自己的资源，但是此时methodA被调用，函数中调用b.last函数，但last函数也是synchronized修饰的， 而调用methodA的时候需要调用b.last()函数，但是B的线程还没有结束，所以A被阻塞，同时B运行的时候也需要A的函数运行，所以造成了循环死锁的情况。

```java
Deadlock(){
		Thread t = new Thread(this);
		int count = 2000;
		
		t.start();
		while(count-->0);
		a.methodA(b);
	}
```

