### Lab Report 4

# Debuggers

- [Debuggers](#debuggers)
    - [Overview](#overview)
    - [Snippet \#1](#snippet-1)
    - [Snippet \#2](#snippet-2)
    - [Snippet \#3](#snippet-3)

---

### Overview

This lab report will cover and discuss the output of the provided test cases on these two implmentaion of `markdown-parse`:

- [`My MarkdownPrase`](https://github.com/Mkhlf/markdown-parse)
- [`Reviewed MarkdownPrase`](https://github.com/m1ma0314/markdown-parse)

### Snippet \#1

Markdown file:

```markdown
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
```

Expected output:```[`google.com, google.com, ucsd.edu]```

```MarkdownParseTest.java``` implemented test:

```java
@Test
public void testSnippet1() throws IOException {
    String contents = Files.readString(Path.of("./lab8_Snippet_1.md"));
    List<String> expect = List.of("`google.com", "google.com", "ucsd.edu");
    assertEquals(expect, MarkdownParse.getLinks(contents));
}
```

My implementation output: ```[url.com, `google.com, google.com, ucsd.edu]```

My implementation failed in identifying the correct URLs. Here is the ```JUnit``` output:

```shell
1) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:87)
```

Reviewed implementation output: ```[url.com, `google.com, google.com, ucsd.edu]```

Reviewed implementation failed in identifying the correct URLs. Here is the ```JUnit``` output:

```shell
3) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:65)
```

Do you think there is a small (<10 lines) code change that will make your program work for snippet 1 and all related cases that use inline code with backticks? If yes, describe the code change. If not, describe why it would be a more involved change.

Based on the snippet above, only one case does not work correctly: when the open backticks and the close are not both within the brackets my code does not work. To fix this, I think we can check if I am starting a inline code or code block at any point of the text. Then, I should ignore anything within that block of characrters. However, implementing that is quite complicated as you have to acount for backticks within in line code and for block of codes as well, while also account for not closing in line code or code block.

### Snippet \#2

Markdown file:

```markdown
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
```

Expected output:```[a.com, a.com(()), example.com]```

```MarkdownParseTest.java``` implemented test:

```java
@Test
    public void testSnippet2() throws IOException {
        String contents = Files.readString(Path.of("./lab8_Snippet_2.md"));
        List<String> expect = List.of("a.com", "a.com(())", "example.com");
        assertEquals(expect, MarkdownParse.getLinks(contents));
    }

}
```

My implementation output: ```[a.com, example.com]```

My implementation failed in identifying the correct URLs. Here is the ```JUnit``` output:

```shell
2) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:94)
```

Reviewed implementation output: ```[a.com, a.com((, example.com]```

Reviewed implementation failed in identifying the correct URLs. Here is the ```JUnit``` output:

```shell
4) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:72)
```

Do you think there is a small (<10 lines) code change that will make your program work for snippet 2 and all related cases that nest parentheses, brackets, and escaped brackets? If yes, describe the code change. If not, escribe why it would be a more involved change.

I think my code can handle the case of nested brackets and escaped quite well. However, for nested parentheses my code only account for a nested open parenthes with no closed one within a valid pair of parentheses. I believe accounting for a pair of parentheses within a pair of parentheses can be solved within 10 lines, as I only have to check the number of opened and closed parentheses after a valid brackets while keeping the oreder in mind using a stack.

### Snippet \#3

Markdown file:

```markdown
[this title text is really long and takes up more than 
one line

and has some line breaks](
    https://www.twitter.com
)

[this title text is really long and takes up more than 
one line](
    https://ucsd-cse15l-w22.github.io/
)


[this link doesn't have a closing parenthesis](github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



)

And then there's more text
```

Expected output:```[https://ucsd-cse15l-w22.github.io/]```

```MarkdownParseTest.java``` implemented test:

```java
 @Test
    public void testSnippet3() throws IOException {
        String contents = Files.readString(Path.of("./lab8_Snippet_3.md"));
        List<String> expect = List.of("https://ucsd-cse15l-w22.github.io/");
        assertEquals(expect, MarkdownParse.getLinks(contents));
    }

```

My implementation output: ```[]```

My implementation failed in identifying the correct URLs. Here is the ```JUnit``` output:

```shell
3) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://ucsd-cse15l-w22.github.io/]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:101)
```

Reviewed implementation output: ```[]```

Reviewed implementation failed in identifying the correct URLs. Here is the ```JUnit``` output:

```shell
5) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://ucsd-cse15l-w22.github.io/]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:79)
```

Do you think there is a small (<10 lines) code change that will make your program work for snippet 3 and all related cases that have newlines in brackets and parentheses? If yes, describe the code change. If not, describe why it would be a more involved change.

My implementaion checks every line by itself. Moreover, the position of ```"\n"``` can change the way markdown decides if the link is valid or not.  Therfore, I think fixing the issue for a having a link in multiple lines would be quite a long fix.

---

[Top of the page](#debuggers)

Mohammad Alkhalifah Lab's for [CSE15 lab](https://ucsd-cse15l-w22.github.io/)

[Home page](index.md)
