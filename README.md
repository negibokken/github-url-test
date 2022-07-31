# GitHub permanlink test

This repository is for checking the rule of generating GitHub permalink.

For example the permalink for 

```text
github-url-test/@foo/[bar]/a !"#$%&'()*+,-.:;<=>?@[\\]^`{|}~.txt
```

is 

```text
/@foo/%5Bbar%5D/a%20!%22%23$%25&'()\*+,-.:;%3C=%3E%3F@%5B%5C%5D%5E%60%7B%7C%7D~.txt
```

## The basic

The GitHub URL consists of like following parts `https://github.com/${owner}/${repository}/blob/${commit_hash}/${path_to_file}?query=parameter#fragment`.

The `${commit_hash}` is simple, but `${owner}`, ${repository}, and ${path_to_file} can contain symbols like ` !"#$%&'()\*+,-.:;<=>?@[\\]^\`\{|\}~`.

Then the permalink has encoded strings. This repository is for the research.

## The Rule for generating permalink

The encode rule is summarized in the below table.

| Symbol | GitHub permalink | encodeURI | encodeURIComponent |
| :-: | :-: | :-: | :-: |
| SP | %20 | %20 | %20 |
| ! | ! | ! | ! |
| " | %22 | %22 | %22 |
| # | %23 | # | %23 |
| $ | $ | $ | %24 |
| % | %25 | %25 | %25 |
| & | & | & | %26 |
| ' | ' | ' | ' |
| ( | ( | ( | ( |
| ) | ) | ) | ) |
| * | * | * | * |
| + | + | + | %2B |
| , | , | , | %2C |
| - | - | - | - |
| . | . | . | . |
| / | / | / | / |
| : | : | : | %3A |
| ; | ; | ; | %3B |
| < | %3C | %3C | %3C |
| = | = | = | %3D |
| > | %3E | %3E | %3E |
| ? | %3F | ? | %3F |
| @ | @ | @ | %40 |
| [ | %5B | %5B | %5B |
| \ | %5C | %5C | %5C |
| ] | %5D | %5D | %5D |
| ^ | %5E | %5E | %5E |
| ` | %60 | %60 | %60 |
| { | %7B | %7B | %7B |
| \| | %7C | %7C | %7C |
| } | %7D | %7D | %7D |
| ~ | ~ | ~ | ~ |

The result of permalink is almost same with the result of [`encodeURI`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) except for `#` and `?`.

## Conclusion

To generate the permalink for a file that includes symbols, follow the below steps.

1. Split the url into from scheme to path as BASE, and query and fragment part as ADDITIONAL
    * i.e. `https://example.com/@foo/[bar]/baz#?!&$.txt` and `?query=test#some-fragment`
2. Run `encodeURI` to BASE and replace `#` into `%23` and `?` into `%3F`
3. Concatenate BASE and ADDITIONAL
    * `https://example.com/@foo/%5Bbar%5D/baz%23?!&$.txt?query=test#some-fragment`

