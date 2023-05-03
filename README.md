Download Link: https://assignmentchef.com/product/solved-cs135-student-grade-calcuation-structs
<br>
<strong>Purpose:       </strong>After completing this assignment, you will be able to:

<ul>

 <li>Show how to create a user-defined data type</li>

 <li>Demonstrate how to use structs in a C++ program</li>

</ul>

<strong>Task: </strong>Write a C++ program which reads in several files of data containing student grade information and calculates each student’s course grade and outputs them to a file.

<strong>Discussion: </strong>You program should utilize six filenames as command-line arguments. The files are formatted in the following manner:

<ul>

 <li>student info.txt—student id, student name</li>

 <li>assignment info.txt – assignment number, points available</li>

 <li>assignment scores.txt – student id, assignment number, points scored</li>

 <li>txt – exam number, points available, exam weight</li>

 <li>txt – student id, exam number, points scored • output file—formatted according to the example later on.</li>

</ul>

Here is an example of the input files:

$ more student_info.txt

<ul>

 <li>Bill</li>

 <li>Jill</li>

 <li>Fred $</li>

</ul>

$ more assignment_info.txt

<ul>

 <li>10</li>

 <li>100</li>

</ul>

$

$ more assignment_scores.txt

<ul>

 <li>1 9</li>

 <li>1 10</li>

 <li>2 83</li>

 <li>2 97</li>

 <li>2 80</li>

</ul>

$

$ more exam_info.txt

<ul>

 <li>92 .3</li>

 <li>120 .4</li>

</ul>

$

$ more exam_scores.txt

<ul>

 <li>1 78</li>

 <li>1 89</li>

 <li>1 65</li>

 <li>2 85</li>

 <li>2 93</li>

 <li>2 54</li>

</ul>

$

To calculate the course total, use the following rules of thumb:

<ul>

 <li>Assignments are worth 30% of the course grade</li>

 <li>Each exam is worth the indicated weight percentage in the exam info file</li>

 <li>If a student does not have a grade indicated for a particular graded item, then a grade of zero (0) should be assumed. For example, in the output above, Fred does not have a grade for Assignment 1, so Fred receives a 0 for that assignment.</li>

 <li>The overall course grade can be calculated using the formula:</li>

</ul>

(hw per * hw wt) + (ex1 per * ex1 wt) + (ex2 per * ex2 wt) + …+ (exN per * exN wt)

Your program must read in the data using an array of structs. You should define three user-defined data types as structs in your program:

<ul>

 <li>Assignment—

  <ul>

   <li>id (int)</li>

   <li>points available (int)</li>

   <li>points scored (int)</li>

  </ul></li>

</ul>

You should also declare three static variables within the Assignment struct definition:

<ul>

 <li>number of assignments (int), initialized to zero</li>

 <li>total assignment points (int), initialized to zero</li>

 <li>total weight of assignments (float), initialized to the ASSIGNMENT WEIGHT constant</li>

 <li>Exam—</li>

</ul>

<ul>

 <li>id (int)</li>

 <li>points available (int)</li>

 <li>points scored (int)</li>

 <li>percentage (float)</li>

 <li>weight (float)</li>

</ul>

You should also declare one static variable within the Exam struct definition:

<ul>

 <li>number of exams (int), initialized to zero</li>

</ul>

<ul>

 <li>Student—

  <ul>

   <li>id (int)</li>

   <li>name (string)</li>

   <li>assignments (array of Assignment structs)</li>

   <li>assignment percentage (float)</li>

   <li>exams (array of Exam structs)</li>

   <li>grade percentage (float)</li>

   <li>letter grade (string)</li>

  </ul></li>

</ul>

You should also declare one static variable within the Student struct:

<ul>

 <li>number of students (int), initialized to zero</li>

</ul>

You must utilize the provided preprocessor, named constant declarations, and main function to get started with your program. Looking at the main function plus the algorithms required to solve the rest of the program, it is clear you must write the following functions:

