--------------------------------------------------------------
	Multithreading :
	----------------
Uniprocessing :- 
----------------
In uni-processing, only one process can occupy the memory So the
major drawbacks are 

1) Memory is wastage
2) Resources are wastage
3) CPU is idle

To avoid the above said problem, multitasking is introduced.

In multitasking multiple tasks can concurrently work with CPU so, our task will be completed as soon as possible.

Multitasking is further divided into two categories.

a) Process based Multitasking [Diagram : 10th Dec]
b) Thread based Multitasking  [Diagram : 10th Dec]

![image](https://github.com/user-attachments/assets/a1cb406c-d2a7-4b86-a723-6030291ac9e7)




Process based Multitasking :
----------------------------
If a CPU is switching from one subtask(Thread) of one process to another subtask of another process then it is called Process based Multitasking.

Thread based Multitasking :
---------------------------
If a CPU is switching from one subtask(Thread) to another subtask within the same process then it is called Thread based Multitasking.

What is Thread in java ?
-------------------------
A thread is light weight process and it is the basic unit of CPU which can run concurrently with another thread within the same context (process).

It is well known for independent execution. The main purpose of multithreading to boost the execution sequence.

A thread can run with another thread concurrently within the same process so our task will be completed as soon as possible.

In java whenever we define main method then JVM internally creates a thread called main thread under main group.

Program that describes that main is a Thread :
-----------------------------------------------
Whenever we define main method then JVM will create main thread internally under main group, the purpose of this main thread to execute the entire main method code.

In java there is a predefined class called Thread available in java.lang package, this class contains a predefined static factory method currentThread() which will provide currently executing Thread Object.

Thread t = Thread.currentThread(); //static Factory Method

Thread class has provided predefined method getName() to get the name of the Thread.
               
		 public final String getName();

   
	package com.ravi.mt;
	public class MainThread {
		public static void main(String[] args) 
		{
			//Thread obj = Thread.currentThread();
			//System.out.println("Current thread name is :"+obj.getName());
			                  //OR
			String name = Thread.currentThread().getName();
			System.out.println("Current thread name is :"+name);
		}
	}
--------------------------------------------------------------
How to create user-defined thread ?
-----------------------------------
We can create user-defined thread by using the following two packages 

  1) By using java.lang package [JDK 1.0]
  2) By using java.util.concurrent sub package [JDK 1.5]

Creating user-defind Thread by using java.lang package :
--------------------------------------------------------
By using java.lang package we can create user-defined thread by using any one of the following two approaches :

 1) By extending java.lang.Thread class
 2) By implementing java.lang.Runnable interface 

 Note :- Thread is a predefined class available in java.lang package where as Runnable is a predefined interface available in java.lang Package.
 -------------------------------------------------------------
 Creating user-defined Thread by using extending Thread class :
 --------------------------------------------------------------
 
	 package com.ravi.mt;
	
	class UserThread extends Thread
	{
		@Override
		public void run()
		{
			System.out.println("My user thread is running in a separate stack"); 
		}
	}
	
	public class MainThread {
	
		public static void main(String[] args) 
		{
			System.out.println("Main thread started");
			
			UserThread ut = new UserThread();
			ut.start();
			
			System.out.println(10/0);
			
			System.out.println("Main thread ended");
	
		}
	
	}


In the above program, we have two threads, main thread which is responsible to execute 
main method and Thread-0 thread which is responsible to execute run() method. [10-DEC-24]

In entire Multithreading concept start() is the only method which is responsible to create a new thread.
-----------------------------------------------------------------
public synchronized void start() :
-----------------------------------
start() is a predefined non static method of Thread class which internally performs the following two tasks :

1) It will make a request to the O.S to assign a new thread for concurrent execution.

2) It will implicitly call run() method on the current object.

--------------------------------------------------------------
public final boolean isAlive() :-
-----------------------------
It is a predefined non static method of Thread class through which we can find out whether a thread has started or not ?

As we know a new thread is created/started after calling start() method so if we use isAlive() method before start() method, it will return false but if the same isAlive() method if we invoke after the start() method, it will return true.

We can't restart a thread in java if we try to restart then It will generate an exception i.e java.lang.IllegalThreadStateException


IsAlive.java
--------------
	package com.ravi.basic;
	
	class Foo extends Thread
	{
		@Override
		public void run()
		{
			System.out.println("Child thread is running...");
			System.out.println("It is running with separate stack");		
		}	
	}
	public class IsAlive 
	{
		public static void main(String[] args) 
		{
			System.out.println("Main Thread started...");
			
			Foo f1 = new Foo();
			System.out.println("Is Thread alive : "+f1.isAlive());
			f1.start();
			System.out.println("Thread is alive or not : "+f1.isAlive());
			
			f1.start();  //java.lang.IllegralThreadStateException
			
			System.out.println("Main Thread ended...");
			
			
		}
	}
---------------------------------------------------------------
	package com.ravi.basic;
	
	class Stuff extends Thread
	{
		@Override
		public void run() 
		{		
			System.out.println("Child Thread is Running!!!!");
		}	
	}
	public class ExceptionDemo 
	{
		public static void main(String[] args)
		{		
			System.out.println("Main Thread Started");			
					
			Stuff s1 = new Stuff(); 
			Stuff s2 = new Stuff(); 		
					
			s1.start();
			s2.start();
			
			System.out.println(10/0);
			
			System.out.println("Main Thread Ended");
		}
	
	}
Note :- Here main thread is interrupted due to AE but still child thread will be executed because child threads are executing with separate Stack
--------------------------------------------------------------
		package com.ravi.basic;
		
		class Sample extends Thread
		{
			@Override
			public void run()
			{
				String name = Thread.currentThread().getName();
				
				for(int i=1; i<=10; i++)
				{
					System.out.println(i+" by "+name+" thread");
				}
				
				
			}
			
		}
		
		public class ThreadLoop 
		{	
			public static void main(String[] args) 
			{	
			    Sample s1 = new Sample();
			    s1.start();
			   
		        String name = Thread.currentThread().getName();
				
				for(int i=1; i<=10; i++)
				{
					System.out.println(i+" by "+name+" Thread");
				}
				
				int x = 1;
				do
				{
					System.out.println("Enjoy Multithreading by "+name+ " Thread");
					
					x++;
				}
				while(x<=10);
				
			}
		}

Note : Here processor is frequently switching from main thread to Thread-0 thread so output is un-predicatable
---------------------------------------------------------------

How to set and get the name of the Thread : 
--------------------------------------------------
Whenever we create a userdefined Thread in java then by default JVM assigns the name of thread is Thread-0, Thread-1, Thread-2 and so on.

If a user wants to assign some user defined name of the Thread, then Thread class has provided a predefined method called setName(String name) to set the name of the Thread.

On the other hand we want to get the name of the Thread then Thread class has provided a predefined method called getName().

public final void setName(String name)  //setter

public final String getName()  //getter

---------------------------------------------------------------
		package com.ravi.basic;
		class DoStuff extends Thread  
		{
			@Override
			public void run()
			{
				String name = Thread.currentThread().getName();
				System.out.println(name +" thread is running Here!!!!");
			}
		}
		public class ThreadName 
		{
			public static void main(String[] args) 
			{
				DoStuff t1 = new DoStuff(); 
				DoStuff t2 = new DoStuff(); 
				
				t1.start();			
				t2.start();	
				
					
			System.out.println(Thread.currentThread().getName()+" thread is running.....");
			}
		}

Note :- If we don't provide our user-defined name for the thread then by default the name would be Thread-0, Thread-1, Thread-2 and so on.
---------------------------------------------------------------
	package com.ravi.basic;
	class Demo extends Thread
	{
		@Override
		public void run()
		{
			 String name = Thread.currentThread().getName();
			 System.out.println("Running Thread name is :"+name);
		}
	}
	public class ThreadName1 
	{
		public static void main(String[] args) 
		{
		  Thread t =  Thread.currentThread();
		  t.setName("Parent"); //Changing the name of the main thread
		  
		   Demo d1 = new Demo();
		   Demo d2 = new Demo();
		   
		 
		   d1.setName("Child1");
		   d2.setName("Child2");	   
		   d1.start();  d2.start();
		   
		   
		   
		   String name = Thread.currentThread().getName();
		   System.out.println(name + " Thread is running Here..");
		}
	}
---------------------------------------------------------------
	package com.ravi.basic;
	
	import java.util.InputMismatchException;
	import java.util.Scanner;
	
	class BatchAssignment extends Thread
	{
		@Override
		public void run()
		{
			String name = Thread.currentThread().getName();
			
			if(name !=null && name.equalsIgnoreCase("Placement"))
			{
				this.placementBatch();
			}
			else if(name !=null && name.equalsIgnoreCase("Regular"))
			{
				this.regularBatch();
			}
			else
			{
				throw new NullPointerException("Name can't be null");
			}
		}
		
		public void placementBatch()
		{
			System.out.println("I am a placement batch student.");
		}
		
		public void regularBatch()
		{
			System.out.println("I am a Regular batch student.");
		}
	}
	
	
	public class ThreadName2 
	{
		public static void main(String[] args) 
		{
			Scanner sc = new Scanner(System.in);	
			try(sc)
			{
				System.out.print("Enter your Batch Title :");
				String title = sc.next();
				
				BatchAssignment b = new BatchAssignment();
				b.setName(title);
				
				b.start();
			}
			catch(InputMismatchException e)
			{
				System.out.println("Invalid Input");
			}
			
			
		}
	
	}
---------------------------------------------------------------
Thread.sleep(long millisecond) :
-------------------------------
If we want to put a thread into temporarly waiting state then we should use sleep() method.

The waiting time of the Thread depends upon the time specified by the user in millisecond as parameter to sleep() method.

It is a static method of Thread class.

Thread.sleep(1000); //Thread will wait for 1 second

It is throwing a checked Exception i.e InterruptedException because there may be chance that this sleeping thread may be interrupted by a thread so provide either try-catch or declare the method as throws.
--------------------------------------------------------------
	package com.ravi.basic;
	
	class Sleep extends Thread
	{
	   @Override
	   public void run()
	   {	 
		   
		   for(int i=1; i<=10; i++)
		   {
			   System.out.println("i value is :"+i);
			   try
			   {
				   Thread.sleep(1000);
			   }
			   catch(InterruptedException e)
			   {
				   System.err.println("Thread interrupted "+e);
			   }
		   }
	   }
	   
	}
	public class SleepDemo 
	{
		public static void main(String[] args)  
		{
		  	Sleep s = new Sleep();
		  	s.start(); 		  	
		  	  
		}
	}
---------------------------------------------------------------
	package com.ravi.basic;
	
	class MyTest extends Thread 
	{	
		
		@Override
		public void run()  
		{		
			System.out.println("Child Thread id is :"+Thread.currentThread().getId());  
			
			for(int i=1; i<=5; i++) 
			{
				System.out.println("i value is :"+i); // 11  22  33  44
				try
				{
					Thread.sleep(1000);				
				}
				catch(InterruptedException e)
				{
					System.err.println("Thread has Interrupted");
				}
			}
		}
	}
	public class SleepDemo1 
	{
		public static void main(String[] args) 
		{		
			System.out.println("Main Thread id is :"+Thread.currentThread().getId());  //1
			
			MyTest m1 = new MyTest();		
			MyTest m2 = new MyTest();
			
			m1.start();
			m2.start();			
		}
	}
---------------------------------------------------------------
	package com.ravi.basic;
	
	class MyThread1 extends Thread
	{
		@Override
		public void run()
		{
			System.out.println("Child Thread is running");
		}
	}
	
	public class SleepDemo2 {
	
		public static void main(String[] args) throws InterruptedException 
		{
			MyThread1 m1 = new MyThread1();
			m1.start();
			
			m1.sleep(2000); //current thread is main
			
			System.out.println("Main Thread is Running");
			
	
		}
	
	}

Note : Here main thread will go into the sleeping state.
--------------------------------------------------------------
Assignment :
--------------
Thread.sleep(long millis, int nanos)
---------------------------------------------------------------
Basic Thread life cycle :
-------------------
As we know a thread is well known for Independent execution and it contains a life cycle which internally contains 5 states (Phases). 

During the life cycle of a thread, It can pass from thses 5 states. At a time a thread can reside to only one state of the given 5 states.

	1) NEW State (Born state)
	
	2) RUNNABLE state (Ready to Run state) [Thread Pool]
	
	3) RUNNING state
	
	4) WAITING / BLOCKED state
	
	5) EXIT/Dead state

----------------------------------------------------------------------
New State :-
-------------
Whenever we create a thread instance(Thread Object) a thread comes to new state OR born state. New state does not mean that the Thread has started yet only the object or instance of Thread has been created.

Runnable state :-
-------------------
Whenever we call start() method on thread object, A thread moves to Runnable state i.e Ready to run state. Here Thread schedular is responsible to select/pick a particular Thread from Runnable state and sending that particular thread to Running state for execution.

Running state :-
-----------------
If a thread is in Running state that means the thread is executing its own run() method in a separate stack Memory.

From Running state a thread can move to waiting state either by an order of thread schedular or user has written some method(wait(), join() or sleep()) to put the thread into temporarly waiting state.

From Running state the Thread may also move to Runnable state directly, if user has written Thread.yield() method explicitly.

Waiting state :-
------------------
A thread is in waiting state means it is waiting for it's time period to complete. Once the time period will be completed then it will re-enter inside the Runnable state to complete its remaining task.

Dead or Exit :
----------------
Once a thread has successfully completed its run method in the corresponding stack then the thread will move to dead state. Please remember once a thread is dead we can't restart a thread in java.
---------------------------------------------------------------------
IQ :- If we write Thread.sleep(1000) then exactly after 1 sec the Thread will re-start?

Ans :- No, We can't say that the Thread will directly move from waiting state to Running state. 

The Thread will definetly wait for 1 sec in the waiting mode and then again it will re-enter into Runnable state which is control by Thread Schedular so we can't say that the Thread will re-start just after 1 sec.
---------------------------------------------------------------
join() method of Thread class :
-------------------------------
The main purpose of join() method to put the current thread into waiting state until the other thread finish its execution.

Here the currently executing thread stops its execution and the thread goes into the waiting state. The current thread remains in the wait state until the thread on which the join() method is invoked has achieved its dead state.

It also throws checked exception i.e InterruptedException so better to use try catch or declare the method as throws.

