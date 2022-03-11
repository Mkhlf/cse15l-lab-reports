### Lab Report 5

# Using a Script to Run Many Files

- [Using a Script to Run Many Files](#using-a-script-to-run-many-files)
    - [Overview](#overview)
    - [Test \#1](#test-1)
    - [Test \#2](#test-2)

---

### Overview

This lab report will cover and discuss two tests from [this](https://github.com/Mkhlf/markdown-parse/tree/main/test-files) on these two implmentaion of `markdown-parse`:

- [`My MarkdownPrase`](https://github.com/Mkhlf/markdown-parse/commit/885412daa8ffb91912d6ffe77b97c120b833a9c8)
- [`Provided MarkdownPrase`](https://github.com/ucsd-cse15l-w22/markdown-parse)

To begin with the test were runned using this bash script:

```bash
for file in test-files/*.md;
do
  echo $file; java MarkdownParse $file
done

```

Then, the output was saved in two `res.txt` files for both implementation.

To compare both file the `diff` command was used and its output was saved in another `diff.txt` file.

The `diff.txt` file was examined manually along with both of the `res.txt` files to finds the following test cases.

### Test \#1

Test number 14 on line 92:

![image](\lab-report-5-pics/test1.png)

Markdown file :

```markdown
\*not emphasized*
\<br/> not a tag
\[not a link](/foo)
\`not code`
1\. not a list
\* not a list
\# not a heading
\[foo]: /url "not a reference"
\&ouml; not a character entity
```

Expected output:```[]```

```MarkdownParseTest.java``` implemented test:

My implementation output: ```[]```

Provided implementation output: ```[/foo]```

Looking at the excpexted output, the provided implementation is incorrect in this test case. After looking at the provided implementation, the bug is that it does not count for the cases of `\` escape. This can be fixed by adding a function to check if there is an escape before links, similar to this one:

```java
    public static int indexOfUnescaped(String str, String search, int startIndex) {
        int currIndex = startIndex - 1;
        int backslashCount;
        do {
            currIndex = str.indexOf(search, currIndex + 1);
            backslashCount = 0;
            int index = currIndex - 1;
            while (index >= 0 && str.charAt(index) == '\\') {
                index--;
                backslashCount++;
            }
        } while (backslashCount % 2 == 1);
        return currIndex;
    }
```

### Test \#2

Test number 580 on line 1070:

![image](\lab-report-5-pics/test2.png)

Markdown file :

```markdown
![](/url)
```

Expected output:```[]```

```MarkdownParseTest.java``` implemented test:

My implementation output: ```[]```

Provided implementation output: ```[/url]```

Looking at the excpexted output, the provided implementation is incorrect in this test case. After looking at the provided implementation, the bug is that it does not count for the cases of `images`, which has a very similar md structure to `links`. This can be fixed by adding a function to check if the link found is an image:

```java
// Check that this isn't an image link
    if (markdown.charAt(nextOpenBracket - 1) == '!'){
        String substr = markdown.substring(openParen + 1, closeParen);
        add(substr);
        }
```

---

[Top of the page](#using-a-script-to-run-many-files)

Mohammad Alkhalifah Lab's for [CSE15 lab](https://ucsd-cse15l-w22.github.io/)

[Home page](index.md)
