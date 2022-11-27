Arvin Zhang
(Collaborated with Dima Demler in lab)

# Lab Report 5 - Autograder

# Code Block:

Code from grade.sh

```
# used some ideas from this the repository https://github.com/kNelakonda/list-examples-grader/blob/main/grade.sh
FSCORE=0
MAXSCORE=100

function score_message() {
    printf "Student recieved $1/$2 \n"
    exit 0
}

rm -rf student-submission
git clone $1 student-submission
echo 'Finished cloning'

cp TestListExamples.java ./student-submission/

if [ -f ./student-submission/ListExamples.java ]; then
    echo found
    FSCORE=$((FSCORE + 10))
else
    echo not found java file
    score_message $FSCORE $MAXSCORE
fi

javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar student-submission/TestListExamples.java student-submission/ListExamples.java

if [[ $? -ne 0 ]]; then
    printf "Compiler did not work\n"
    score_message $FSCORE $MAXSCORE
fi

FSCORE=$((FSCORE + 20))

cd student-submission/

printf "_________running java now__________\n"

java -cp .:../lib/hamcrest-core-1.3.jar:../lib/junit-4.13.2.jar org.junit.runner.JUnitCore TestListExamples >output.txt

if [[ $? -eq 0 ]]; then
    cat output.txt
    FSCORE=$((FSCORE + 70))
else
    cat output.txt
    t=$(grep -i "Tests run" output.txt)
    tests=$(echo $t | cut -d ' ' -f 3 | cut -d ',' -f 1)
    failed=$(echo $t | cut -d ' ' -f 5)

    echo $((tests - failed))/$tests tests passed

    FSCORE=$((FSCORE + 35 * (tests - failed)))

fi

score_message $FSCORE $MAXSCORE
```

# Example 1

# Example 2

# Example 3

# Tracing Example 1

In example one, I tested grade.sh on this repository: https://github.com/ucsd-cse15l-f22/list-methods-lab3

## Line 1-4

```
FSCORE=0
MAXSCORE=100
```

Defines variables FSCORE and MAXSCORE which is updated later on in the program to calculate the scores of the student submission with each test case. 