It is a non static method so we can call this method with the help of Thread object reference.

	package com.ravi.basic;
	
	class Join extends Thread
	{
		@Override
		public void run()
		{
			for(int i=1; i<=5; i++)
			{
				System.out.println("i value is :"+i);
				try
				{
					Thread.sleep(1000);
				}
				catch(InterruptedException e)
				{
					e.printStackTrace();
				}
			}
		}
	}
	
	public class JoinDemo 
	{
		public static void main(String[] args) throws InterruptedException 
		{
			System.out.println("Main Thread started");
			
			Join j1 = new Join();
			Join j2 = new Join();
			Join j3 = new Join();
			
			
			j1.start();
			
			j1.join(); //Here main thread will go to waiting state
					
			j2.start();
			j3.start();
			
			
			System.out.println("Main Thread ended");
		}
	
	}
---------------------------------------------------------------
13-12-2024
-----------
	package com.ravi.basic;
	
	class Alpha extends Thread   
	{
		@Override
		public void run()
		{
			Thread t = Thread.currentThread();
			String name = t.getName();	//Alpha_Thread is current thread		
			
			Beta b1 = new Beta();
			b1.setName("Beta_Thread");
	        b1.start();  
	        try 
	        {
				b1.join(); //Alpha thread is waiting 4 Beta Thread to complete
			
				System.out.println("Alpha thread re-started");
			} 
	        catch (InterruptedException e) 
	        {			
				e.printStackTrace();
			}
			
			for(int i=1; i<=10; i++)
			{
				System.out.println(i+" by "+name);
			}
			
		}
	}
	
	public class JoinDemo2 
	{
		public static void main(String[] args) 
		{
			Alpha a1 = new Alpha();
			a1.setName("Alpha_Thread");
			a1.start();
		}
	}
	
	class Beta extends Thread
	{
		@Override
		public void run()
		{
			Thread t = Thread.currentThread();
			String name = t.getName();	
			
			for(int i=1; i<=20; i++)
			{
				System.out.println(i+" by "+name);
				try
				{
					Thread.sleep(500);
				}
				catch(InterruptedException e) {
					
				}
			}
			System.out.println("Beta Thread Ended");
		}
	}

Note : Alpha Thread will wait till the completion of Beta Thread.
---------------------------------------------------------------
	package com.ravi.basic;
	
	public class JoinDemo1 
	{
		public static void main(String[] args) throws InterruptedException 
		{
	       System.out.println("Main Thread started");
	       
	       Thread t = Thread.currentThread();
	       
	       for(int i=1; i<=10; i++)
	       {
	    	   System.out.println(i+" by "+t.getName());
	       }
	       
	       t.join();  //Deadlock [Main thread is waiting 4 main thread to complete]
	           
	       System.out.println("Main Thread Ended");
		}
	}


Here, It is a deadlock state because main thread is waiting for main thread to complete.
---------------------------------------------------------------
Assignment :

join(long millis)
join(long millis, long nanos)
---------------------------------------------------------------
Anonymous inner class by using Thread class :
----------------------------------------------
 1) Anonymous inner class with reference variable :
 ---------------------------------------------------
	package com.ravi.anonymous_thread;
	
	public class AnonymousThreadWithReference 
	{
		public static void main(String[] args) 
		{
			Thread t1 = new Thread()
			{
				@Override
				public void run()
				{
					String name = Thread.currentThread().getName();
					System.out.println("Anonymous Thread name is :"+name);
				}
				
			};
			t1.start();
			
			
	
		}
	
	}

2) Anonymous inner class without reference variable :
-------------------------------------------------------
	package com.ravi.anonymous_thread;
	
	public class AnonymousWithoutReference {
	
		public static void main(String[] args) 
		{
			new Thread()
			{
			   @Override
			   public void run()
			   {
				   String name = Thread.currentThread().getName();
				   System.out.println("Running thread name is :"+name);
			   }
			}.start();
			
		}
	
	}
---------------------------------------------------------------
Assigning target by Runnable interface :[Loose Coupling]
----------------------------------------------------------
By using Runnable interface approach we can assign different target to our threads. [13-DEC-24]

	package com.ravi.basic;
	
	class Ravi implements Runnable
	{
		@Override
		public void run() 
		{
			System.out.println("Thread is performing my task ");		
		}	
	}
	
	public class RunnableDemo
	{
	   public static void main(String [] args)
	   {
		   System.out.println("Main");        
	        
	        Thread t1 = new Thread(new Ravi());
	        t1.start();
	       
	   }
	}   
--------------------------------------------------------------
Assign different target for different Threads :
-----------------------------------------------
	package com.ravi.basic;
	
	class Ravi implements Runnable
	{
		@Override
		public void run() 
		{
			System.out.println("Thread is performing my task ");		
		}	
	}
	
	public class RunnableDemo
	{
	   public static void main(String [] args)
	   {
		   System.out.println("Main");        
	        
	        Thread t1 = new Thread(new Ravi());
	        t1.start();
	       
	   }
	}   
---------------------------------------------------------------
Thread class constructor :
---------------------------
We have total 10 constructors in the Thread class, The following are commonly used constructor in the Thread class

	1) Thread t1 = new Thread();
	
	2) Thread t2 = new Thread(String name);
	
	3) Thread t3 = new Thread(Runnable target);
	
	4) Thread t4 = new Thread(Runnable target, String name);
	
	5) Thread t5 = new Thread(ThredGroup tg, String name);
	
	6) Thread t6 = new Thread(ThredGroup tg, Runnable target);
	
	7) Thread t7 = new Thread(ThredGroup tg, Runnable target, String name);
--------------------------------------------------------------
14-12-2024
-----------
	Anonymous inner class by using Runnable interface :
	----------------------------------------------------
	Case 1 :
	--------
	package com.ravi.anonymous_runnable;
	
	public class AnonymousDemo1 {
	
		public static void main(String[] args) 
		{
			Runnable r1 = new Runnable() 
			{			
				@Override
				public void run() 
				{
					String name = Thread.currentThread().getName();
					System.out.println("Current Thread Name is :"+name);
				}
			};
	
			Thread t1 = new Thread(r1);
			t1.start();
			
		}
	
	}
--------------------------------------------------------------
	Case 2:
	-------
	package com.ravi.anonymous_runnable;
	
	public class AnonymousDemo2 {
	
		public static void main(String[] args) 
		{
			Thread t1 = new Thread(new Runnable() 
			{			
				@Override
				public void run() 
				{
					String name = Thread.currentThread().getName();
					System.out.println("Current Thread Name is :"+name);
					
				}
			});
			
			t1.start();	
			
		}
	
	}
--------------------------------------------------------------
	Case 3 :
	--------
	package com.ravi.anonymous_runnable;
	
	public class AnonymousDemo3 {
	
		public static void main(String[] args) 
		{
			Thread t1 = new Thread(()-> System.out.println(Thread.currentThread().getName()));
			t1.start();
			
			System.out.println("................");
			
			new Thread(()-> System.out.println(Thread.currentThread().getName())).start();
			
			System.out.println("................");
			
			new Thread(()-> System.out.println(Thread.currentThread().getName()),"Scott").start();
			
		}
	
	}
--------------------------------------------------------------
Drawback of Multithreading :
----------------------------
Multithreading is very good to complete our task as soon as possible but in some situation, It provides some wrong data or wrong result.

In Data Race or Race condition, all the threads try to access the resource at the same time so the result may be corrupted.

In multithreading if we want to perform read operation and data is not updatable data then multithreading is good but if the data is updatable data (modifiable data) then multithreading may produce some wrong result or wrong data which is known as Data Race OR race condition as shown in the diagram.(14-DEC)
---------------------------------------------------------------
	package com.nit.testing;
	
	class Customer implements Runnable
	{
	   private int availableSeat = 1;   
	   private int wantedSeat;  //1
	   
		public Customer(int wantedSeat) 
		{
		  super();
		  this.wantedSeat = wantedSeat;
	    }
		
		@Override
		public synchronized void run() 
		{
			String name = null;	
			
			if(availableSeat >= wantedSeat)
			{
				name = Thread.currentThread().getName();
				System.out.println(wantedSeat+" seat is reserved for "+name);
				availableSeat = availableSeat - wantedSeat;			
			}
			else
			{
				name = Thread.currentThread().getName();
				System.err.println("Sorry!!"+name+" seat is not available");
			}
			
			
		}
		
	}
	
	public class RailwayReservation {
	
		public static void main(String[] args) throws InterruptedException 
		{
		   Customer c1 = new Customer(1);
		   
		   Thread t1 = new Thread(c1,"Scott");
		   Thread t2 = new Thread(c1,"Smith");
		   
		   t1.start();
		   
		  	   
		   t2.start();
	
		}
	
	}
--------------------------------------------------------------
	package com.ravi.multithreading_drawback;
	
	class Customer
	{
		private double availableBalance = 20000;	
		private double withdrawAmount;
		
		public Customer(double withdrawAmount) 
		{
			super();
			this.withdrawAmount = withdrawAmount;
		}
		
		public void withdraw()
		{
			String name = null;
			
			if(withdrawAmount<= availableBalance)
			{
				name = Thread.currentThread().getName();
				System.out.println(withdrawAmount+ " Amount, withdraw by :"+name);
				availableBalance = availableBalance - withdrawAmount;
			}
			else
			{
				name = Thread.currentThread().getName();
				System.err.println("Sorry!!"+name+" you have insufficient Balance");
			}		
		}
		
	}
	
	public class BankApplication {
	
		public static void main(String[] args) 
		{
			Customer obj = new Customer(20000);
			
			Runnable r1 = ()-> 
			{
			  obj.withdraw();
			};
			
			Thread t1 = new Thread(r1,"Scott");
			Thread t2 = new Thread(r1,"Smith");
			
			t1.start();  
				
			t2.start();
		}
	
	}

Note : Here both the thread will get the balance.
-----------------------------------------------------------
16-12-2024
-----------
***Synchronization :
-------------------
In order to solve the problem of multithreading java software people has introduced synchronization concept.

In order to acheive sycnhronization in java we have a keyword called "synchronized".

It is a technique through which we can control multiple threads but accepting only one thread at all the time.

Synchronization allows only one thread to enter inside the synchronized area for a single object.

Synchronization can be divided into two categories :-

1) Method level synchronization

2) Block level synchronization

1) Method level synchronization :-
-----------------------------------
In method level synchronization, the entire method gets synchronized so all the thread will wait at method level and only one thread will enter inside the synchronized area as shown in the diagram.(14-DEC-24)


2) Block level synchronization
-------------------------------
In block level synchronization the entire method does not get synchronized, only the part of the method gets synchronized so all the thread will enter inside the method but only one thread will enter inside the synchronized block as shown in the diagram (14-DEC-24) 

Note :- In between method level synchronization and block level synchronization, block level synchronization is more preferable because all the threads can enter inside the method so only the PART OF THE METHOD GETS synchronized so only one thread will enter inside the synchronized block.

Note :  Synchronized area is a restricated area, with 
permission only a thread can enter inside synchronized area.
-----------------------------------------------------------
How synchronization logic controls multiple threads ?
------------------------------------------------------
Every Object has a lock(monitor) in java environment and this lock can be given to only one Thread at a time.

Actually this lock is available with each individual object provided by Object class. 

The thread who acquires the lock from the object will enter inside the synchronized area, it will complete its task without any disturbance because at a time there will be only one thread inside the synchronized area(for single Object). *This is known as Thread-safety in java.

The thread which is inside the synchronized area, after completion of its task while going back will release the lock so the other threads (which are waiting outside for the lock) will get a chance to enter inside the synchronized area by again taking the lock from the object and submitting it to the synchronization mechanism.
This is how synchronization mechanism controls multiple Threads.

Note :- Synchronization logic can be done by senior programmers in the real time industry because due to poor synchronization there may be chance of getting deadlock.
-----------------------------------------------------------
Program on Method Level Synchronization :
------------------------------------------
	package com.ravi.thread;
	
	class Table
	{
		public synchronized void printTable(int num) 
		{
			for(int i=1; i<=10; i++)
			{
				System.out.println(num+" X "+i+" = "+(num*i));
				try
				{
					Thread.sleep(1000);
				}
				catch(InterruptedException e)
				{
					e.printStackTrace();
				}
			}
			System.out.println("...................");
		}
	}
	public class MethodLevelSynchronization 
	{
		public static void main(String[] args) 
		{
			Table obj = new Table();   //Lock is created Here
			
			Thread t1 = new Thread()
			{
				@Override
				public void run()
				{
					obj.printTable(5);
				}
			};
	
			Thread t2 = new Thread()
			{
				@Override
				public void run()
				{
					obj.printTable(10);
				}
			};
	
			t1.start();  t2.start();
		}
	
	}
----------------------------------------------------------
//Program on block Level Synchronization :
------------------------------------------
	package com.ravi.advanced;
	
	//Block level synchronization
	
	class ThreadName
	{
		public void printThreadName()  
		{		  		
		  String name = Thread.currentThread().getName();
		  System.out.println("Thread inside the method is :"+name);
				
			   synchronized(this)  //synchronized Block
			   {  			   
				for(int i=1; i<=9; i++)   
				{
					System.out.println("i value is :"+i+" by :"+name);
				}
				System.out.println(".............................");
			   }		
		}
	}
	public class BlockSynchronization 
	{
		public static void main(String[] args)
		{
			ThreadName obj1 = new ThreadName(); //lock is created	
			
			Runnable r1 = () -> obj1.printThreadName();
			
			Thread t1 = new Thread(r1,"Child1"); 
			Thread t2 = new Thread(r1,"Child2"); 
			t1.start(); t2.start();				
		}
	}
-----------------------------------------------------------
Limitation/Drawback of Object Level Synchronization :
------------------------------------------------------
From the given diagram it is clear that there is no interference between t1 and t2 thread because they are passing throgh Object1 where as on the other hand there is no interferenec even in between t3 and t4 threads because they are also passing through Object2 (another object).

But there may be chance that with t1 Thread (object1), t3 or t4 thread can enter inside the synchronized area at the same time, simillarly it is also possible that with t2 thread, t3 or t4 thread can enter inside the synchronized area so the conclusion is, synchronization mechanism does not work with multiple Objects.(Diagram 16-DEC-24)

	package com.ravi.advanced;
	class PrintTable
	{
		    public synchronized void printTable(int n)
		    {
		       for(int i=1; i<=10; i++)
		       {
		    	   System.out.println(n+" X "+i+" = "+(n*i));
		    	   try
		    	   {
		    		   Thread.sleep(500);
		    	   }
		    	   catch(Exception e)
		    	   {	    		   
		    	   }
		       }
		       System.out.println(".......................");
		    }	
	}
	
	public class ProblemWithObjectLevelSynchronization
	{
		public static void main(String[] args) 
		{
			
			PrintTable pt1 = new PrintTable(); //lock1		
			PrintTable pt2 = new PrintTable(); //lock2	
					
			Thread t1 = new Thread()  //Anonymous inner class concept
					{
				       @Override
				       public void run()
				       {
				    	   pt1.printTable(2);	//lock1
				       }			   
					};
			       	        
			        Thread t2 = new Thread()
					{
				       @Override
				       public void run()
				       {
				    	   pt1.printTable(3);	//lock1
				       }			   
					};
			                
			        Thread t3 = new Thread()
					{
				       @Override
				       public void run()
				       {
				    	   pt2.printTable(8);	//lock2
				       }			   
					};
			               
			        Thread t4 = new Thread()
					{
				       @Override
				       public void run()
				       {
				    	   pt2.printTable(9); //lock2
				       }			   
					};
					 t1.start();	t2.start();	 t3.start();  t4.start(); 
		}
	}

Note : So, It is clear that synchronization logic will not work with multiple Objects.
-----------------------------------------------------------
In order to resolve this synchronization issue, Static
Synchronization is introduced by Oracle Developer.

**Static Synchronization :
------------------------
If we make our synchronized method as a static method then it is called static synchronization.

Here, To call static synchronized method, object is not required.

The thread will take the lock from class but not object because we can call the static method with the help of class name.

Unlike Object, we cann't create multiple classes in the same package.

For synchronized block we can write the following code :

	synchronized(ClassName.class)
	{
	
	}

-----------------------------------------------------------
	//Program on static synchronization :
	package com.ravi.advanced;
	class MyTable     
	{
		 public static synchronized void printTable(int n)  //static synchronization
		    {
		       for(int i=1; i<=10; i++)
		       {
		    	   try
		    	   {
		    		   Thread.sleep(100);
		    	   }
		    	   catch(InterruptedException e)
		    	   {
		    		  System.err.println("Thread is Interrupted...");
		    	   }
		    	   System.out.println(n+" X "+i+" = "+(n*i));
		       }
		       System.out.println("------------------------");
		    }
	}
	public class StaticSynchronization 
	{
		public static void main(String[] args)
		{
				        Thread t1 = new Thread()
						{
					      @Override
					      public void run()
					      {
					    	 MyTable.printTable(5); 
					      }
						};		
						
						Thread t2 = new Thread()
						{
					      @Override
					      public void run()
					      {
					    	  MyTable.printTable(10);
					      }
						};										
	
						Runnable r3 = ()-> MyTable.printTable(15);
						Thread t3 = new Thread(r3);
						
						t1.start();
						t2.start();	
						t3.start();
						
			}
	}
----------------------------------------------------------
In between extends Thread and implements Runnable which one is better and why?

In between extends Thread and implements Runnable, implements Runnable is more better approach due to the following reasons.

1) In extend Thread class approach, We can't extend any other class further
   (Multiple Inheritance not possible by using class) but we can implement multiple interfaces. On the other hand in implements Runnable approach we can extend a single class as well as implement multiple interfaces.
   
2) In extends Thread class all the Thread class properties are available to sub class so sub class is heavy weight, the same thing is not available  while implementing Runnable interface.

3) In extends Thread class approach we have same target for different  Threads but by using implements we can provide different target for  different Threads.

4) In extends Thread class approach we don't have Lambda expression support  but by using Runnable interface we can implement Lambda.

Conclusion :
Implements Runnable Advantage 
1) extend a single class
2) Light weight
3) Assigning different targets (Loose coupling)
3) Implemnting Lambda
-----------------------------------------------------------
Thread Priority :
-----------------
It is possible in java to assign priority to a Thread. Thread class has provided two predefined methods setPriority(int newPriority) and getPriority() to set and get the priority of the thread respectively.

In java we can set the priority of the Thread in numbers from 1- 10 only where 1 is the minimum priority and 10 is the maximum priority.

Whenever we create a thread in java by default its priority would be 5 that is normal priority.

The user-defined thread created as a part of main thread will acquire the same priority of main Thread.

Thread class has also provided 3 final static variables which are as follows :-

	public static final int MIN_PRIORITY = 1;
	public static final int NORM_PRIORITY = 5;
	public static final int MAX_PRIORITY = 10;


Note :- We can't set the priority of the Thread beyond the limit(1-10) so if we set the priority beyond the limit (1 to 10) then it will generate an exception java.lang.IllegalArgumentException.

------------------------------------------------------------

	public class ThreadPriority {
	
		public static void main(String[] args) 
		{
			//Default Priority of the Thread
			Thread t = Thread.currentThread();
			System.out.println("Main thread priority is :"+t.getPriority());
			
			Thread t1 = new Thread();
			System.out.println("User thread priority is :"+t1.getPriority());
			
		}
	
	}

Note : By default every thread even main thread is having default priority i.e 5.
-----------------------------------------------------------

	class Priority extends Thread
	{
		@Override
		public void run()
		{
			int priority = Thread.currentThread().getPriority();
			System.out.println("Child Thread priority is :"+priority);
		}
	}
	
	public class ThreadPriorityDemo1 {
	
		public static void main(String[] args) 
		{
			Thread t = Thread.currentThread();
			t.setPriority(9);
			
			Priority p1 = new Priority();
			p1.start();
			
			System.out.println("Main Thread priority is :"+t.getPriority());
	
		}
	
	}

Note : The thread which are created under main Thread will 
acquire main thread priority.

-----------------------------------------------------------


	class MyPriority extends Thread
	{
		@Override
		public void run()
		{
			int count = 0;
			
			for(int i=1; i<=1000000; i++)
			{
		       count++;		
			}
			
			System.out.println("Thread name is :"+Thread.currentThread().getName());
			System.out.println("Thread Priority is :"+Thread.currentThread().getPriority());
		}
	}
	
	public class ThreadPriorityDemo2 
	{
		public static void main(String[] args) 
		{
	       MyPriority m1 = new MyPriority();
	       m1.setPriority(Thread.MIN_PRIORITY);
	       m1.setName("Last");
	       
	       MyPriority m2 = new MyPriority();
	       m2.setPriority(Thread.MAX_PRIORITY);
	       m2.setName("First");
	       
	       m1.start();  m2.start();
	
		}
	
	}

Most of time the thread having highest priority will complete its task but we can't say that it will always complete its task first that means Thread schedular dominates over the priority of the Thread.
----------------------------------------------------------
Thread.yield() :
----------------
It is a static method of Thread class.

It will send a notification to thread schedular to stop the currently executing Thread (In Running state) and provide a chance to Threads which are in Runnable state to enter inside the running state having same priority or higher priority than currently executing Thread. 

Here The running Thread will directly move from Running state to Runnable state.

The Thread schedular may accept OR ignore this notification message given by currently executing Thread.

Here there is no guarantee that  after using yield() method the running Thread will move to Runnable state and from Runnable state the thread can move to Running state.[That is the reason yield() method is not throwing InterruptedExecption]

If the thread which is in runnable state is having low priority than the current executing thread in Running state,  then currently executing thread will continue its execution.

*It is mainly used to avoid the over-utilisation a CPU by the current Thread.



	class MyThread implements Runnable
	{
		@Override
		public void run()
		{
			String name = Thread.currentThread().getName();
			
			for(int i=1; i<=10; i++)
			{
				System.out.println("i value is :"+i+" by "+name);
				
				if(name.equals("Child1"))
				{
					Thread.yield();
				}
			}
		}
	}
	
	public class YieldDemo {
	
		public static void main(String[] args) 
		{
			MyThread mt = new MyThread();
			
			Thread t1 = new Thread(mt, "Child1");
			Thread t2 = new Thread(mt, "Child2");
			
			t1.start();  t2.start();
			
	
		}
	
	}
-----------------------------------------------------------
18-12-2024
----------
** Inter Thread Communication(ITC) :
------------------------------------
It is a mechanism to communicate or co-ordinate between two synchronized threads within the context to achieve a particular task.

In ITC we put a thread into wait mode by using wait() method and other thread will complete its corresponding task, after completion of the task it will call notify() method so the waiting thread will get a notification to complete its remaining task.

ITC can be implemented by the following method of Object class.

1) public  final void wait() throws InterruptedException

2) public native final void notify()

3) public native final void notifyAll()


public  final void wait() throws InterruptedException :-
-------------------------------------------------------------
It is a predefined non static method of Object class. We can use this method from synchronized area only otherwise we will get IllegalMonitorStateException.

It will put a thread into temporarly waiting state and it will release the Object lock, It will remain in the wait state till another thread provides a notification message on the same object, After getting the lock (not notification message), It will wake up and it will complete its remaining task.

public native final void notify() :-
-------------------------------------
It will wake up the single thread that is waiting on the same object.It will not release the lock , once synchronized area is completed then only lock will be released.

Once a waiting thread will get the notification from the another thraed using notify()/notifyAll() method then the waiting thread will move from Waiting state to Runnable state(Ready to run state)

public native final void notifyAll() :-
----------------------------------------
It will wake up all the threads which are waiting on the same object.It will not release the lock , once synchronized area is completed then only lock will be released.

*Note :- wait(), notify() and notifyAll() methods are defined in Object class but not in Thread class because these methods are related to lock(because we can use these methods from the synchronized area ONLY) and Object has a lock so, all these methods are defined inside Object class.

The following program explains we should use these methods from synchronized area only otherwise we will get java.lang.IllegalMonitorStateException.

	package com.ravi.itc;
	
	
	public class ITCDemo1 
	{
		public  static void main(String[] args) throws InterruptedException 
		{
		    Object obj = new Object();
		    obj.wait();	
		}
	}
--------------------------------------------------------------
The following program explains, if we don't have proper
co-ordination between two threads then Output is un-predicatable.

	package com.ravi.itc;
	
	class Test extends Thread
	{
		private int val = 0;
		
		@Override
		public void run()
		{
			for(int i=1; i<=10; i++)  //i = 
			{
				val = val + i;        //val = 1    3    6   10    15
				try
				{
					Thread.sleep(100);
				}
				catch(InterruptedException e)
				{
					e.printStackTrace();
				}
			}
		
		}
		
		public int getVal()
		{
			return this.val;
		}	
	}
	
	public class ITCDemo2 
	{
		public static void main(String[] args) throws InterruptedException 
		{
	       System.out.println("Main Thread Started...");
	       
	       Test t1 = new Test();
	       t1.start();
	       
	       Thread.sleep(200);
	       
	       System.out.println(t1.getVal());
		}
	
	}

Note : In the above program, there is no co-ordination between 
main thread and child thread so the value of val will change based on loop iteration so output is un-predictable
---------------------------------------------------------------
	package com.ravi.itc;
	
	class Demo extends Thread
	{
		private int val = 0;
		
		@Override
		public void run()
		{
			synchronized(this)
			{
				for(int i=1; i<=10; i++)
				{
					val = val + i;
				}
				System.out.println("Completed My task, Sending u notification");
				notify();		
			}
		}
		
		public int getVal()
		{
			return this.val;
		}
		
		
		
		
	}
	
	public class ITCDemo3 {
	
		public static void main(String[] args) throws InterruptedException 
		{		
			Demo d1 = new Demo();
			d1.start();
							
			synchronized(d1)
			{			
				System.out.println("main thread is waiting Here :");
				d1.wait();
				System.out.println("Main thread got notification :");
				System.out.println(d1.getVal());
			}
			
			
		}
	
	}

Note : Here we have co-ordination between main thread and child thread so we will get predicatable output.
--------------------------------------------------------------
	package com.ravi.itc;
	
	class Customer
	{
		private double balance = 10000;
		
		public synchronized void withdraw(double amount)
		{
			System.out.println("Withdraw is in progress..");
			
			if(amount> this.balance)
			{
				System.out.println("Balance is low, waiting for deposit");
				try
				{
					wait();
					System.out.println("Got Notification");
				}
				catch(InterruptedException e)
				{
					e.printStackTrace();
				}
			}
			this.balance = this.balance - amount;
			System.out.println("Amount after withdraw is :"+this.balance);
		}
		
		public synchronized void deposit(double amount)
		{
			System.out.println("Deposit is in progress...");
			this.balance = this.balance + amount;
			System.out.println("Balance after deposit is :"+this.balance);
			System.out.println("Sending notification to withdraw amount");
			notify();		
		}
		
	}
	
	public class ITCDemo4 {
	
		public static void main(String[] args)
		{
			Customer obj = new Customer();
			
			Thread son = new Thread()
			{
			   @Override
			   public void run()
			   {
				   obj.withdraw(10000);
			   }
			   
			};
			son.start();
			
			Thread father = new Thread()
			{
				   @Override
				   public void run()
				   {
					   obj.deposit(10000);
				   }
			};
			
			father.start();
					
		}
	
	}
--------------------------------------------------------------
19-12-2024
-----------
	package com.ravi.itc;
	
	class TicketSystem 
	{
	    private int availableTickets = 5;   //availableTickets = 3
	    
	    public synchronized void bookTicket(int numberOfTickets) //numberOfTickets 4 
	    {
	        while (availableTickets < numberOfTickets) //5 < 4
	        {
	           System.out.println("Not enough tickets available, Waiting for cancellation...");
	            try 
	            {
	                wait(); 
	            }
	            catch (InterruptedException e) 
	            {
	                e.printStackTrace();
	            }
	        }
	        availableTickets = availableTickets - numberOfTickets;  
	        
	        System.out.println("Booked " + numberOfTickets + " ticket(s). Remaining tickets: " + availableTickets);
	        notify(); 
	              
	    }
	
	    
	  public synchronized void cancelTicket(int numberOfTickets)//numberOfTickets 3 
	    {
	        availableTickets = availableTickets + numberOfTickets;
	        System.out.println("Canceled " + numberOfTickets + " ticket(s). Available tickets: " + availableTickets);
	        notify(); 
	    }
	}
	
	
	public class ITCDemo5 
	{
	    public static void main(String[] args) 
	    {
	        TicketSystem ticketSystem = new TicketSystem(); //lock is available
	
	        Thread bookingThread = new Thread()
	        {
	        	@Override
	            public void run() 
	        	{
	                int[] ticketsToBook = {2, 4, 4};  
	                
	                for (int ticket : ticketsToBook) //ticket = 1
	                {
	                    ticketSystem.bookTicket(ticket);
	                    try 
	                    {
	                        Thread.sleep(1000); // give some time b/w booking
	                    } 
	                    catch (InterruptedException e)
	                    {
	                        e.printStackTrace();
	                    }
	                }
	        	 }        	
	        };
	        bookingThread.start();
	        
	        Thread cancellationThread = new Thread()
	       	{
	        	@Override
	            public void run() 
	        	{
	                int[] ticketsToCancel = {1, 3, 2};
	                for (int ticket : ticketsToCancel) //ticket = 3
	                {
	                    ticketSystem.cancelTicket(ticket);
	                    try 
	                    {
	                        Thread.sleep(1500);  // give some time b/w cancellation
	                    } 
	                    catch (InterruptedException e) 
	                    {
	                        e.printStackTrace();
	                    }
	                }
	            }
	        };
	        cancellationThread.start();
	        
	        
	        
	        
	    }
	}

---------------------------------------------------------------
	package com.ravi.itc;
	
	class Resource 
	{
	    private boolean flag = false;
	
	    public synchronized void waitMethod() 
		{
			System.out.println("Wait");  //child1  wait  child1 is waiting Child1 notif
			
	       	while (!flag)
			{
	          try 
			  {
	             System.out.println(Thread.currentThread().getName() + " is waiting...");
	             System.out.println(Thread.currentThread().getName()+" is Waiting for Notification");
	             wait(); 
	          } 
			  catch (InterruptedException e) 
			  {
	                e.printStackTrace();
	          }
	        }
	        System.out.println(Thread.currentThread().getName() + " thread completed!!");
	    }
	
	    public synchronized void setMethod() 
		{
			System.out.println("notifyAll");
			this.flag = true;
	        System.out.println(Thread.currentThread().getName() + " has make flag value as a true");
	        notifyAll(); // Notify all waiting threads that the signal is set
	    }
	}
	
	public class ITCDemo6
	{
	    public static void main(String[] args) 
			{   	
	    	
	    	
	        Resource r1 = new Resource(); //lock is created
	
	        Thread t1 = new Thread(() -> r1.waitMethod(), "Child1");
			Thread t2 = new Thread(() -> r1.waitMethod(), "Child2");
			Thread t3 = new Thread(() -> r1.waitMethod(), "Child3");
	
			t1.start();
	        t2.start();
	        t3.start();
	       
			
			Thread setter = new Thread(() -> r1.setMethod(), "Setter_Thread");
	      
			   try 
				{
		            Thread.sleep(2000);
		        } 
				catch (InterruptedException e) 
				{
		            e.printStackTrace();
		        }
			
		       setter.start();
	    }
	}
---------------------------------------------------------------
ThreadGroup :
------------
It is a predefined class available in java.lang Package.

By using ThreadGroup class we can put 'n' number of threads into a single group to perform some common/different operation.

By using ThreadGroup class constructor, we can assign the name of group under which all the thread will be executed.

ThreadGroup tg = new ThreadGroup(String groupName);

ThreadGroup class has provided the following methods :

public String getName() : To get the name of the Group

pubic int activeCount() : How many threads are alive and running under that particular group.

Thread class has provided constructor to put the thread into particular group.

Thread t1 = new Thread(ThreadGroup tg, Runnable target, String name);

By using ThreadGroup class, multiple threads will be executed under single group.

	package com.ravi.group;
	
	class Foo implements Runnable
	{
		@Override
		public void run() 
		{
			String name = Thread.currentThread().getName();
		System.out.println(name+" is Enjoying Full Stack Developer under Batch 38");
			
		}	
	}
 
	public class ThreadGroupDemo1 
	{
	  public static void main(String[] args) throws InterruptedException 
	  {
		  
		 ThreadGroup tg = new ThreadGroup("Batch 38");
		 
		 Thread t1 = new Thread(tg, new Foo(), "Scott");
		 Thread t2 = new Thread(tg, new Foo(), "Smith");
		 Thread t3 = new Thread(tg, new Foo(), "Alen");
		 Thread t4 = new Thread(tg, new Foo(), "John");
		 Thread t5 = new Thread(tg, new Foo(), "Martin");
		 
		 t1.start();
		 t2.start();
		 t3.start();
		 t4.start();
		 t5.start();
		 
		 //Thread.sleep(5000);
		 
		System.out.println("How many threads are active under Batch 38 group :"+tg.activeCount());
		 
		System.out.println("Name of the the Group is :"+tg.getName());
		 
	  }
	}
---------------------------------------------------------------
	package com.ravi.group;
	
	public class ThreadGroupDemo2 {
	
		public static void main(String[] args) 
		{
			Thread t = Thread.currentThread();
			System.out.println(t.toString());
		}
	
	}



Output : Thread[#1main, 5, main]

Here first main is the name of the Thread, 5 is the priority and last main represents group name.

Whenever we define a main method then internally, main group is created and under this main group main thread is executed.
---------------------------------------------------------------
	package com.ravi.group;
	
	class TatkalTicket implements Runnable
	{
		@Override
		public void run() 
		{
			String name = Thread.currentThread().getName();
			System.out.println("Tatkal ticket booked by :"+name);
			
		}
		
	}
	
	class PremiumTatkal implements Runnable
	{
		@Override
		public void run() 
		{
			String name = Thread.currentThread().getName();
			System.out.println("Premium Tatkal ticket booked by :"+name);
			
		}	
	}
	
	public class ThreadGroupDemo3 
	{
		public static void main(String[] args) 
		{
			ThreadGroup tg1 = new ThreadGroup("Tatkal Ticket");		
			ThreadGroup tg2 = new ThreadGroup("Premium Tatkal");
			
			Thread t1 = new Thread(tg1,new TatkalTicket(), "Scott");		
			Thread t2 = new Thread(tg2,new PremiumTatkal(), "Smith");
			
			t1.start(); t2.start();		
	
		}
	
	}
---------------------------------------------------------------
20-12-2024
------------
Daemon Thread [Service Level Thread]
------------------------------------
Daemon thread is a low- priority thread which is used to provide background maintenance.  

The main purpose of of Daemon thread to provide services to the user thread.              

JVM can't terminate the program till any of the non-daemon (user) thread is active, once all the user thread will be completed then JVM will automatically terminate all Daemon threads, which are running in the background to support user threads.

The example of Daemon thread is Garbage Collection thread, which is running in the background for memory management.

In order to make a thread as a  Daemon thread , we should use setDaemon(true) which is a non static method Thread class.

-------------------------------------------------------------
	public class DaemonThreadDemo1 
	{
	  public static void main(String[] args) 
		{
		    System.out.println("Main Thread Started...");
	
	        Thread daemonThread = new Thread(() -> 
			{
	            while (true) 
				{
	                System.out.println("Daemon Thread is running...");
	                try 
					{
	                    Thread.sleep(1000);
	                } 
					catch (InterruptedException e) 
					{
	                    e.printStackTrace();
	                }
	            }
	        });
	
	        daemonThread.setDaemon(true); 
	        daemonThread.start();
	
	        
	        Thread userThread = new Thread(() -> 
			{
	            for (int i=1; i<=9; i++) 
				{
	                System.out.println("User Thread: " + i);
	                try 
					{
	                    Thread.sleep(2000);
	                } 
					catch (InterruptedException e) 
					{
	                    e.printStackTrace();
	                }
	            }
	        });
	
	        userThread.start();
	
	        System.out.println("Main Thread Ended...");
	    }
	}
------------------------------------------------------------
interrupt Method of Thread class :
----------------------------------
It is a predefined non static method of Thread class. The main purpose of this method to disturb the execution of the Thread, if the thread is in waiting or sleeping state.

Whenever a thread is interupted then it throws InterruptedException so the thread (if it is in sleeping or waiting mode) will get a chance to come out from a particular logic.

Points :-
---------
If we call interrupt method and if the thread is not in sleeping or waiting state then it will behave normally.

If we call interrupt method and if the thread is in sleeping or waiting state then we can stop the thread  gracefully.

*Overall interrupt method is mainly used to interrupt the
thread safely so we can manage the resources easily.

Methods :
---------
1) public void interrupt () :- Used to interrupt the Thread but the thread must be in sleeping or waiting mode.

2) public boolean isInterrupted() :- Used to verify whether thread is interrupted or not.
-----------------------------------------------------------
	class Interrupt extends Thread
	{
	   @Override
	   public void run()
		{
		   Thread t = Thread.currentThread();
		   System.out.println(t.isInterrupted()); 
	       
		   for(int i=1; i<=5; i++)
			{
			   System.out.println(i);  
	           try
			   {
				Thread.sleep(1000);
			   }
			   catch (Exception e)
			   {
				   System.err.println("Thread is Interrupted ");
				   e.printStackTrace();
			   }
			   
			}
		}
	}
	public class  InterruptThread
	{
		public static void main(String[] args) 
		{
	        Interrupt it = new Interrupt();
			System.out.println(it.getState());  //NEW
			it.start();
			it.interrupt();  //main thread is interrupting the child thread	
		}
	}

Note : main thread is interrupting child thread.
-------------------------------------------------------------
	class Interrupt extends Thread
	{
	   @Override
	   public void run()
		{
		   try
		   {
		    Thread.currentThread().interrupt();
	
		   for(int i=1; i<=10; i++)
			{
			   System.out.println("i value is :"+i);
			   Thread.sleep(1000);
			}
	
		   }
			catch (InterruptedException e)
			{
				System.err.println("Thread is Interrupted :"+e);
			}
			System.out.println("Child thread completed...");
		}
	}
	public class  InterruptThread1
	{
		public static void main(String[] args) 
		{	
			Interrupt it = new Interrupt();
			it.start();	
		}
	}

Note : Here Child Thread is interrupting itself.
------------------------------------------------------------
	public class InterruptThread2
	{
	    public static void main(String[] args) 
		{
	        Thread thread = new Thread(new MyRunnable());
	        thread.start();
	     
	        try 
			{
	          Thread.sleep(3000); //Main thread is waiting for 3 Sec
	        } 
			catch (InterruptedException e) 
			{
	            e.printStackTrace();
	        }
	        
	       thread.interrupt();  
	    }
	}
	
	class MyRunnable implements Runnable 
	{
	    @Override
	    public void run() 
		{
	        try 
			{
	            while (!Thread.currentThread().isInterrupted())
				{
	                System.out.println("Thread is running by locking the resource");
	                Thread.sleep(500);
	            }
	        } 
			catch (Exception e) 
			{
	            System.out.println("Thread interrupted gracefully.");
	        } 
			finally 
			{
	            System.out.println("Thread resource can be release here.");
	        }
	    }
	}
------------------------------------------------------------
Deadlock :
------------
It is a situation where two or more than two threads are in blocked state forever, here threads are waiting to acquire another thread resource without releasing it's own resource.

This situation happens when multiple threads demands same resource without releasing its own attached resource so as a result we get Deadlock situation and our execution of the program will go to an infinite state as shown in the diagram. (20-DEC-24)

	public class DeadlockExample
		{
	  public static void main(String[] args) 
		 {
	     String resource1 = "Ameerpet";  
	     String resource2 = "S R Nagar";  
	
	    // t1 tries to lock resource1 then resource2
	
	    Thread t1 = new Thread() 
			{
		  @Override
	      public void run() 
			  {
				  synchronized (resource1) 
					  {
				   System.out.println("Thread 1: locked resource 1");
				      try 
					   { 
					   Thread.sleep(1000);
					   } 
					   catch (Exception e) 
					   {}				  
					  				  
				   synchronized (resource2) //Nested synchronized block
				   {
					System.out.println("Thread 1: locked resource 2");
				   }
	             }
	      }
	    };
	
	
	    // t2 tries to lock resource2 then resource1
	    Thread t2 = new Thread() 
		{
	      @Override
	      public void run() 
		  {
	        synchronized (resource2) 
				{
	          System.out.println("Thread 2: locked resource 2");
	              try 
				  { 
				  Thread.sleep(1000);
				  } 
				  catch (Exception e) 
				  {}
	
	          synchronized (resource1) //Nested synchronized block
			  {
	            System.out.println("Thread 2: locked resource 1");
	          }
	        }
	      }
	    };    
	    t1.start();
	    t2.start();
	  }
	}

Note : Both the threads are in infinite waiting state so it is Deadlock situation.
------------------------------------------------------------
21-12-2024
----------
Remaining methods of Object class :
------------------------------------
protected native Object clone() throws CloneNotSupportedException
---------------------------------------------------------------
Object cloning in Java is the process of creating an exact copy of the original object. In other words, it is a way of creating a new object by copying all the data and attributes from the original object. 

The clone method of Object class creates an exact copy of an object.

In order to use clone() method , a class must implements Clonable interface because we can perform cloning operation on Cloneable objects only [JVM must have additional information] otherwise cloning opertion is not possible and JVM will throw an exception at runtime java.lang.CloneNotSupportedException

We can say an object is a Cloneable object if the corresponding class implements Cloneable interface.

It throws a checked Exception i.e CloneNotSupportedException

Note :- clone() method is not the part of Clonable interface[marker interface], actually it is the method of Object class.

clone() method of Object class follows deep copy concept so hashcode will be different as well as if we modify one object content then another object content will not be modified.

clone() method of Object class has protected access modifier so we need to override clone() method in sub class.

Steps we need to follow to perform clone operation :
----------------------------------------------------
1) The class must implements Cloneable interface
2) Override clone method [throws OR try catch]
3) In this Overridden method call Object class clone method
4) Perform downcasting at the time creating duplicate object
5) Two different objects are created [deep copy]

CloneMethodOperation.java
--------------------------
	package com.ravi.cloneable;
	
	class Customer implements Cloneable
	{
		private Integer customerId;
		private String customerName;
		
		public Customer(Integer customerId, String customerName)
		{
			super();
			this.customerId = customerId;
			this.customerName = customerName;
		}
	
		@Override
		public String toString() 
		{			
			return "Customer [customerId=" + customerId + ", customerName=" + customerName + "]";
		}
		
		
		public void setCustomerId(Integer customerId) 
		{
			this.customerId = customerId;
		}
	
		public void setCustomerName(String customerName) 
		{
			this.customerName = customerName;
		}
	
		@Override
		protected Object clone() throws CloneNotSupportedException
		{
			return super.clone();
		}	
	}
	
	public class CloneMethodOperation
	{
		public static void main(String[] args) throws CloneNotSupportedException 
		{
	       Customer c1 = new Customer(111, "Scott");                
	       Customer c2 = (Customer) c1.clone();  //Down casting
	       
	       System.out.println("Before Change :");
	       System.out.println(c1);
	       System.out.println(c2);
	   
	       System.out.println("After Change :");
	       c2.setCustomerId(222);
	       c2.setCustomerName("Smith");
	       System.out.println(c1);
	       System.out.println(c2);
	       
	       System.out.println(".............");
	       System.out.println(c1.hashCode());
	       System.out.println(c2.hashCode());
	             
		}
	}
-------------------------------------------------------------

protected void finalize() throws Throwable :
---------------------------------------------
It is a predefined method of Object class.

Garbage Collector automatically call this method just before an object is eligible for garbage collection to perform clean-up activity.

Here clean-up activity means closing the resources associated with that object like file connection, database connection, network connection and so on we can say resource de-allocation.

Note :- JVM calls finalize method only one per object.

         This method is deprecated from java 9V.



	class Student
	{
		private Integer studentId;
		private String studentName;
		public Student(Integer studentId, String studentName)
		{
			super();
			this.studentId = studentId;
			this.studentName = studentName;
		}
		@Override
		public String toString() 
		{
			return "Student [studentId=" + studentId + ", studentName=" + studentName + "]";
		}
		
		@Override
		public void finalize()
		{
		  System.out.println("Finalize method is invoked");	
		}	
	}
	public class FinalizeDemo 
	{
		public static void main(String[] args) throws InterruptedException 
		{
			Student s1 = new Student(111, "Raj");		
			System.out.println(s1);
			
			s1 = null;
			
			System.gc();  //calling garbage collector explicitly
			
			Thread.sleep(4000);
			
			System.out.println(s1);
			
	
		}
	
	}

Note : If Student object is eligible for GC then JVM will defnetly call finalize() method of Student class.
-------------------------------------------------------------

*What is the difference between final, finally and finalize

final :- It is a keyword which is used to provide some kind of          restriction like class is final, Method is
         final,variable is final.

finally :- if we open any resource as a part of try block then       that particular resource must be closed inside 
	   finally block otherwise program will be terminated ab-normally and the corresponding resource will not be closed (because the remaining lines of try block will not be executed)

finalize() :- It is a method which JVM is calling automatically just before object  destruction so if any resource (database, file and network) is associated with
	      that particular object then it will be closed
	      or de-allocated by JVM by calling finalize().
------------------------------------------------------------



Process based Multitasking :
----------------------------
If a CPU is switching from one subtask(Thread) of one process to another subtask of another process then it is called Process based Multitasking.

Thread based Multitasking :
---------------------------
If a CPU is switching from one subtask(Thread) to another subtask within the same process then it is called Thread based Multitasking.

What is Thread in java ?
-------------------------
A thread is light weight process and it is the basic unit of CPU which can run concurrently with another thread within the same context (process).

It is well known for independent execution. The main purpose of multithreading to boost the execution sequence.

A thread can run with another thread concurrently within the same process so our task will be completed as soon as possible.

In java whenever we define main method then JVM internally creates a thread called main thread under main group.

Program that describes that main is a Thread :
-----------------------------------------------
Whenever we define main method then JVM will create main thread internally under main group, the purpose of this main thread to execute the entire main method code.

In java there is a predefined class called Thread available in java.lang package, this class contains a predefined static factory method currentThread() which will provide currently executing Thread Object.

Thread t = Thread.currentThread(); //static Factory Method

Thread class has provided predefined method getName() to get the name of the Thread.
                 public final String getName();
		 
package com.ravi.mt;

public class MainThread {

	public static void main(String[] args) 
	{
		//Thread obj = Thread.currentThread();
		//System.out.println("Current thread name is :"+obj.getName());
		
		                  //OR
		
		String name = Thread.currentThread().getName();
		System.out.println("Current thread name is :"+name);

	}

}
--------------------------------------------------------------
How to create user-defined thread ?
-----------------------------------
We can create user-defined thread by using the following two packages 

  1) By using java.lang package [JDK 1.0]
  2) By using java.util.concurrent sub package [JDK 1.5]

Creating user-defind Thread by using java.lang package :
--------------------------------------------------------
By using java.lang package we can create user-defined thread by using any one of the following two approaches :

 1) By extending java.lang.Thread class
 2) By implementing java.lang.Runnable interface 

 Note :- Thread is a predefined class available in java.lang package where as Runnable is a predefined interface available in java.lang Package.
 -------------------------------------------------------------
 Creating user-defined Thread by using extending Thread class :
 --------------------------------------------------------------
 
 package com.ravi.mt;

class UserThread extends Thread
{
	@Override
	public void run()
	{
		System.out.println("My user thread is running in a separate stack"); 
	}
}

public class MainThread {

	public static void main(String[] args) 
	{
		System.out.println("Main thread started");
		
		UserThread ut = new UserThread();
		ut.start();
		
		System.out.println(10/0);
		
		System.out.println("Main thread ended");

	}

}


In the above program, we have two threads, main thread which is responsible to execute 
main method and Thread-0 thread which is responsible to execute run() method. [10-DEC-24]

In entire Multithreading concept start() is the only method which is responsible to create a new thread.
-----------------------------------------------------------------
public synchronized void start() :
-----------------------------------
start() is a predefined non static method of Thread class which internally performs the following two tasks :

1) It will make a request to the O.S to assign a new thread for concurrent execution.

2) It will implicitly call run() method on the current object.

--------------------------------------------------------------
public final boolean isAlive() :-
-----------------------------
It is a predefined non static method of Thread class through which we can find out whether a thread has started or not ?

As we know a new thread is created/started after calling start() method so if we use isAlive() method before start() method, it will return false but if the same isAlive() method if we invoke after the start() method, it will return true.

We can't restart a thread in java if we try to restart then It will generate an exception i.e java.lang.IllegalThreadStateException


IsAlive.java
--------------
package com.ravi.basic;

class Foo extends Thread
{
	@Override
	public void run()
	{
		System.out.println("Child thread is running...");
		System.out.println("It is running with separate stack");		
	}	
}
public class IsAlive 
{
	public static void main(String[] args) 
	{
		System.out.println("Main Thread started...");
		
		Foo f1 = new Foo();
		System.out.println("Is Thread alive : "+f1.isAlive());
		f1.start();
		System.out.println("Thread is alive or not : "+f1.isAlive());
		
		f1.start();  //java.lang.IllegralThreadStateException
		
		System.out.println("Main Thread ended...");
		
		
	}
}
---------------------------------------------------------------
package com.ravi.basic;

class Stuff extends Thread
{
	@Override
	public void run() 
	{		
		System.out.println("Child Thread is Running!!!!");
	}	
}
public class ExceptionDemo 
{
	public static void main(String[] args)
	{		
		System.out.println("Main Thread Started");			
				
		Stuff s1 = new Stuff(); 
		Stuff s2 = new Stuff(); 		
				
		s1.start();
		s2.start();
		
		System.out.println(10/0);
		
		System.out.println("Main Thread Ended");
	}

}
Note :- Here main thread is interrupted due to AE but still child thread will be executed because child threads are executing with separate Stack
--------------------------------------------------------------
package com.ravi.basic;

class Sample extends Thread
{
	@Override
	public void run()
	{
		String name = Thread.currentThread().getName();
		
		for(int i=1; i<=10; i++)
		{
			System.out.println(i+" by "+name+" thread");
		}
		
		
	}
	
}

public class ThreadLoop 
{	
	public static void main(String[] args) 
	{	
	    Sample s1 = new Sample();
	    s1.start();
	   
        String name = Thread.currentThread().getName();
		
		for(int i=1; i<=10; i++)
		{
			System.out.println(i+" by "+name+" Thread");
		}
		
		int x = 1;
		do
		{
			System.out.println("Enjoy Multithreading by "+name+ " Thread");
			
			x++;
		}
		while(x<=10);
		
	}
}

Note : Here processor is frequently switching from main thread to Thread-0 thread so output is un-predicatable
---------------------------------------------------------------
How to set and get the name of the Thread : 
--------------------------------------------
How to set and get the name of the Thread : 
--------------------------------------------------
Whenever we create a userdefined Thread in java then by default JVM assigns the name of thread is Thread-0, Thread-1, Thread-2 and so on.

If a user wants to assign some user defined name of the Thread, then Thread class has provided a predefined method called setName(String name) to set the name of the Thread.

On the other hand we want to get the name of the Thread then Thread class has provided a predefined method called getName().

public final void setName(String name)  //setter

public final String getName()  //getter

---------------------------------------------------------------
package com.ravi.basic;
class DoStuff extends Thread  
{
	@Override
	public void run()
	{
		String name = Thread.currentThread().getName();
		System.out.println(name +" thread is running Here!!!!");
	}
}
public class ThreadName 
{
	public static void main(String[] args) 
	{
		DoStuff t1 = new DoStuff(); 
		DoStuff t2 = new DoStuff(); 
		
		t1.start();			
		t2.start();	
		
			
	System.out.println(Thread.currentThread().getName()+" thread is running.....");
	}
}

Note :- If we don't provide our user-defined name for the thread then by default the name would be Thread-0, Thread-1, Thread-2 and so on.
---------------------------------------------------------------
package com.ravi.basic;
class Demo extends Thread
{
	@Override
	public void run()
	{
		 String name = Thread.currentThread().getName();
		 System.out.println("Running Thread name is :"+name);
	}
}
public class ThreadName1 
{
	public static void main(String[] args) 
	{
	  Thread t =  Thread.currentThread();
	  t.setName("Parent"); //Changing the name of the main thread
	  
	   Demo d1 = new Demo();
	   Demo d2 = new Demo();
	   
	 
	   d1.setName("Child1");
	   d2.setName("Child2");	   
	   d1.start();  d2.start();
	   
	   
	   
	   String name = Thread.currentThread().getName();
	   System.out.println(name + " Thread is running Here..");
	}
}
---------------------------------------------------------------
package com.ravi.basic;

import java.util.InputMismatchException;
import java.util.Scanner;

class BatchAssignment extends Thread
{
	@Override
	public void run()
	{
		String name = Thread.currentThread().getName();
		
		if(name !=null && name.equalsIgnoreCase("Placement"))
		{
			this.placementBatch();
		}
		else if(name !=null && name.equalsIgnoreCase("Regular"))
		{
			this.regularBatch();
		}
		else
		{
			throw new NullPointerException("Name can't be null");
		}
	}
	
	public void placementBatch()
	{
		System.out.println("I am a placement batch student.");
	}
	
	public void regularBatch()
	{
		System.out.println("I am a Regular batch student.");
	}
}


public class ThreadName2 
{
	public static void main(String[] args) 
	{
		Scanner sc = new Scanner(System.in);	
		try(sc)
		{
			System.out.print("Enter your Batch Title :");
			String title = sc.next();
			
			BatchAssignment b = new BatchAssignment();
			b.setName(title);
			
			b.start();
		}
		catch(InputMismatchException e)
		{
			System.out.println("Invalid Input");
		}
		
		
	}

}
---------------------------------------------------------------
Thread.sleep(long millisecond) :
-------------------------------
If we want to put a thread into temporarly waiting state then we should use sleep() method.

The waiting time of the Thread depends upon the time specified by the user in millisecond as parameter to sleep() method.

It is a static method of Thread class.

Thread.sleep(1000); //Thread will wait for 1 second

It is throwing a checked Exception i.e InterruptedException because there may be chance that this sleeping thread may be interrupted by a thread so provide either try-catch or declare the method as throws.
--------------------------------------------------------------
package com.ravi.basic;

class Sleep extends Thread
{
   @Override
   public void run()
   {	 
	   
	   for(int i=1; i<=10; i++)
	   {
		   System.out.println("i value is :"+i);
		   try
		   {
			   Thread.sleep(1000);
		   }
		   catch(InterruptedException e)
		   {
			   System.err.println("Thread interrupted "+e);
		   }
	   }
   }
   
}
public class SleepDemo 
{
	public static void main(String[] args)  
	{
	  	Sleep s = new Sleep();
	  	s.start(); 		  	
	  	  
	}
}
---------------------------------------------------------------
package com.ravi.basic;

class MyTest extends Thread 
{	
	
	@Override
	public void run()  
	{		
		System.out.println("Child Thread id is :"+Thread.currentThread().getId());  
		
		for(int i=1; i<=5; i++) 
		{
			System.out.println("i value is :"+i); // 11  22  33  44
			try
			{
				Thread.sleep(1000);				
			}
			catch(InterruptedException e)
			{
				System.err.println("Thread has Interrupted");
			}
		}
	}
}
public class SleepDemo1 
{
	public static void main(String[] args) 
	{		
		System.out.println("Main Thread id is :"+Thread.currentThread().getId());  //1
		
		MyTest m1 = new MyTest();		
		MyTest m2 = new MyTest();
		
		m1.start();
		m2.start();			
	}
}
---------------------------------------------------------------
package com.ravi.basic;

class MyThread1 extends Thread
{
	@Override
	public void run()
	{
		System.out.println("Child Thread is running");
	}
}

public class SleepDemo2 {

	public static void main(String[] args) throws InterruptedException 
	{
		MyThread1 m1 = new MyThread1();
		m1.start();
		
		m1.sleep(2000); //current thread is main
		
		System.out.println("Main Thread is Running");
		

	}

}

Note : Here main thread will go into the sleeping state.
--------------------------------------------------------------
Assignment :
--------------
Thread.sleep(long millis, int nanos)
---------------------------------------------------------------
Basic Thread life cycle :
-------------------
As we know a thread is well known for Independent execution and it contains a life cycle which internally contains 5 states (Phases). 

During the life cycle of a thread, It can pass from thses 5 states. At a time a thread can reside to only one state of the given 5 states.

1) NEW State (Born state)

2) RUNNABLE state (Ready to Run state) [Thread Pool]

3) RUNNING state

4) WAITING / BLOCKED state

5) EXIT/Dead state

----------------------------------------------------------------------
New State :-
-------------
Whenever we create a thread instance(Thread Object) a thread comes to new state OR born state. New state does not mean that the Thread has started yet only the object or instance of Thread has been created.

Runnable state :-
-------------------
Whenever we call start() method on thread object, A thread moves to Runnable state i.e Ready to run state. Here Thread schedular is responsible to select/pick a particular Thread from Runnable state and sending that particular thread to Running state for execution.

Running state :-
-----------------
If a thread is in Running state that means the thread is executing its own run() method in a separate stack Memory.

From Running state a thread can move to waiting state either by an order of thread schedular or user has written some method(wait(), join() or sleep()) to put the thread into temporarly waiting state.

From Running state the Thread may also move to Runnable state directly, if user has written Thread.yield() method explicitly.

Waiting state :-
------------------
A thread is in waiting state means it is waiting for it's time period to complete. Once the time period will be completed then it will re-enter inside the Runnable state to complete its remaining task.

Dead or Exit :
----------------
Once a thread has successfully completed its run method in the corresponding stack then the thread will move to dead state. Please remember once a thread is dead we can't restart a thread in java.
---------------------------------------------------------------------
IQ :- If we write Thread.sleep(1000) then exactly after 1 sec the Thread will re-start?

Ans :- No, We can't say that the Thread will directly move from waiting state to Running state. 

The Thread will definetly wait for 1 sec in the waiting mode and then again it will re-enter into Runnable state which is control by Thread Schedular so we can't say that the Thread will re-start just after 1 sec.
---------------------------------------------------------------
join() method of Thread class :
-------------------------------
The main purpose of join() method to put the current thread into waiting state until the other thread finish its execution.

Here the currently executing thread stops its execution and the thread goes into the waiting state. The current thread remains in the wait state until the thread on which the join() method is invoked has achieved its dead state.

