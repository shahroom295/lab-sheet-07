# lab-sheet-07
code

// Q1. Extending Thread Class
class Q1 extends Thread {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Shahroom");
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                System.out.println(e);
            }
        }
    }
}

// Q2. Using Runnable Interface
class Q2 implements Runnable {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Shahroom");
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                System.out.println(e);
            }
        }
    }
}

// Q3. Thread Name and Priority
class Q3 {
    static void run() {
        Thread t = Thread.currentThread();

        System.out.println("Name: " + t.getName());
        System.out.println("Priority: " + t.getPriority());

        t.setName("MyThread");
        System.out.println("New Name: " + t.getName());
    }
}

// Q4. Two Threads
class Q4 extends Thread {
    Q4(String name) {
        super(name);
    }

    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(getName() + ": " + i);
        }
    }
}

// Q5. Three Threads with Random Sleep
class Q5 extends Thread {
    Q5(String name) {
        super(name);
    }

    public void run() {
        try {
            Thread.sleep((int)(Math.random() * 501));
        } catch (InterruptedException e) {
            System.out.println(e);
        }
        System.out.println(getName() + " is working");
    }
}

// Q6. isAlive() Method
class Q6 extends Thread {
    public void run() {
        System.out.println("Thread is running");
    }

    static void check() throws Exception {
        Q6 t = new Q6();

        System.out.println(t.isAlive());
        t.start();
        System.out.println(t.isAlive());

        t.join();
        System.out.println(t.isAlive());
    }
}

// Q7. join() Method
class Q7 extends Thread {
    public void run() {
        long sum = 0;
        for (int i = 1; i <= 10000; i++) {
            sum += i;
        }
        System.out.println("Sum: " + sum);
    }
}

// Q8. Thread Priority
class Q8 extends Thread {
    Q8(String name) {
        super(name);
    }

    public void run() {
        System.out.println(getName() + " Priority: " + getPriority());
    }
}

// Q9. Five Threads with Different Priorities
class Q9 extends Thread {
    Q9(String name, int p) {
        super(name);
        setPriority(p);
    }

    public void run() {
        for (int i = 1; i <= 3; i++) {
            System.out.println(getName() + " " + i);
        }
    }
}

// Q10, Q11, Q12. Bank Account
class BankAccount {
    int balance = 10000;
    private final Object lock = new Object();

    // Q10: Not Synchronized
    void withdraw(int amount) {
        if (balance >= amount) {
            balance -= amount;
        }
    }

    // Q11: Synchronized Method
    synchronized void withdrawSync(int amount) {
        if (balance >= amount) {
            balance -= amount;
        }
    }

    // Q12: Synchronized Block
    void withdrawBlock(int amount) {
        synchronized (lock) {
            if (balance >= amount) {
                balance -= amount;
            }
        }
    }
}

class WithdrawThread extends Thread {
    BankAccount account;
    int type;

    WithdrawThread(BankAccount a, int t) {
        account = a;
        type = t;
    }

    public void run() {
        for (int i = 1; i <= 1000; i++) {
            if (type == 10)
                account.withdraw(1);
            else if (type == 11)
                account.withdrawSync(1);
            else
                account.withdrawBlock(1);
        }
    }
}

// Q13. Producer Consumer using wait() and notify()
class Buffer {
    int data;
    boolean available = false;

    synchronized void produce(int n) throws Exception {
        while (available) {
            wait();
        }
        data = n;
        available = true;
        System.out.println("Produced: " + n);
        notifyAll();
    }

    synchronized void consume() throws Exception {
        while (!available) {
            wait();
        }
        System.out.println("Consumed: " + data);
        available = false;
        notifyAll();
    }
}

// Q14. Deadlock Example
class Q14 {
    static final Object Lock1 = new Object();
    static final Object Lock2 = new Object();

    static void run() {
        Thread t1 = new Thread(() -> {
            synchronized (Lock1) {
                System.out.println("Thread 1: Lock1");
                synchronized (Lock2) {
                    System.out.println("Thread 1: Lock2");
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (Lock2) {
                System.out.println("Thread 2: Lock2");
                synchronized (Lock1) {
                    System.out.println("Thread 2: Lock1");
                }
            }
        });

        t1.start();
        t2.start();
    }
}

// Q15. Deadlock Prevention
class Q15 {
    static final Object Lock1 = new Object();
    static final Object Lock2 = new Object();

    static void run() {
        Thread t1 = new Thread(() -> {
            synchronized (Lock1) {
                synchronized (Lock2) {
                    System.out.println("Thread 1 completed");
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (Lock1) {
                synchronized (Lock2) {
                    System.out.println("Thread 2 completed");
                }
            }
        });

        t1.start();
        t2.start();
    }
}

// MAIN CLASS
public class Main {
    public static void main(String[] args) throws Exception {

        // Q1
        Q1 a = new Q1();
        a.start();
        a.join();

        // Q2
        Thread b = new Thread(new Q2());
        b.start();
        b.join();

        // Q3
        Q3.run();

        // Q4
        Q4 c1 = new Q4("Thread1");
        Q4 c2 = new Q4("Thread2");
        c1.start();
        c2.start();
        c1.join();
        c2.join();

        // Q5
        Q5 r1 = new Q5("Reader");
        Q5 r2 = new Q5("Writer");
        Q5 r3 = new Q5("Logger");
        r1.start();
        r2.start();
        r3.start();
        r1.join();
        r2.join();
        r3.join();

        // Q6
        Q6.check();

        // Q7
        Q7 task = new Q7();
        task.start();
        task.join();
        System.out.println("Task finished");

        // Q8
        Q8 high = new Q8("High");
        Q8 low = new Q8("Low");
        high.setPriority(Thread.MAX_PRIORITY);
        low.setPriority(Thread.MIN_PRIORITY);
        System.out.println(high.getPriority());
        System.out.println(low.getPriority());
        high.start();
        low.start();
        high.join();
        low.join();

        // Q9
        for (int i = 1; i <= 5; i++) {
            Q9 t = new Q9("Thread" + i, i * 2);
            t.start();
        }

        // Q10
        BankAccount acc = new BankAccount();
        WithdrawThread w1 = new WithdrawThread(acc, 10);
        WithdrawThread w2 = new WithdrawThread(acc, 10);
        w1.start();
        w2.start();
        w1.join();
        w2.join();
        System.out.println("Q10 Balance: " + acc.balance);

        // Q11
        acc = new BankAccount();
        w1 = new WithdrawThread(acc, 11);
        w2 = new WithdrawThread(acc, 11);
        w1.start();
        w2.start();
        w1.join();
        w2.join();
        System.out.println("Q11 Balance: " + acc.balance);

        // Q12
        acc = new BankAccount();
        w1 = new WithdrawThread(acc, 12);
        w2 = new WithdrawThread(acc, 12);
        w1.start();
        w2.start();
        w1.join();
        w2.join();
        System.out.println("Q12 Balance: " + acc.balance);

        // Q13
        Buffer buffer = new Buffer();

        Thread producer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    buffer.produce(i);
                }
            } catch (Exception e) {
                System.out.println(e);
            }
        });

        Thread consumer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    buffer.consume();
                }
            } catch (Exception e) {
                System.out.println(e);
            }
        });

        producer.start();
        consumer.start();
        producer.join();
        consumer.join();

        // Q14 and Q15 are run separately.
        // Q14.run(); // Deadlock example
        Q15.run();   // Deadlock prevention
    }
}