<ul>

 <li>readSetupData—a void function that reads the student info, exam info, and assignment info into the array of student structs. This function must take in the student info filename, exam info filename, assignment info filename, and array of Student structs.</li>

 <li>readScoreData—a void function that reads the score data for assignments and exams, and updates the appropriate Student in the array of Student structs for the given info. This function must take in the exam scores filename, the assignment scores filename, and the array of Student structs.</li>

</ul>

The readScoreData function will require you to also define the following helper functions:

<ul>

 <li>findStudent—a value-returning function that takes in a student id and the array of Student structs, and returns the index position of the Student with the given id.</li>

 <li>findExam—a value-returning function that takes in an exam id and an array of Exam structs, and returns the index position of the exam in the array.</li>

 <li>findAssignment—a value-returning function that takes in an assignment id and an array of Assignment structs, and returns the index position of the assignment in the array.</li>

</ul>

Continuing the functions required from looking at the main function:

<ul>

 <li>calculateGrades—a void function that calculates the grade percentages for assignments, exams, and overall course percentage, and the overall letter grade for the course. This function should take in only the array of Student structs.</li>

 <li>writeGradeData—a void function that writes the grading data for each student to the output file. This function should take in the output filename and the array of Student structs.</li>

</ul>

Use the provide code below to get started with your program:

#include &lt;cmath&gt; // round

#include &lt;cstdlib&gt; // atoi, exit

#include &lt;fstream&gt; // ifstream, ofstream

#include &lt;iomanip&gt; // setprecision #include &lt;iostream&gt; // cout, fixed using namespace std;

const int MAX_STUDENTS = 40; const int MAX_ASSIGNMENTS = 20; const int MAX_EXAMS = 3; const float ASSIGNMENT_WEIGHT = .3;

const float DMINUS = 60.0; const float D = 63.0; const float DPLUS = 67.0; const float CMINUS = 70.0; const float C = 73.0; const float CPLUS = 77.0; const float BMINUS = 80.0; const float B = 83.0; const float BPLUS = 87.0; const float AMINUS = 90.0; const float A = 93.0;

// **************

// YOUR CODE HERE // **************

int main(int argc, char *argv[]) { // filename and filestream variables string sfile, efile, afile, escores, ascores, ofile;

// keep track of each Student in an array Student students[MAX_STUDENTS];

// check command-line args if (argc != 7) { cout &lt;&lt; “Must supply required filenames in the correct order:”

&lt;&lt; endl; cout &lt;&lt; ”          ./a.out &lt;studinfo&gt; &lt;exinfo&gt; &lt;hwinfo&gt; &lt;exscores&gt;

&lt;hwscores&gt; &lt;outfile&gt;” &lt;&lt; endl;

exit(1);

}

// grab the command-line args sfile = argv[1]; efile = argv[2]; afile = argv[3]; escores = argv[4]; ascores = argv[5]; ofile = argv[6];

// process the files, do the calculations, and write the output readSetupData(sfile, efile, afile, students); readScoreData(escores, ascores, students); calculateGrades(students); writeGradeData(ofile, students);

return 0;

}

<strong>Criteria:           </strong>Submissions must adhere to the criteria specified in the syllabus. Your program should run like this:

$ g++ cs135-grade-calc.cpp

$ ./a.out stud_info.txt exam_info.txt hw_info.txt exam_scores.txt  hw_scores.txt grades.out

$ more grades.out

Id       Name               Assignments            Exam 1           Exam 2            Crs Grade

———————————————————–

<table width="0">

 <tbody>

  <tr>

   <td width="115">1             Bill</td>

   <td width="115">83.6</td>

   <td width="76">84.8</td>

   <td width="76">70.8</td>

   <td width="69">78.9 (C+)</td>

  </tr>

  <tr>

   <td width="115">2             Jill</td>

   <td width="115">97.3</td>

   <td width="76">96.7</td>

   <td width="76">77.5</td>

   <td width="69">89.2 (B+)</td>

  </tr>

  <tr>

   <td width="115">3           Fred$</td>

   <td width="115">72.7</td>

   <td width="76">70.7</td>

   <td width="76">45.0</td>

   <td width="69">61.0 (D-)</td>

  </tr>

 </tbody>

</table>