It also throws checked exception i.e InterruptedException so better to use try catch or declare the method as throws.

It is a non static method so we can call this method with the help of Thread object reference.

package com.ravi.basic;

class Join extends Thread
{
	@Override
	public void run()
	{
		for(int i=1; i<=5; i++)
		{
			System.out.println("i value is :"+i);
			try
			{
				Thread.sleep(1000);
			}
			catch(InterruptedException e)
			{
				e.printStackTrace();
			}
		}
	}
}

public class JoinDemo 
{
	public static void main(String[] args) throws InterruptedException 
	{
		System.out.println("Main Thread started");
		
		Join j1 = new Join();
		Join j2 = new Join();
		Join j3 = new Join();
		
		
		j1.start();
		
		j1.join(); //Here main thread will go to waiting state
				
		j2.start();
		j3.start();
		
		
		System.out.println("Main Thread ended");
	}

}
---------------------------------------------------------------
13-12-2024
-----------
package com.ravi.basic;

class Alpha extends Thread   
{
	@Override
	public void run()
	{
		Thread t = Thread.currentThread();
		String name = t.getName();	//Alpha_Thread is current thread		
		
		Beta b1 = new Beta();
		b1.setName("Beta_Thread");
        b1.start();  
        try 
        {
			b1.join(); //Alpha thread is waiting 4 Beta Thread to complete
		
			System.out.println("Alpha thread re-started");
		} 
        catch (InterruptedException e) 
        {			
			e.printStackTrace();
		}
		
		for(int i=1; i<=10; i++)
		{
			System.out.println(i+" by "+name);
		}
		
	}
}

public class JoinDemo2 
{
	public static void main(String[] args) 
	{
		Alpha a1 = new Alpha();
		a1.setName("Alpha_Thread");
		a1.start();
	}
}

class Beta extends Thread
{
	@Override
	public void run()
	{
		Thread t = Thread.currentThread();
		String name = t.getName();	
		
		for(int i=1; i<=20; i++)
		{
			System.out.println(i+" by "+name);
			try
			{
				Thread.sleep(500);
			}
			catch(InterruptedException e) {
				
			}
		}
		System.out.println("Beta Thread Ended");
	}
}

Note : Alpha Thread will wait till the completion of Beta Thread.
---------------------------------------------------------------
package com.ravi.basic;

public class JoinDemo1 
{
	public static void main(String[] args) throws InterruptedException 
	{
       System.out.println("Main Thread started");
       
       Thread t = Thread.currentThread();
       
       for(int i=1; i<=10; i++)
       {
    	   System.out.println(i+" by "+t.getName());
       }
       
       t.join();  //Deadlock [Main thread is waiting 4 main thread to complete]
           
       System.out.println("Main Thread Ended");
	}
}


Here, It is a deadlock state because main thread is waiting for main thread to complete.
---------------------------------------------------------------
Assignment :

join(long millis)
join(long millis, long nanos)
---------------------------------------------------------------
Anonymous inner class by using Thread class :
----------------------------------------------
 1) Anonymous inner class with reference variable :
 ---------------------------------------------------
package com.ravi.anonymous_thread;

public class AnonymousThreadWithReference 
{
	public static void main(String[] args) 
	{
		Thread t1 = new Thread()
		{
			@Override
			public void run()
			{
				String name = Thread.currentThread().getName();
				System.out.println("Anonymous Thread name is :"+name);
			}
			
		};
		t1.start();
		
		

	}

}

2) Anonymous inner class without reference variable :
-------------------------------------------------------
package com.ravi.anonymous_thread;

public class AnonymousWithoutReference {

	public static void main(String[] args) 
	{
		new Thread()
		{
		   @Override
		   public void run()
		   {
			   String name = Thread.currentThread().getName();
			   System.out.println("Running thread name is :"+name);
		   }
		}.start();
		
	}

}
---------------------------------------------------------------
Assigning target by Runnable interface :[Loose Coupling]
----------------------------------------------------------
By using Runnable interface approach we can assign different target to our threads. [13-DEC-24]

package com.ravi.basic;

class Ravi implements Runnable
{
	@Override
	public void run() 
	{
		System.out.println("Thread is performing my task ");		
	}	
}

public class RunnableDemo
{
   public static void main(String [] args)
   {
	   System.out.println("Main");        
        
        Thread t1 = new Thread(new Ravi());
        t1.start();
       
   }
}   
--------------------------------------------------------------
Assign different target for different Threads :
-----------------------------------------------
package com.ravi.basic;

class Ravi implements Runnable
{
	@Override
	public void run() 
	{
		System.out.println("Thread is performing my task ");		
	}	
}

public class RunnableDemo
{
   public static void main(String [] args)
   {
	   System.out.println("Main");        
        
        Thread t1 = new Thread(new Ravi());
        t1.start();
       
   }
}   
---------------------------------------------------------------
Thread class constructor :
---------------------------
We have total 10 constructors in the Thread class, The following are commonly used constructor in the Thread class

1) Thread t1 = new Thread();

2) Thread t2 = new Thread(String name);

3) Thread t3 = new Thread(Runnable target);

4) Thread t4 = new Thread(Runnable target, String name);

5) Thread t5 = new Thread(ThredGroup tg, String name);

6) Thread t6 = new Thread(ThredGroup tg, Runnable target);

7) Thread t7 = new Thread(ThredGroup tg, Runnable target, String name);
--------------------------------------------------------------
14-12-2024
-----------
Anonymous inner class by using Runnable interface :
----------------------------------------------------
Case 1 :
--------
package com.ravi.anonymous_runnable;

public class AnonymousDemo1 {

	public static void main(String[] args) 
	{
		Runnable r1 = new Runnable() 
		{			
			@Override
			public void run() 
			{
				String name = Thread.currentThread().getName();
				System.out.println("Current Thread Name is :"+name);
			}
		};

		Thread t1 = new Thread(r1);
		t1.start();
		
	}

}
--------------------------------------------------------------
Case 2:
-------
package com.ravi.anonymous_runnable;

public class AnonymousDemo2 {

	public static void main(String[] args) 
	{
		Thread t1 = new Thread(new Runnable() 
		{			
			@Override
			public void run() 
			{
				String name = Thread.currentThread().getName();
				System.out.println("Current Thread Name is :"+name);
				
			}
		});
		
		t1.start();	
		
	}

}
--------------------------------------------------------------
Case 3 :
--------
package com.ravi.anonymous_runnable;

public class AnonymousDemo3 {

	public static void main(String[] args) 
	{
		Thread t1 = new Thread(()-> System.out.println(Thread.currentThread().getName()));
		t1.start();
		
		System.out.println("................");
		
		new Thread(()-> System.out.println(Thread.currentThread().getName())).start();
		
		System.out.println("................");
		
		new Thread(()-> System.out.println(Thread.currentThread().getName()),"Scott").start();
		
	}

}
--------------------------------------------------------------
Drawback of Multithreading :
----------------------------
Multithreading is very good to complete our task as soon as possible but in some situation, It provides some wrong data or wrong result.

In Data Race or Race condition, all the threads try to access the resource at the same time so the result may be corrupted.

In multithreading if we want to perform read operation and data is not updatable data then multithreading is good but if the data is updatable data (modifiable data) then multithreading may produce some wrong result or wrong data which is known as Data Race OR race condition as shown in the diagram.(14-DEC)
---------------------------------------------------------------
package com.nit.testing;

class Customer implements Runnable
{
   private int availableSeat = 1;   
   private int wantedSeat;  //1
   
	public Customer(int wantedSeat) 
	{
	  super();
	  this.wantedSeat = wantedSeat;
    }
	
	@Override
	public synchronized void run() 
	{
		String name = null;	
		
		if(availableSeat >= wantedSeat)
		{
			name = Thread.currentThread().getName();
			System.out.println(wantedSeat+" seat is reserved for "+name);
			availableSeat = availableSeat - wantedSeat;			
		}
		else
		{
			name = Thread.currentThread().getName();
			System.err.println("Sorry!!"+name+" seat is not available");
		}
		
		
	}
	
}

public class RailwayReservation {

	public static void main(String[] args) throws InterruptedException 
	{
	   Customer c1 = new Customer(1);
	   
	   Thread t1 = new Thread(c1,"Scott");
	   Thread t2 = new Thread(c1,"Smith");
	   
	   t1.start();
	   
	  	   
	   t2.start();

	}

}
--------------------------------------------------------------
package com.ravi.multithreading_drawback;

class Customer
{
	private double availableBalance = 20000;	
	private double withdrawAmount;
	
	public Customer(double withdrawAmount) 
	{
		super();
		this.withdrawAmount = withdrawAmount;
	}
	
	public void withdraw()
	{
		String name = null;
		
		if(withdrawAmount<= availableBalance)
		{
			name = Thread.currentThread().getName();
			System.out.println(withdrawAmount+ " Amount, withdraw by :"+name);
			availableBalance = availableBalance - withdrawAmount;
		}
		else
		{
			name = Thread.currentThread().getName();
			System.err.println("Sorry!!"+name+" you have insufficient Balance");
		}		
	}
	
}

public class BankApplication {

	public static void main(String[] args) 
	{
		Customer obj = new Customer(20000);
		
		Runnable r1 = ()-> 
		{
		  obj.withdraw();
		};
		
		Thread t1 = new Thread(r1,"Scott");
		Thread t2 = new Thread(r1,"Smith");
		
		t1.start();  
			
		t2.start();
	}

}

Note : Here both the thread will get the balance.
-----------------------------------------------------------
16-12-2024
-----------
***Synchronization :
-------------------
In order to solve the problem of multithreading java software people has introduced synchronization concept.

In order to acheive sycnhronization in java we have a keyword called "synchronized".

It is a technique through which we can control multiple threads but accepting only one thread at all the time.

Synchronization allows only one thread to enter inside the synchronized area for a single object.

Synchronization can be divided into two categories :-

1) Method level synchronization

2) Block level synchronization

1) Method level synchronization :-
-----------------------------------
In method level synchronization, the entire method gets synchronized so all the thread will wait at method level and only one thread will enter inside the synchronized area as shown in the diagram.(14-DEC-24)


2) Block level synchronization
-------------------------------
In block level synchronization the entire method does not get synchronized, only the part of the method gets synchronized so all the thread will enter inside the method but only one thread will enter inside the synchronized block as shown in the diagram (14-DEC-24) 

Note :- In between method level synchronization and block level synchronization, block level synchronization is more preferable because all the threads can enter inside the method so only the PART OF THE METHOD GETS synchronized so only one thread will enter inside the synchronized block.

Note :  Synchronized area is a restricated area, with 
permission only a thread can enter inside synchronized area.
-----------------------------------------------------------
How synchronization logic controls multiple threads ?
------------------------------------------------------
Every Object has a lock(monitor) in java environment and this lock can be given to only one Thread at a time.

Actually this lock is available with each individual object provided by Object class. 

The thread who acquires the lock from the object will enter inside the synchronized area, it will complete its task without any disturbance because at a time there will be only one thread inside the synchronized area(for single Object). *This is known as Thread-safety in java.

The thread which is inside the synchronized area, after completion of its task while going back will release the lock so the other threads (which are waiting outside for the lock) will get a chance to enter inside the synchronized area by again taking the lock from the object and submitting it to the synchronization mechanism.
This is how synchronization mechanism controls multiple Threads.

Note :- Synchronization logic can be done by senior programmers in the real time industry because due to poor synchronization there may be chance of getting deadlock.
-----------------------------------------------------------
Program on Method Level Synchronization :
------------------------------------------
package com.ravi.thread;

class Table
{
	public synchronized void printTable(int num) 
	{
		for(int i=1; i<=10; i++)
		{
			System.out.println(num+" X "+i+" = "+(num*i));
			try
			{
				Thread.sleep(1000);
			}
			catch(InterruptedException e)
			{
				e.printStackTrace();
			}
		}
		System.out.println("...................");
	}
}
public class MethodLevelSynchronization 
{
	public static void main(String[] args) 
	{
		Table obj = new Table();   //Lock is created Here
		
		Thread t1 = new Thread()
		{
			@Override
			public void run()
			{
				obj.printTable(5);
			}
		};

		Thread t2 = new Thread()
		{
			@Override
			public void run()
			{
				obj.printTable(10);
			}
		};

		t1.start();  t2.start();
	}

}
----------------------------------------------------------
//Program on block Level Synchronization :
------------------------------------------
package com.ravi.advanced;

//Block level synchronization

class ThreadName
{
	public void printThreadName()  
	{		  		
	  String name = Thread.currentThread().getName();
	  System.out.println("Thread inside the method is :"+name);
			
		   synchronized(this)  //synchronized Block
		   {  			   
			for(int i=1; i<=9; i++)   
			{
				System.out.println("i value is :"+i+" by :"+name);
			}
			System.out.println(".............................");
		   }		
	}
}
public class BlockSynchronization 
{
	public static void main(String[] args)
	{
		ThreadName obj1 = new ThreadName(); //lock is created	
		
		Runnable r1 = () -> obj1.printThreadName();
		
		Thread t1 = new Thread(r1,"Child1"); 
		Thread t2 = new Thread(r1,"Child2"); 
		t1.start(); t2.start();				
	}
}
-----------------------------------------------------------
Limitation/Drawback of Object Level Synchronization :
------------------------------------------------------
From the given diagram it is clear that there is no interference between t1 and t2 thread because they are passing throgh Object1 where as on the other hand there is no interferenec even in between t3 and t4 threads because they are also passing through Object2 (another object).

But there may be chance that with t1 Thread (object1), t3 or t4 thread can enter inside the synchronized area at the same time, simillarly it is also possible that with t2 thread, t3 or t4 thread can enter inside the synchronized area so the conclusion is, synchronization mechanism does not work with multiple Objects.(Diagram 16-DEC-24)

package com.ravi.advanced;
class PrintTable
{
	    public synchronized void printTable(int n)
	    {
	       for(int i=1; i<=10; i++)
	       {
	    	   System.out.println(n+" X "+i+" = "+(n*i));
	    	   try
	    	   {
	    		   Thread.sleep(500);
	    	   }
	    	   catch(Exception e)
	    	   {	    		   
	    	   }
	       }
	       System.out.println(".......................");
	    }	
}

public class ProblemWithObjectLevelSynchronization
{
	public static void main(String[] args) 
	{
		
		PrintTable pt1 = new PrintTable(); //lock1		
		PrintTable pt2 = new PrintTable(); //lock2	
				
		Thread t1 = new Thread()  //Anonymous inner class concept
				{
			       @Override
			       public void run()
			       {
			    	   pt1.printTable(2);	//lock1
			       }			   
				};
		       	        
		        Thread t2 = new Thread()
				{
			       @Override
			       public void run()
			       {
			    	   pt1.printTable(3);	//lock1
			       }			   
				};
		                
		        Thread t3 = new Thread()
				{
			       @Override
			       public void run()
			       {
			    	   pt2.printTable(8);	//lock2
			       }			   
				};
		               
		        Thread t4 = new Thread()
				{
			       @Override
			       public void run()
			       {
			    	   pt2.printTable(9); //lock2
			       }			   
				};
				 t1.start();	t2.start();	 t3.start();  t4.start(); 
	}
}

Note : So, It is clear that synchronization logic will not work with multiple Objects.
-----------------------------------------------------------
In order to resolve this synchronization issue, Static
Synchronization is introduced by Oracle Developer.

**Static Synchronization :
------------------------
If we make our synchronized method as a static method then it is called static synchronization.

Here, To call static synchronized method, object is not required.

The thread will take the lock from class but not object because we can call the static method with the help of class name.

Unlike Object, we cann't create multiple classes in the same package.

For synchronized block we can write the following code :

synchronized(ClassName.class)
{

}

-----------------------------------------------------------
//Program on static synchronization :
package com.ravi.advanced;
class MyTable     
{
	 public static synchronized void printTable(int n)  //static synchronization
	    {
	       for(int i=1; i<=10; i++)
	       {
	    	   try
	    	   {
	    		   Thread.sleep(100);
	    	   }
	    	   catch(InterruptedException e)
	    	   {
	    		  System.err.println("Thread is Interrupted...");
	    	   }
	    	   System.out.println(n+" X "+i+" = "+(n*i));
	       }
	       System.out.println("------------------------");
	    }
}
public class StaticSynchronization 
{
	public static void main(String[] args)
	{
			        Thread t1 = new Thread()
					{
				      @Override
				      public void run()
				      {
				    	 MyTable.printTable(5); 
				      }
					};		
					
					Thread t2 = new Thread()
					{
				      @Override
				      public void run()
				      {
				    	  MyTable.printTable(10);
				      }
					};										

					Runnable r3 = ()-> MyTable.printTable(15);
					Thread t3 = new Thread(r3);
					
					t1.start();
					t2.start();	
					t3.start();
					
		}
}
----------------------------------------------------------
In between extends Thread and implements Runnable which one is better and why?

In between extends Thread and implements Runnable, implements Runnable is more better approach due to the following reasons.

1) In extend Thread class approach, We can't extend any other class further
   (Multiple Inheritance not possible by using class) but we can implement multiple interfaces. On the other hand in implements Runnable approach we can extend a single class as well as implement multiple interfaces.
   
2) In extends Thread class all the Thread class properties are available to sub class so sub class is heavy weight, the same thing is not available  while implementing Runnable interface.

3) In extends Thread class approach we have same target for different  Threads but by using implements we can provide different target for  different Threads.

4) In extends Thread class approach we don't have Lambda expression support  but by using Runnable interface we can implement Lambda.

Conclusion :
Implements Runnable Advantage 
1) extend a single class
2) Light weight
3) Assigning different targets (Loose coupling)
3) Implemnting Lambda
-----------------------------------------------------------
Thread Priority :
-----------------
It is possible in java to assign priority to a Thread. Thread class has provided two predefined methods setPriority(int newPriority) and getPriority() to set and get the priority of the thread respectively.

In java we can set the priority of the Thread in numbers from 1- 10 only where 1 is the minimum priority and 10 is the maximum priority.

Whenever we create a thread in java by default its priority would be 5 that is normal priority.

The user-defined thread created as a part of main thread will acquire the same priority of main Thread.

Thread class has also provided 3 final static variables which are as follows :-

public static final int MIN_PRIORITY = 1;
public static final int NORM_PRIORITY = 5;
public static final int MAX_PRIORITY = 10;


Note :- We can't set the priority of the Thread beyond the limit(1-10) so if we set the priority beyond the limit (1 to 10) then it will generate an exception java.lang.IllegalArgumentException.

------------------------------------------------------------

public class ThreadPriority {

	public static void main(String[] args) 
	{
		//Default Priority of the Thread
		Thread t = Thread.currentThread();
		System.out.println("Main thread priority is :"+t.getPriority());
		
		Thread t1 = new Thread();
		System.out.println("User thread priority is :"+t1.getPriority());
		
	}

}

Note : By default every thread even main thread is having default priority i.e 5.
-----------------------------------------------------------

class Priority extends Thread
{
	@Override
	public void run()
	{
		int priority = Thread.currentThread().getPriority();
		System.out.println("Child Thread priority is :"+priority);
	}
}

public class ThreadPriorityDemo1 {

	public static void main(String[] args) 
	{
		Thread t = Thread.currentThread();
		t.setPriority(9);
		
		Priority p1 = new Priority();
		p1.start();
		
		System.out.println("Main Thread priority is :"+t.getPriority());

	}

}

Note : The thread which are created under main Thread will 
acquire main thread priority.

-----------------------------------------------------------


class MyPriority extends Thread
{
	@Override
	public void run()
	{
		int count = 0;
		
		for(int i=1; i<=1000000; i++)
		{
	       count++;		
		}
		
		System.out.println("Thread name is :"+Thread.currentThread().getName());
		System.out.println("Thread Priority is :"+Thread.currentThread().getPriority());
	}
}

public class ThreadPriorityDemo2 
{
	public static void main(String[] args) 
	{
       MyPriority m1 = new MyPriority();
       m1.setPriority(Thread.MIN_PRIORITY);
       m1.setName("Last");
       
       MyPriority m2 = new MyPriority();
       m2.setPriority(Thread.MAX_PRIORITY);
       m2.setName("First");
       
       m1.start();  m2.start();

	}

}

Most of time the thread having highest priority will complete its task but we can't say that it will always complete its task first that means Thread schedular dominates over the priority of the Thread.
----------------------------------------------------------
Thread.yield() :
----------------
It is a static method of Thread class.

It will send a notification to thread schedular to stop the currently executing Thread (In Running state) and provide a chance to Threads which are in Runnable state to enter inside the running state having same priority or higher priority than currently executing Thread. 

Here The running Thread will directly move from Running state to Runnable state.

The Thread schedular may accept OR ignore this notification message given by currently executing Thread.

Here there is no guarantee that  after using yield() method the running Thread will move to Runnable state and from Runnable state the thread can move to Running state.[That is the reason yield() method is not throwing InterruptedExecption]

If the thread which is in runnable state is having low priority than the current executing thread in Running state,  then currently executing thread will continue its execution.

*It is mainly used to avoid the over-utilisation a CPU by the current Thread.



class MyThread implements Runnable
{
	@Override
	public void run()
	{
		String name = Thread.currentThread().getName();
		
		for(int i=1; i<=10; i++)
		{
			System.out.println("i value is :"+i+" by "+name);
			
			if(name.equals("Child1"))
			{
				Thread.yield();
			}
		}
	}
}

public class YieldDemo {

	public static void main(String[] args) 
	{
		MyThread mt = new MyThread();
		
		Thread t1 = new Thread(mt, "Child1");
		Thread t2 = new Thread(mt, "Child2");
		
		t1.start();  t2.start();
		

	}

}
-----------------------------------------------------------
18-12-2024
----------
** Inter Thread Communication(ITC) :
------------------------------------
It is a mechanism to communicate or co-ordinate between two synchronized threads within the context to achieve a particular task.

In ITC we put a thread into wait mode by using wait() method and other thread will complete its corresponding task, after completion of the task it will call notify() method so the waiting thread will get a notification to complete its remaining task.

ITC can be implemented by the following method of Object class.

1) public  final void wait() throws InterruptedException

2) public native final void notify()

3) public native final void notifyAll()


public  final void wait() throws InterruptedException :-
-------------------------------------------------------------
It is a predefined non static method of Object class. We can use this method from synchronized area only otherwise we will get IllegalMonitorStateException.

It will put a thread into temporarly waiting state and it will release the Object lock, It will remain in the wait state till another thread provides a notification message on the same object, After getting the lock (not notification message), It will wake up and it will complete its remaining task.

public native final void notify() :-
-------------------------------------
It will wake up the single thread that is waiting on the same object.It will not release the lock , once synchronized area is completed then only lock will be released.

Once a waiting thread will get the notification from the another thraed using notify()/notifyAll() method then the waiting thread will move from Waiting state to Runnable state(Ready to run state)

public native final void notifyAll() :-
----------------------------------------
It will wake up all the threads which are waiting on the same object.It will not release the lock , once synchronized area is completed then only lock will be released.

*Note :- wait(), notify() and notifyAll() methods are defined in Object class but not in Thread class because these methods are related to lock(because we can use these methods from the synchronized area ONLY) and Object has a lock so, all these methods are defined inside Object class.

The following program explains we should use these methods from synchronized area only otherwise we will get java.lang.IllegalMonitorStateException.

package com.ravi.itc;


public class ITCDemo1 
{
	public  static void main(String[] args) throws InterruptedException 
	{
	    Object obj = new Object();
	    obj.wait();	
	}
}
--------------------------------------------------------------
The following program explains, if we don't have proper
co-ordination between two threads then Output is un-predicatable.

package com.ravi.itc;

class Test extends Thread
{
	private int val = 0;
	
	@Override
	public void run()
	{
		for(int i=1; i<=10; i++)  //i = 
		{
			val = val + i;        //val = 1    3    6   10    15
			try
			{
				Thread.sleep(100);
			}
			catch(InterruptedException e)
			{
				e.printStackTrace();
			}
		}
	
	}
	
	public int getVal()
	{
		return this.val;
	}	
}

public class ITCDemo2 
{
	public static void main(String[] args) throws InterruptedException 
	{
       System.out.println("Main Thread Started...");
       
       Test t1 = new Test();
       t1.start();
       
       Thread.sleep(200);
       
       System.out.println(t1.getVal());
	}

}

Note : In the above program, there is no co-ordination between 
main thread and child thread so the value of val will change based on loop iteration so output is un-predictable
---------------------------------------------------------------
package com.ravi.itc;

class Demo extends Thread
{
	private int val = 0;
	
	@Override
	public void run()
	{
		synchronized(this)
		{
			for(int i=1; i<=10; i++)
			{
				val = val + i;
			}
			System.out.println("Completed My task, Sending u notification");
			notify();		
		}
	}
	
	public int getVal()
	{
		return this.val;
	}
	
	
	
	
}

public class ITCDemo3 {

	public static void main(String[] args) throws InterruptedException 
	{		
		Demo d1 = new Demo();
		d1.start();
						
		synchronized(d1)
		{			
			System.out.println("main thread is waiting Here :");
			d1.wait();
			System.out.println("Main thread got notification :");
			System.out.println(d1.getVal());
		}
		
		
	}

}

Note : Here we have co-ordination between main thread and child thread so we will get predicatable output.
--------------------------------------------------------------
package com.ravi.itc;

class Customer
{
	private double balance = 10000;
	
	public synchronized void withdraw(double amount)
	{
		System.out.println("Withdraw is in progress..");
		
		if(amount> this.balance)
		{
			System.out.println("Balance is low, waiting for deposit");
			try
			{
				wait();
				System.out.println("Got Notification");
			}
			catch(InterruptedException e)
			{
				e.printStackTrace();
			}
		}
		this.balance = this.balance - amount;
		System.out.println("Amount after withdraw is :"+this.balance);
	}
	
	public synchronized void deposit(double amount)
	{
		System.out.println("Deposit is in progress...");
		this.balance = this.balance + amount;
		System.out.println("Balance after deposit is :"+this.balance);
		System.out.println("Sending notification to withdraw amount");
		notify();		
	}
	
}

public class ITCDemo4 {

	public static void main(String[] args)
	{
		Customer obj = new Customer();
		
		Thread son = new Thread()
		{
		   @Override
		   public void run()
		   {
			   obj.withdraw(10000);
		   }
		   
		};
		son.start();
		
		Thread father = new Thread()
		{
			   @Override
			   public void run()
			   {
				   obj.deposit(10000);
			   }
		};
		
		father.start();
				
	}

}
--------------------------------------------------------------
19-12-2024
-----------
package com.ravi.itc;

class TicketSystem 
{
    private int availableTickets = 5;   //availableTickets = 3
    
    public synchronized void bookTicket(int numberOfTickets) //numberOfTickets 4 
    {
        while (availableTickets < numberOfTickets) //5 < 4
        {
           System.out.println("Not enough tickets available, Waiting for cancellation...");
            try 
            {
                wait(); 
            }
            catch (InterruptedException e) 
            {
                e.printStackTrace();
            }
        }
        availableTickets = availableTickets - numberOfTickets;  
        
        System.out.println("Booked " + numberOfTickets + " ticket(s). Remaining tickets: " + availableTickets);
        notify(); 
              
    }

    
  public synchronized void cancelTicket(int numberOfTickets)//numberOfTickets 3 
    {
        availableTickets = availableTickets + numberOfTickets;
        System.out.println("Canceled " + numberOfTickets + " ticket(s). Available tickets: " + availableTickets);
        notify(); 
    }
}


public class ITCDemo5 
{
    public static void main(String[] args) 
    {
        TicketSystem ticketSystem = new TicketSystem(); //lock is available

        Thread bookingThread = new Thread()
        {
        	@Override
            public void run() 
        	{
                int[] ticketsToBook = {2, 4, 4};  
                
                for (int ticket : ticketsToBook) //ticket = 1
                {
                    ticketSystem.bookTicket(ticket);
                    try 
                    {
                        Thread.sleep(1000); // give some time b/w booking
                    } 
                    catch (InterruptedException e)
                    {
                        e.printStackTrace();
                    }
                }
        	 }        	
        };
        bookingThread.start();
        
        Thread cancellationThread = new Thread()
       	{
        	@Override
            public void run() 
        	{
                int[] ticketsToCancel = {1, 3, 2};
                for (int ticket : ticketsToCancel) //ticket = 3
                {
                    ticketSystem.cancelTicket(ticket);
                    try 
                    {
                        Thread.sleep(1500);  // give some time b/w cancellation
                    } 
                    catch (InterruptedException e) 
                    {
                        e.printStackTrace();
                    }
                }
            }
        };
        cancellationThread.start();
        
        
        
        
    }
}

---------------------------------------------------------------
package com.ravi.itc;

class Resource 
{
    private boolean flag = false;

    public synchronized void waitMethod() 
	{
		System.out.println("Wait");  //child1  wait  child1 is waiting Child1 notif
		
       	while (!flag)
		{
          try 
		  {
             System.out.println(Thread.currentThread().getName() + " is waiting...");
             System.out.println(Thread.currentThread().getName()+" is Waiting for Notification");
             wait(); 
          } 
		  catch (InterruptedException e) 
		  {
                e.printStackTrace();
          }
        }
        System.out.println(Thread.currentThread().getName() + " thread completed!!");
    }

