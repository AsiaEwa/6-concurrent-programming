# 6-concurrent-programming
Viability and security violation situations of concurrent programs: deadlock, report and race conditions. How to implement critical sections with mutexes. C++
situations of violating the viability and safety of concurrent programs: deadlocks, announcements and race conditions.

Covers how to implement critical sections using mutexes in the C++ language standard. Critical sections are pieces of code that can only be executed by one thread at a time. Often the problem concerns access to some area of memory with data (e.g. arrays or data structures) or to a stream (e.g. standard output or file).
 
## 2. Mutex class
In thread builders, check with the number 50 and 5000.
a) Without critical section (mutex disabled)
b) With a critical section - comments on the lines in which the lock() and unlock() methods of the mutex are activated.

In a program with mutexes, access to the cout output is blocked. The work is in the order: first one works (while the second one "waits" because it is blocked), then the second one, after finishing the work of the first one, does the work when he already has access.

![image](https://github.com/AsiaEwa/6-concurrent-programming/assets/101841759/5e28b94a-4211-4433-aec0-4b39d6d12fb2)
![image](https://github.com/AsiaEwa/6-concurrent-programming/assets/101841759/f3c79c98-9aca-4ba6-a051-e55e626b7145)

## 3. Lock_guard() wrapper class
The lock_guard() wrapper class is a convenient way to use the mutex mechanism in C++.

The program generates a two-dimensional array of integers and fills it with pseudorandom numbers. Then it creates a second two-dimensional array Tab2, in which it saves the elements of the first array raised to the power of 2. The average values from individual rows of the Tab2 array are saved in a one-dimensional array Average.
Each row of the Tab table and Tab2 table is processed by a separate thread. Pointers to thread objects are stored in two tables: watki_tab2[] and watki_average[].
![image](https://github.com/AsiaEwa/6-concurrent-programming/assets/101841759/492b3519-8cc4-4d1f-8283-70a76020bc20)
![image](https://github.com/AsiaEwa/6-concurrent-programming/assets/101841759/6cdc115b-781a-485a-a77b-11edd2486fd5)

#### Change 1.
Modifications:

Addition: header file for mutex handling:
#include <mutex>

Creation:
global object of the mutex class:
std::mutex mtx;

Added thread handling function: do_square and average by a line of code that starts the mutex using the lock_guard wrapper class.
Placed before the line that prints the array data on the standard output. (a similar modification applies to the "average" function) Square_function after change:

void to_square(int Tab[LDT][LG], int Tab2[LDT][LG], int row_number)

{

for (int column = 0; column < LG; column++) {

int a = Tab[row_no][column];

Tab2[row_number][column] = a*a;

std::lock_guard<std::mutex> lock(mtx);

std::cout << "Tab2[" <<line_no.<<"]["<<column<<"] = "<<Tab2[line_no.][column] <<std::endl;

}

}

![image](https://github.com/AsiaEwa/6-concurrent-programming/assets/101841759/c912067b-3d52-4767-ab13-c845700f3893)
![image](https://github.com/AsiaEwa/6-concurrent-programming/assets/101841759/c4c83930-fc21-4f38-8f63-97e8ae8c7999)

#### Change 2.
moving the line with the mutex object to the beginning of the do_square and average function.
After change:

void to_square(int Tab[LDT][LG], int Tab2[LDT][LG], int row_number)

{

std::lock_guard<std::mutex> lock(mtx);

for (int column = 0; column < LG; column++) {

int a = Tab[row_no][column];

Tab2[row_number][column] = a*a;

std::cout << "Tab2[" <<line_no.<<"]["<<column<<"] = "<<Tab2[line_no.][column] <<std::endl;

}

}

![image](https://github.com/AsiaEwa/6-concurrent-programming/assets/101841759/d37b30cd-7d2b-4d01-893f-36716179d69d)

**After change 1:** Only the thread to cout output is blocked. The mutex blocks the thread when squaring one number or calculating one average. Calculations are performed randomly. The threads count simultaneously and the thread that manages to perform the operations prints its result first. The order of displayed elements is random, the tab and average results are intertwined. Due to mixed access, average values may not be calculated as expected.

**After change 2:** The mutex blocks squaring entire rows and performs an average after each row. All operations within the average and squared functions are blocked. Counting and saving takes place in the following order: first, calculate the table, then write the averages

#### A program 
that creates and fills a two-dimensional array of dimensions 30 x 6 with pseudorandom numbers from 0 to 50. The generated array is displayed on the standard output.

for each row, a new thread is started, which calculates the average value of the elements of this row and stores it in a one-dimensional array Avg[30].

• For each row, a new thread is started that calculates the differences between the values of the elements of the Tab[30][6] array and the average value from the Average[30] array. The result is stored in the Roznice[30][6] table. For example: Differences[1][2] = Tab[1][2] – Averages[1];

• For each row of the Differences table, a new thread is started that calculates the average value of the elements of that row. The result is saved in the Standard_Deviations[30] table. Note that you can (but don't have to) use the same thread handling function that stored the values in the Averages table.

The program displays information on the standard output

![image](https://github.com/AsiaEwa/6-concurrent-programming/assets/101841759/4f61f65f-270f-416b-a49f-c4a98f7caf2a)
