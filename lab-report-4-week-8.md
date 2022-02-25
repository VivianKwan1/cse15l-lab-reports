# Tests for `markdown-parse`
## Table of Contents
* [Snippet 1](#snippet-1)
* [Snippet 2](#snippet-2)
* [Snippet 3](#snippet-3)

[My repository](https://github.com/VivianKwan1/markdown-parse)

[Reviewed repository](https://github.com/merrickqiu/markdown-parse)
### Snippet 1
Expected output:

![Image](https://cdn.discordapp.com/attachments/487122748162834432/946567095246802955/unknown.png)
Links printed should be "`google.com", "google.com", and "ucsd.edu"

Test:

![Image](https://media.discordapp.net/attachments/487122748162834432/946566706359328790/unknown.png)

My `markdown-parse` fails:

![Image](https://cdn.discordapp.com/attachments/487122748162834432/946567666972389417/unknown.png)

Reviewed `markdown-parse` fails:

![Image](https://media.discordapp.net/attachments/487122748162834432/946584504519245844/unknown.png?width=705&height=105)

This may be a more involved change, as there are multiple issues here. First, our code does not consider the fact that there may be a bracket in the link text, which is why it does not properly register ucsd.edu as a link. We would likely need another conditional to check for more close brackets beyond the first. Second, our code does not consider code snippets at all, which is why it sees url.com as a link. We would likely need more conditionals and potentially variables for the code snippets to make this fix.
### Snippet 2
Expected output:

![Image](https://media.discordapp.net/attachments/487122748162834432/946568826198323271/unknown.png?width=705&height=144)
Links printed should be "a.com", "a.com(())", and "example.com"

Test:

![Image](https://cdn.discordapp.com/attachments/487122748162834432/946571579079417886/unknown.png)

My `markdown-parse` fails:

![Image](https://media.discordapp.net/attachments/487122748162834432/946570857566847006/unknown.png)

Reviewed `markdown-parse` fails:

![Image](https://media.discordapp.net/attachments/487122748162834432/946584546848161792/unknown.png)

This may be a somewhere between a small (10 lines) fix and a more involved change, as there are multiple issues here but they are a bit simpler. Once again, our code does not consider the fact that there may be a bracket in the link text, which is why it does not properly register example.com as a link. We would likely need another conditional to check for more close brackets beyond the first. Second, our code does not consider that there may be a close parentheses within the link, so it cuts the link off at the first close parentheses and returns a.com(( and not a.com(()). We would likely need more conditionals for the code snippets to make this fix.
### Snippet 3
Expected output:

![Image](https://cdn.discordapp.com/attachments/487122748162834432/946572527994556426/unknown.png)
Link printed should be "https://ucsd-cse15l-wi22.github.io/"

Test:

![Image](https://media.discordapp.net/attachments/487122748162834432/946582103351525426/unknown.png)

My `markdown-parse`

![Image](https://cdn.discordapp.com/attachments/487122748162834432/946582371921190963/unknown.png)

Reviewed `markdown-parse` fails:

![Image](https://media.discordapp.net/attachments/487122748162834432/946584590447968356/unknown.png)

This would be a more involved change, as we currently have a conditional checking for new lines, which prevents https://twitter.com and https://cse.ucsd.edu/ from being printed (correctly) and also allows another test to pass, but this means it's also not printing https://ucsd-cse15l-wi22.github.io/ like it should. We would need to incorporate more specific conditionals to make sure the other test passes while also not preventing all links with new lines.