    public synchronized void setMethod() 
	{
		System.out.println("notifyAll");
		this.flag = true;
        System.out.println(Thread.currentThread().getName() + " has make flag value as a true");
        notifyAll(); // Notify all waiting threads that the signal is set
    }
}

public class ITCDemo6
{
    public static void main(String[] args) 
		{   	
    	
    	
        Resource r1 = new Resource(); //lock is created

        Thread t1 = new Thread(() -> r1.waitMethod(), "Child1");
		Thread t2 = new Thread(() -> r1.waitMethod(), "Child2");
		Thread t3 = new Thread(() -> r1.waitMethod(), "Child3");

		t1.start();
        t2.start();
        t3.start();
       
		
		Thread setter = new Thread(() -> r1.setMethod(), "Setter_Thread");
      
		   try 
			{
	            Thread.sleep(2000);
	        } 
			catch (InterruptedException e) 
			{
	            e.printStackTrace();
	        }
		
	       setter.start();
    }
}
---------------------------------------------------------------
ThreadGroup :
------------
It is a predefined class available in java.lang Package.

By using ThreadGroup class we can put 'n' number of threads into a single group to perform some common/different operation.

By using ThreadGroup class constructor, we can assign the name of group under which all the thread will be executed.

ThreadGroup tg = new ThreadGroup(String groupName);

ThreadGroup class has provided the following methods :

public String getName() : To get the name of the Group

pubic int activeCount() : How many threads are alive and running under that particular group.

Thread class has provided constructor to put the thread into particular group.

Thread t1 = new Thread(ThreadGroup tg, Runnable target, String name);

By using ThreadGroup class, multiple threads will be executed under single group.

package com.ravi.group;

class Foo implements Runnable
{
	@Override
	public void run() 
	{
		String name = Thread.currentThread().getName();
	System.out.println(name+" is Enjoying Full Stack Developer under Batch 38");
		
	}	
}
 
public class ThreadGroupDemo1 
{
  public static void main(String[] args) throws InterruptedException 
  {
	  
	 ThreadGroup tg = new ThreadGroup("Batch 38");
	 
	 Thread t1 = new Thread(tg, new Foo(), "Scott");
	 Thread t2 = new Thread(tg, new Foo(), "Smith");
	 Thread t3 = new Thread(tg, new Foo(), "Alen");
	 Thread t4 = new Thread(tg, new Foo(), "John");
	 Thread t5 = new Thread(tg, new Foo(), "Martin");
	 
	 t1.start();
	 t2.start();
	 t3.start();
	 t4.start();
	 t5.start();
	 
	 //Thread.sleep(5000);
	 
	System.out.println("How many threads are active under Batch 38 group :"+tg.activeCount());
	 
	System.out.println("Name of the the Group is :"+tg.getName());
	 
  }
}
---------------------------------------------------------------
package com.ravi.group;

public class ThreadGroupDemo2 {

	public static void main(String[] args) 
	{
		Thread t = Thread.currentThread();
		System.out.println(t.toString());
	}

}



Output : Thread[#1main, 5, main]

Here first main is the name of the Thread, 5 is the priority and last main represents group name.

Whenever we define a main method then internally, main group is created and under this main group main thread is executed.
---------------------------------------------------------------
package com.ravi.group;

class TatkalTicket implements Runnable
{
	@Override
	public void run() 
	{
		String name = Thread.currentThread().getName();
		System.out.println("Tatkal ticket booked by :"+name);
		
	}
	
}

class PremiumTatkal implements Runnable
{
	@Override
	public void run() 
	{
		String name = Thread.currentThread().getName();
		System.out.println("Premium Tatkal ticket booked by :"+name);
		
	}	
}

public class ThreadGroupDemo3 
{
	public static void main(String[] args) 
	{
		ThreadGroup tg1 = new ThreadGroup("Tatkal Ticket");		
		ThreadGroup tg2 = new ThreadGroup("Premium Tatkal");
		
		Thread t1 = new Thread(tg1,new TatkalTicket(), "Scott");		
		Thread t2 = new Thread(tg2,new PremiumTatkal(), "Smith");
		
		t1.start(); t2.start();		

	}

}
---------------------------------------------------------------
20-12-2024
------------
Daemon Thread [Service Level Thread]
------------------------------------
Daemon thread is a low- priority thread which is used to provide background maintenance.  

The main purpose of of Daemon thread to provide services to the user thread.              

JVM can't terminate the program till any of the non-daemon (user) thread is active, once all the user thread will be completed then JVM will automatically terminate all Daemon threads, which are running in the background to support user threads.

The example of Daemon thread is Garbage Collection thread, which is running in the background for memory management.

In order to make a thread as a  Daemon thread , we should use setDaemon(true) which is a non static method Thread class.

-------------------------------------------------------------
public class DaemonThreadDemo1 
{
  public static void main(String[] args) 
	{
	    System.out.println("Main Thread Started...");

        Thread daemonThread = new Thread(() -> 
		{
            while (true) 
			{
                System.out.println("Daemon Thread is running...");
                try 
				{
                    Thread.sleep(1000);
                } 
				catch (InterruptedException e) 
				{
                    e.printStackTrace();
                }
            }
        });

        daemonThread.setDaemon(true); 
        daemonThread.start();

        
        Thread userThread = new Thread(() -> 
		{
            for (int i=1; i<=9; i++) 
			{
                System.out.println("User Thread: " + i);
                try 
				{
                    Thread.sleep(2000);
                } 
				catch (InterruptedException e) 
				{
                    e.printStackTrace();
                }
            }
        });

        userThread.start();

        System.out.println("Main Thread Ended...");
    }
}
------------------------------------------------------------
interrupt Method of Thread class :
----------------------------------
It is a predefined non static method of Thread class. The main purpose of this method to disturb the execution of the Thread, if the thread is in waiting or sleeping state.

Whenever a thread is interupted then it throws InterruptedException so the thread (if it is in sleeping or waiting mode) will get a chance to come out from a particular logic.

Points :-
---------
If we call interrupt method and if the thread is not in sleeping or waiting state then it will behave normally.

If we call interrupt method and if the thread is in sleeping or waiting state then we can stop the thread  gracefully.

*Overall interrupt method is mainly used to interrupt the
thread safely so we can manage the resources easily.

Methods :
---------
1) public void interrupt () :- Used to interrupt the Thread but the thread must be in sleeping or waiting mode.

2) public boolean isInterrupted() :- Used to verify whether thread is interrupted or not.
-----------------------------------------------------------
class Interrupt extends Thread
{
   @Override
   public void run()
	{
	   Thread t = Thread.currentThread();
	   System.out.println(t.isInterrupted()); 
       
	   for(int i=1; i<=5; i++)
		{
		   System.out.println(i);  
           try
		   {
			Thread.sleep(1000);
		   }
		   catch (Exception e)
		   {
			   System.err.println("Thread is Interrupted ");
			   e.printStackTrace();
		   }
		   
		}
	}
}
public class  InterruptThread
{
	public static void main(String[] args) 
	{
        Interrupt it = new Interrupt();
		System.out.println(it.getState());  //NEW
		it.start();
		it.interrupt();  //main thread is interrupting the child thread	
	}
}

Note : main thread is interrupting child thread.
-------------------------------------------------------------
class Interrupt extends Thread
{
   @Override
   public void run()
	{
	   try
	   {
	    Thread.currentThread().interrupt();

	   for(int i=1; i<=10; i++)
		{
		   System.out.println("i value is :"+i);
		   Thread.sleep(1000);
		}

	   }
		catch (InterruptedException e)
		{
			System.err.println("Thread is Interrupted :"+e);
		}
		System.out.println("Child thread completed...");
	}
}
public class  InterruptThread1
{
	public static void main(String[] args) 
	{	
		Interrupt it = new Interrupt();
		it.start();	
	}
}

Note : Here Child Thread is interrupting itself.
------------------------------------------------------------
public class InterruptThread2
{
    public static void main(String[] args) 
	{
        Thread thread = new Thread(new MyRunnable());
        thread.start();
     
        try 
		{
          Thread.sleep(3000); //Main thread is waiting for 3 Sec
        } 
		catch (InterruptedException e) 
		{
            e.printStackTrace();
        }
        
       thread.interrupt();  
    }
}

class MyRunnable implements Runnable 
{
    @Override
    public void run() 
	{
        try 
		{
            while (!Thread.currentThread().isInterrupted())
			{
                System.out.println("Thread is running by locking the resource");
                Thread.sleep(500);
            }
        } 
		catch (Exception e) 
		{
            System.out.println("Thread interrupted gracefully.");
        } 
		finally 
		{
            System.out.println("Thread resource can be release here.");
        }
    }
}
------------------------------------------------------------
Deadlock :
------------
It is a situation where two or more than two threads are in blocked state forever, here threads are waiting to acquire another thread resource without releasing it's own resource.

This situation happens when multiple threads demands same resource without releasing its own attached resource so as a result we get Deadlock situation and our execution of the program will go to an infinite state as shown in the diagram. (20-DEC-24)

public class DeadlockExample
	{
  public static void main(String[] args) 
	 {
     String resource1 = "Ameerpet";  
     String resource2 = "S R Nagar";  

    // t1 tries to lock resource1 then resource2

    Thread t1 = new Thread() 
		{
	  @Override
      public void run() 
		  {
			  synchronized (resource1) 
				  {
			   System.out.println("Thread 1: locked resource 1");
			      try 
				   { 
				   Thread.sleep(1000);
				   } 
				   catch (Exception e) 
				   {}				  
				  				  
			   synchronized (resource2) //Nested synchronized block
			   {
				System.out.println("Thread 1: locked resource 2");
			   }
             }
      }
    };


    // t2 tries to lock resource2 then resource1
    Thread t2 = new Thread() 
	{
      @Override
      public void run() 
	  {
        synchronized (resource2) 
			{
          System.out.println("Thread 2: locked resource 2");
              try 
			  { 
			  Thread.sleep(1000);
			  } 
			  catch (Exception e) 
			  {}

          synchronized (resource1) //Nested synchronized block
		  {
            System.out.println("Thread 2: locked resource 1");
          }
        }
      }
    };    
    t1.start();
    t2.start();
  }
}

Note : Both the threads are in infinite waiting state so it is Deadlock situation.
------------------------------------------------------------
21-12-2024
----------
Remaining methods of Object class :
------------------------------------
protected native Object clone() throws CloneNotSupportedException
---------------------------------------------------------------
Object cloning in Java is the process of creating an exact copy of the original object. In other words, it is a way of creating a new object by copying all the data and attributes from the original object. 

The clone method of Object class creates an exact copy of an object.

In order to use clone() method , a class must implements Clonable interface because we can perform cloning operation on Cloneable objects only [JVM must have additional information] otherwise cloning opertion is not possible and JVM will throw an exception at runtime java.lang.CloneNotSupportedException

We can say an object is a Cloneable object if the corresponding class implements Cloneable interface.

It throws a checked Exception i.e CloneNotSupportedException

Note :- clone() method is not the part of Clonable interface[marker interface], actually it is the method of Object class.

clone() method of Object class follows deep copy concept so hashcode will be different as well as if we modify one object content then another object content will not be modified.

clone() method of Object class has protected access modifier so we need to override clone() method in sub class.

Steps we need to follow to perform clone operation :
----------------------------------------------------
1) The class must implements Cloneable interface
2) Override clone method [throws OR try catch]
3) In this Overridden method call Object class clone method
4) Perform downcasting at the time creating duplicate object
5) Two different objects are created [deep copy]

CloneMethodOperation.java
--------------------------
package com.ravi.cloneable;

class Customer implements Cloneable
{
	private Integer customerId;
	private String customerName;
	
	public Customer(Integer customerId, String customerName)
	{
		super();
		this.customerId = customerId;
		this.customerName = customerName;
	}

	@Override
	public String toString() 
	{			
		return "Customer [customerId=" + customerId + ", customerName=" + customerName + "]";
	}
	
	
	public void setCustomerId(Integer customerId) 
	{
		this.customerId = customerId;
	}

	public void setCustomerName(String customerName) 
	{
		this.customerName = customerName;
	}

	@Override
	protected Object clone() throws CloneNotSupportedException
	{
		return super.clone();
	}	
}

public class CloneMethodOperation
{
	public static void main(String[] args) throws CloneNotSupportedException 
	{
       Customer c1 = new Customer(111, "Scott");                
       Customer c2 = (Customer) c1.clone();  //Down casting
       
       System.out.println("Before Change :");
       System.out.println(c1);
       System.out.println(c2);
   
       System.out.println("After Change :");
       c2.setCustomerId(222);
       c2.setCustomerName("Smith");
       System.out.println(c1);
       System.out.println(c2);
       
       System.out.println(".............");
       System.out.println(c1.hashCode());
       System.out.println(c2.hashCode());
             
	}
}
-------------------------------------------------------------

protected void finalize() throws Throwable :
---------------------------------------------
It is a predefined method of Object class.

Garbage Collector automatically call this method just before an object is eligible for garbage collection to perform clean-up activity.

Here clean-up activity means closing the resources associated with that object like file connection, database connection, network connection and so on we can say resource de-allocation.

Note :- JVM calls finalize method only one per object.

         This method is deprecated from java 9V.



class Student
{
	private Integer studentId;
	private String studentName;
	public Student(Integer studentId, String studentName)
	{
		super();
		this.studentId = studentId;
		this.studentName = studentName;
	}
	@Override
	public String toString() 
	{
		return "Student [studentId=" + studentId + ", studentName=" + studentName + "]";
	}
	
	@Override
	public void finalize()
	{
	  System.out.println("Finalize method is invoked");	
	}	
}
public class FinalizeDemo 
{
	public static void main(String[] args) throws InterruptedException 
	{
		Student s1 = new Student(111, "Raj");		
		System.out.println(s1);
		
		s1 = null;
		
		System.gc();  //calling garbage collector explicitly
		
		Thread.sleep(4000);
		
		System.out.println(s1);
		

	}

}

Note : If Student object is eligible for GC then JVM will defnetly call finalize() method of Student class.
-------------------------------------------------------------

*What is the difference between final, finally and finalize

final :- It is a keyword which is used to provide some kind of          restriction like class is final, Method is
         final,variable is final.

finally :- if we open any resource as a part of try block then       that particular resource must be closed inside 
	   finally block otherwise program will be terminated ab-normally and the corresponding resource will not be closed (because the remaining lines of try block will not be executed)

finalize() :- It is a method which JVM is calling automatically just before object  destruction so if any resource (database, file and network) is associated with
	      that particular object then it will be closed
	      or de-allocated by JVM by calling finalize().
------------------------------------------------------------
