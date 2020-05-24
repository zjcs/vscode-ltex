---
title: "Advanced Features"
permalink: "/docs/advanced-features.html"
sidebar: "sidebar"
---

## Magic Comments

Magic comments are special comments which contents are interpreted by LTeX and which can be used to change some LTeX settings in the middle of the document. The comments have the following form:

- LaTeX: `% LTeX: <SETTINGS>` on its own line (can be preceded by whitespace)
- Markdown: `# LTeX: <SETTINGS>` at the end of a line (can be preceded by anything). Markdown doesn't really support comments, [but there are some possibilities to workaround this.](https://stackoverflow.com/a/32190021) The recommended way is as follows:

  ```Markdown
  (empty line)
  [comment]: # LTeX: <settings>
  (empty line)
  ```

Magic comments are case-insensitive (except for the setting values), and the space before and after `LTeX:` can be any amount of whitespace (or can be omitted altogether).

`<SETTINGS>` is a whitespace-separated list of `KEY=VALUE` pairs. Neither `KEY` nor `VALUE` are enclosed in quotation marks. Currently, only the following setting is supported:

- `language`: Changes the value of [`ltex.language`](settings.html#ltexlanguage) for the rest of the document.

  *Example:* `language=de-DE`

## LanguageTool HTTP Servers

Although LTeX ships with its own version of LanguageTool (LT), it's possible to run LT independently of LTeX and have LTeX communicate with [LT via HTTP](http://wiki.languagetool.org/http-server). There are multiple scenarios in which this might be useful:

- You want to run your own LT HTTP server, usually on `localhost`. This might be because the version of LT that comes with LTeX does not work, because you want to use an older or newer version of LT than LTeX's LT, or because you use other programs that use LT as well and you only want one instance of LT running.
- You want to use [LanguageTool Plus](https://languagetoolplus.com/), the paid premium version of LT.

To connect to an LT HTTP server, set the setting [`ltex.ltex-ls.languageToolHttpServerUri`](settings.html#ltexltexlslanguagetoolhttpserveruri) to the root URI of the server, for instance, `http://localhost:8081/`. Note that in this mode, LT<sub>E</sub>X will still depend on ltex-ls and Java, as the interface for communicating with LT over HTTP is in ltex-ls.

## Ignoring False Positives with Regular Expressions

It's possible to use [`ltex.ignoreRuleInSentence`](settings.html#ltexignoreruleinsentence) to make LTeX ignore false positives based on regular expressions.

The recommended way of using [`ltex.ignoreRuleInSentence`](settings.html#ltexignoreruleinsentence) is via the `Ignore in this sentence` quick fix. This will add a pair consisting of the ID of the LanguageTool rule and the “sentence” to [`ltex.ignoreRuleInSentence`](settings.html#ltexignoreruleinsentence). LanguageTool internally splits every document into “sentences”, based on language-dependent rules (usually punctuation).

If you want to ignore sentences based on a general pattern, you can add rule/sentence pairs yourself to [`ltex.ignoreRuleInSentence`](settings.html#ltexignoreruleinsentence). The sentence in the pair is a [Java-compatible regular expression](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html).

Note that the checking whether a match returned by LanguageTool is a false positive according to [`ltex.ignoreRuleInSentence`](settings.html#ltexignoreruleinsentence) happens after the document has been split into sentences. Therefore, it's not possible to have regular expressions that span multiple sentences.