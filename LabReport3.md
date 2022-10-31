Arvin Zhang

# Lab Report 3 - Researching Commands

## Grep Command

Overview: The grep command allows the user to locate specific lines with certain strings of characters inside specific files.

### grep -i

The -i command line option tells the grep command to be case insensitive and ignore all cases.

#### Example 1:

Command:
`grep -i AwarENesS 911report/chapter-1.txt`

When ran in the working directory of ./technical, this command will find all of the instances of the term "awareness" in 911report/chapter-1.txt while ignoring cases.

Output:
![grep -i Awareness](./LabReport3Imgs/grep-iAwareness.png)

Analysis:
As seen in the output above. When running grep -i with a mixed cased string of characters, "AwarENesS," the terminal finds all cases of "AwarENesS." The first two "awareness" has no capital letters, but the last one in the image has a capital "A." Each term found is also highlighted for the user to see and its surrounding words are also printed out. This command line option is useful because it allows the user to search for all instances of a term regardless of cases instead of searching for each and every possible case combination.

### Example 2:
