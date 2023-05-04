Download Link: https://assignmentchef.com/product/solved-cse-330-operating-systems-project-3
<br>
<strong>Implementing Semaphores</strong>




Using the threads, you have implemented, implement semaphores. Since the threads are non-preemptive, you do not need to ensure atomicity of the semaphores (they are already atomic).




Implement the following: (you can either create file called sem.h or write it in your main c file)




<ol>

 <li><strong>Semaphore data structure: </strong>A value field and a queue of TCBs.</li>

</ol>










<ol start="2">

 <li><strong>InitSem(semaphore, value): </strong>Initializes the value field with the specified value.</li>

</ol>










<ol start="3">

 <li><strong>P(semaphore): </strong>The P routine decrements the semaphore, and if the value is 0 or less than zero then blocks the process in the queue associated with the semaphore.</li>

</ol>










<ol start="4">

 <li><strong>V(semaphore): </strong>The V routine increments the semaphore, and if the value is 0 or negative, then takes a PCB out of the semaphore queue and puts it into the run queue. Note: <strong>The V routine also “<em><u>yields</u></em>” to the next runnable process.</strong> //this is important.</li>

</ol>










<ol start="5">

 <li>Implement a solution to the Producer Consumer Problem with the following settings: <em><u>There are P producers all in while loops that loop N times, each producing 1 item in one loop</u>. </em></li>

</ol>

<em><u>There are C consumers all in while loops that loop N times each consuming 1 item in one loop</u></em><em>. </em>

<em><u>Every consumer or producer yields at the end of a loop to the next process</u></em><em>.</em>




<em><u>Consider that the buffer size is B items.</u></em>

<em><u>You will be given a Queue of numbers, where positive numbers denote the producer ID and negative numbers denote consumer ID</u></em>

<em><u>Arrange your ready queue accordingly</u></em>

<em><u>Start with the first thread in the queue and run it.</u></em>

<em><u>At the end of each loop before yield a consumer should print the following: if consumer X is consuming an item then print</u></em>

<em><u>printf”
 Consumer %d is consuming item generated by Producer %d”,X,Y) else print</u></em>

<em><u>printf(“
 Consumer %d is waiting
”,X)</u></em>

<em><u>At the end of each loop before yield the producer should print the following:</u></em>

<em><u>if producer X is producing an item then print</u></em>

<em><u>printf(“Producer %d is producing item number %d”, X,Y)</u></em>

<em><u>else print</u></em>

<em><u>printf(“
 Producer %d is waiting 
”, X)</u></em>

<em><u> </u></em>

<strong>How will you implement P(S) and V(S)?</strong>

<strong> </strong>

P(S) requires two operations: a) block when S&lt;= 0 and b) let go when S &gt; 0;




The task (b) is easy as nothing needs to be done except for decrementing the semaphore value.




To perform task (a) for every semaphore you declare you have to create a new queue semQ. Whenever a thread executed P(S) and S &lt;= 0 the TCB of the thread gets deleted from the ReadyQ and inserted at the <strong>end</strong> of the semQ. Then the process calls yield(). In this way if the thread remains in the semQ it never gets executed again.




V(S) requires two operations a) increment S and b) unblock <strong>one</strong> process. To unblock a process, you will deleted the <strong>head</strong> of semQ and add at the <strong>end</strong> of the ReadyQ. This way the processes can perform yield and the unblocked process will execute again.




<strong>Test cases</strong>

<strong> </strong>

Similar to project 1, we will test your code using input redirection. The first line of each test case file will be of the form:




B,P,C,N




Where B is the buffer size, P is the number of producers, C is the number of consumers, and N is the number of times producers and consumers run their while loop. (Note all producers and consumers run the same number of while loop).




This line will be followed by P+C numbers (one number in each line). Each number denotes process ID. Positive numbers indicate producer process ID, while negative numbers indicate consumer process ID.




Example test case

2,3,1,2

1

-1

2

3




Example test case output




For this particular test case we have buffer size 2, 3 producers and 1 consumer, and each producer or consumer executes their while loops 2 times.

We have two semaphores here: a) Full (for the Consumer) and b) Empty (for the Producer).




Initially the ready queue looks like the following




ReadyQ à 1 -1 2 3




Our Buffer is initially empty




The Full and Empty queues remain empty




The first producer produces an item and prints




<em>Producer 1 is producing item number 1</em>




After this print the producer yields so the new ready queue looks like the following




ReadyQ à -1 2 3 1




Buffer à <u>P1</u>  <u>__</u>




The first consumer then executes and prints




<em>Consumer 1 is consuming item generated by Producer 1</em>




The ready queue now looks like the following




ReadyQ à 2 3 1 -1




Buffer à __    __




The second producer then executes.




<em>Producer 2 is producing item number 1</em>




Then it yields




ReadyQ à 3 1 -1 2




Buffer à <u>P2</u>    <u>__</u>




Producer 3 then executes




<em>Producer 3 is producing item number 1</em>




ReadyQ à 1 -1 2 3




Buffer à <u>P2</u>  <u>P3</u>

Producer 1 then attempts to produce but the buffer is full so has to wait




<em>Producer 1 is waiting</em>




EmptyQ à P1




ReadyQ à -1 2 3




Consumer 1 then executes. It unblocks P1 from empty queue and hence P1 joins the end of the ready queue. It then prints:




<em>Consumer 1 is consuming item generated by Producer 2</em>




After printing it exits because it is done.




EmptyQ à empty




ReadyQ à 2 3 1




Buffer à <u>P3</u>  __




Process P2 executes




<em>Producer 2 is producing item number 2</em>

<em> </em>

Then producer 2 is done so it exits

ReadyQ à 3  1

Buffer à <u>P3</u>  <u>P2</u>

<u> </u>

Producer 3 then executes but is blocked because buffer is full




<em>Producer 3 is waiting</em>




EmptyQ à P3




ReadyQ à 1




Producer 1 then executes




Producer 1 is waiting




The Ready Q is empty so




The program ends




SO overall output is as follows
















Producer 1 is producing item number 1




Consumer 1 is consuming item generated by Producer 1




Producer 2 is producing item number 1




Producer 3 is producing item number 1




Producer 1 is waiting




Consumer 1 is consuming item generated by Producer 2




Producer 2 is producing item number 2




Producer 3 is waiting




Producer 1 is waiting










<strong>Submission and Grading:</strong>




Your project must consist of 5 files




<ol>

 <li>h (uses ucontext.h)</li>

</ol>




<ol start="2">

 <li>h (includes TCB.h)</li>

</ol>




<ol start="3">

 <li>h (includes q.h)</li>

</ol>




<ol start="4">

 <li>h (includes threads.h) if you have written one.</li>

</ol>




<ol start="5">

 <li>proj-3.c (includes threads.h)</li>

</ol>

(make sure the compile command, “gcc proj-3.c” does the correct compilation).




All 5 files are to be ZIPPED into one zip file and uploaded in Canvas. The name of this file should reflect the name of the student (abbreviated, do not make it too long).




<strong>Note: Grading is on Ubuntu. It is in your interest to make sure the program compiles and runs in the target test platform.</strong>

<strong> </strong>





