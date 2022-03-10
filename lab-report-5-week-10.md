# Debugging commonmark-spec Tests

## Table of Contents
* [Finding Differences](#finding-differences)
* [487.md](#487md)
* [495.md](#495md)

## Finding Differences
To find the tests with different results, I used `diff` on the results of running a bash for loop. Then I went to the specified line to find the test (which was printed before the result).\
Sample of `diff`:\
![Image](https://cdn.discordapp.com/attachments/487122748162834432/951461180835258368/unknown.png)

## 487.md
The test:\
![Image](https://media.discordapp.net/attachments/487122748162834432/951463900459708506/unknown.png)\
In this test, the expected outcome is `[]`.\
Results from `diff`:\
![Image](https://cdn.discordapp.com/attachments/487122748162834432/951464255968915466/unknown.png)

My implementation was incorrect and the provided was correct. This bug occurs because my implementation does not check for \ within parentheses. I would need a conditional checking for \ within parentheses but another conditional checking for `<>`, as the `<>` would allow the \ to be a link (demonstrated below):\
The `<>` (488.md):\
![Image](https://cdn.discordapp.com/attachments/487122748162834432/951464970514755605/unknown.png)\
Where conditionals would be inserted:
![Image](https://cdn.discordapp.com/attachments/487122748162834432/951465113188188180/unknown.png)

## 495.md
The test:\
![Image](https://cdn.discordapp.com/attachments/487122748162834432/951460918557028352/unknown.png) \
In this test, the expected outcome is `[foo(and(bar))]`.\
Results from `diff`:\
![Image](https://cdn.discordapp.com/attachments/487122748162834432/951461492157472828/unknown.png)

My implementation was incorrect and the provided was correct. This bug occurs because my implementation takes the first close parentheses as the end of the link and does not consider the possibility of a parentheses in the link. I would probably need some conditionals here to determine if there is another valid close parentheses after the first:\
![Image](https://cdn.discordapp.com/attachments/487122748162834432/951462188202201128/unknown.png)