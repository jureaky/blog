---
title: "[C] Read String With fgets() and sscanf() instead of scanf()"
date: 2022-05-30T13:39:19+09:00
draft: true
resources:
- name: "featured-image"
  src: "images/featured-img.jpg"
tags: ["C"]
categories: ["Lang"]

---

## 1 Problem with scanf() when reading a string

{{< prism line=true lang="clike">}}
int main(void)
{
    char str[10];

    scanf("%s", str);

    printf("String read: %s\n", str);

}
{{< /prism >}}

Look at the simple code block above. This code just reads a non-whitespaced string from a user and write to `str` character array, which has size 10 bytes.

So, `str` array can only store 9 characters because null character(`\0`) should be appended at the very last position.

What happens if users give more than 9 characters as input?

Don't know. But this is clearly a bad situation. Some systems might crash, some might work anyway.

Even thought it seems working in some systems, this is a buffer overflow, which can cause way worse results than just crashing.


## 2. How to prevent buffer overflow then?

```c
#define BUFFER_LENGTH (10)

int main(void)
{
    char buffer[BUFFER_LENGTH];
    char str[BUFFER_LENGTH];

    while (1) {
        if (fgets(buffer, BUFFER_LENGTH, stdin) == NULL) {
            break;
        }

        if (sscanf(buffer, "%s", str) == 1) {
            printf("%s\n", str);
        }
    }

}
```

To prevent buffer overflow, use `fgets()`. `fgets()` takes in 3 arguments, location to store the data read, size of data to read, from where to read data.

`fgets()` reads maximum `BUFFER_LENGTH - 1` bytes (because of `'\0'`) at once, so this prevents buffer overflow.

Instead, we need a while loop to read all the data over `BUFFER_LENGTH - 1` bytes. Then, we use `sscanf()` to read from the buffer used by `fgets()`.

Because `str` array and `buffer` array sizes are equal, we can guarantee that no buffer overflow will occur.


## 3. Little peculiarities of using fgets() and sscanf() together

Even though `str` array and `buffer` array have the same size, we need to be careful when using those 2 functions together.

This is not about security thing, but if we overlook details, we might not get the results that we wanted.

`fgets()` stores newline character(`\n`) in the string. So, if a user entered "say" on prompt, "say\n\0" is stored on `buffer`.

However, `sscanf()` rules out the newline character, as it is considered as whitespace character, so only "say\0" would be sotred on `str`.

It doesn't even read when the user only pressed enter without any characters. That's why `sscanf()` is enclosed by if-statement. Therefore, newline character is added at `printf()` function.

But when the user puts more than `BUFFER_LENGTH - 1` characters, we would see the printed string is spearated by newline characters every `BUFFER_LENGTH - 1` character.


:point_right: **Thank you for reading my post !** :pray:

:point_right: Please leave a comment if you have any ideas to share ! :notes:
