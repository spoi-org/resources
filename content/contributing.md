---
draft: false
title: 'Contributing'
weight: 7
---

We're currently looking for contributors to write editorials for older ZCO and INOI problems to improve the completeness of this resource. However, high-quality editorials for other problems and olympiads are also welcome!

## Standards

We place a **strong** emphasis on quality. Explanations must be written by the contributor and must **not** be generated using AI. AI tools may be used to reword or improve the clarity of contributor-written sentences, but they must not be used to produce the underlying explanations or ideas.

We encourage the use of diagrams and animated GIFs where they improve clarity. AI tools can be used to create these visual aids if necessary.

All contributions should remain consistent with the existing visual style of the repository. In particular, for diagrams:

* Use fonts consistent with those already used throughout the website.
* Do not use rounded corners or `border-radius`.

If you include code in your editorial, ensure that:
* Your code is formatted with clang-format (with the [following](/.clang-format) configuration file)
  * In particular, ensure that you use two spaces for indentation.
* If possible, avoid using defines such as `#define int long long`. They reduce your code's clarity.

## Creating the editorial

To add a ZCO, INOI, or other olympiad problem, first open:

```text
layouts/shortcodes/problem.html
```

If the problem is not already present, add it to the problem list using the following format:

```go-html-template
"[problem-name]" (dict
  "title" "[problem title]"
  "source" "[source, such as CodeChef or Codeforces]"
  "url" "https://link/to/problem"
)
```

---

Create a new Markdown file in the appropriate directory:

```text
content/editorials/zco/
```

or:

```text
content/editorials/inoi/
```

or whatever's appropriate. The filename should correspond to the problem name.

Add the following front matter at the beginning of the file:

```yaml
---
draft: false
title: "[problem title]"
editorial:
  platform: "[olympiad name, such as ZCO or INOI]"
  name: "[problem title]"
---
```

---

Use the following shortcode to insert the problem card:

```go-html-template
{{</* problem "[problem-name]" */>}}
```

---

Inline LaTeX can be written using single dollar signs:

```text
$expression$
```

Display-style LaTeX can be written using double dollar signs:

```text
$$
expression
$$
```

HTML elements may also be used where necessary.

Before submitting a contribution, verify that the editorial renders correctly and that its writing, formatting, and visual elements are consistent with the rest of the repository.

Thank you!