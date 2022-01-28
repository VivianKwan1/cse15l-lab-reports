# Debugging `MarkdownParse`
## Errors
* [Infinite Loop](#infinite-loop)
* [Excluding Images](#excluding-images)
* [Error if Link was at the Beginning of the File](#error-if-link-was-at-the-beginning-of-the-file)

## Infinite Loop
* Change Made:
![Image](https://media.discordapp.net/attachments/487122748162834432/936424948254261258/unknown.png?width=2880&height=1151)
* [Failure-Inducing File](https://github.com/agurel33/markdown-parse/blob/main/test-file2.md)
* Symptom: Infinite Loop
![Image](https://media.discordapp.net/attachments/487122748162834432/936439543287123968/ss1.PNG)
* In the original code, there was an assumption that the file would end with a link, since `currentIndex = closeParen + 1`. However, if there was text after the last link, like in the test file, `currentIndex` wouldn’t ever be greater than `markdown.length()`, so it wouldn’t exit the while loop.

## Images
* Change Made: 
![Image](https://media.discordapp.net/attachments/487122748162834432/936428608413634610/unknown.png?width=2880&height=1141)
    * Note: this screenshot also includes the changes from the infinite loop commit above because it was made by a different person, but the changes to exclude images are on lines 26-28
* [Failure-Inducing File](https://github.com/ericwpei/markdown-parse/blob/main/test-file4.md)
* Symptom: Incorrect Output Including Image Links
![Image](https://media.discordapp.net/attachments/487122748162834432/936439543597527080/ss2.PNG)
    * Expected "[https://random.com]"
* Before this change, the code only searched for brackets and parentheses but did not account for images, which have a very similar format. As a result, it would also print the image links within the file.

## Error if Link was at the Beginning of the File
* Change Made:
![Image](https://media.discordapp.net/attachments/487122748162834432/936435944704335872/unknown.png?width=2730&height=1496)
     * Note: Once again, this screenshot includes changes from both commits above because it was made by a different person, but the change specifically fixing this issue is the if/else statement on lines 29-37
* [Failure-inducing file](https://github.com/VivianKwan1/markdown-parse/blob/main/test-file2.md)
* Symptom: `IndexOutOfBoundsException`
![Image](https://media.discordapp.net/attachments/487122748162834432/936439543907897354/ss3.PNG)
* In the previous change accommodating for images, we added an if statement to check if the character prior to the first bracket was an !. However, if the first bracket was the first character this would result in an `IndexOutOFBoundsException`, which was the focus of this change.