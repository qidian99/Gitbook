# Regular Expressions

## Lookahead and Lookbehind

{% embed url="https://www.regular-expressions.info/refadv.html" %}





| Feature | Syntax | Description | Example | [JGsoft](https://www.regular-expressions.info/jgsoft.html) | [.NET](https://www.regular-expressions.info/dotnet.html) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| [Comment](https://www.regular-expressions.info/freespacing.html#parenscomment) | \(?\#comment\) | Everything between \(?\# and \) is ignored by the regex engine. | a\(?\#foobar\)b matches ab | YES | YES |
| [Branch reset group](https://www.regular-expressions.info/branchreset.html) | \(?\|regex\) | If the regex inside the branch reset group has multiple alternatives with capturing groups, then the capturing group numbers are the same in all the alternatives. | \(x\)\(?\|\(a\)\|\(bc\)\|\(def\)\)\2 matches xaa, xbcbc, or xdefdef with the first group capturing x and the second group capturing a, bc, or def | V2 | no |
| [Atomic group](https://www.regular-expressions.info/atomic.html) | \(?&gt;regex\) | Atomic groups prevent the regex engine from backtracking back into the group after a match has been found for the group. If the remainder of the regex fails, the engine may backtrack over the group if a quantifier or alternation makes it optional. But it will not backtrack into the group to try other permutations of the group. | a\(?&gt;bc\|b\)c matches abcc but not abc | YES | YES |
| [Positive lookahead](https://www.regular-expressions.info/lookaround.html) | \(?=regex\) | Matches at a position where the pattern inside the lookahead can be matched. Matches only the position. It does not consume any characters or expand the match. In a pattern like one\(?=two\)three, both two and three have to match at the position where the match of one ends. | t\(?=s\) matches the second t in streets. | YES | YES |
| [Negative lookahead](https://www.regular-expressions.info/lookaround.html) | \(?!regex\) | Similar to positive lookahead, except that negative lookahead only succeeds if the regex inside the lookahead fails to match. | t\(?!s\) matches the first t in streets. | YES | YES |
| [Positive lookbehind](https://www.regular-expressions.info/lookaround.html#lookbehind) | \(?&lt;=regex\) | Matches at a position if the pattern inside the lookbehind can be matched ending at that position. | \(?&lt;=s\)t matches the first t in streets. | YES | YES |
| [Negative lookbehind](https://www.regular-expressions.info/lookaround.html#lookbehind) | \(?&lt;!regex\) | Matches at a position if the pattern inside the lookbehind cannot be matched ending at that position. | \(?&lt;!s\)t matches the second t in streets. | YES | YES |
| [Lookbehind](https://www.regular-expressions.info/lookaround.html#limitbehind) | \(?&lt;=regex\|longer regex\) | Alternatives inside lookbehind can differ in length. | \(?&lt;=is\|e\)t matches the second and fourth t in twisty streets. | YES | YES |
| [Lookbehind](https://www.regular-expressions.info/lookaround.html#limitbehind) | \(?&lt;=x{n,m}\) | Quantifiers with a finite maximum number of repetitions can be used inside lookbehind. | \(?&lt;=s\w{1,7}\)t matches only the fourth t in twisty streets. | YES | YES |
| [Lookbehind](https://www.regular-expressions.info/lookaround.html#limitbehind) | \(?&lt;=regex\) | The full regular expression syntax can be used inside lookbehind. | \(?&lt;=s\w+\)t matches only the fourth t in twisty streets. | YES | YES |
| [Lookbehind](https://www.regular-expressions.info/lookaround.html#limitbehind) | \(group\)\(?&lt;=\1\) | Backreferences can be used inside lookbehind. Syntax prohibited in lookbehind is also prohibited in the referenced capturing group. | \(\w\).+\(?&lt;=\1\) matches twisty street in twisty streets. | YES | YES |
| [Keep text out of the regex match](https://www.regular-expressions.info/keep.html) | \K | The text matched by the part of the regex to the left of the \K is omitted from the overall regex match. Other than that the regex is matched normally from left to right. Capturing groups to the left of the \K capture as usual. | s\Kt matches only the first t in streets. | V2 | no |
| [Lookaround conditional](https://www.regular-expressions.info/conditional.html) | \(?\(?=regex\)then\|else\) where \(?=regex\) is any valid lookaround and then and else are any valid regexes | If the lookaround succeeds, the “then” part must match for the overall regex to match. If the lookaround fails, the “else” part must match for the overall regex to match. The lookaround is zero-length. The “then” and “else” parts consume their matches like normal regexes. | \(?\(?&lt;=a\)b\|c\) matches the second b and the first c in babxcac | YES | YES |
| [Implicit lookahead conditional](https://www.regular-expressions.info/conditional.html) | \(?\(regex\)then\|else\) where regex, then, and else are any valid regexes and regex is not the name of a capturing group | If “regex” is not the name of a capturing group, then it is interpreted as a lookahead as if you had written \(?\(?=regex\)then\|else\). If the lookahead succeeds, the “then” part must match for the overall regex to match. If the lookahead fails, the “else” part must match for the overall regex to match. The lookaround is zero-length. The “then” and “else” parts consume their matches like normal regexes. | \(?\(\d{2}\)7\|c\) matches the first 7 and the c in 747c | no | YES |
| [Named conditional](https://www.regular-expressions.info/conditional.html) | \(?\(name\)then\|else\) where name is the name of a capturing group and then and else are any valid regexes | If the capturing group with the given name took part in the match attempt thus far, the “then” part must match for the overall regex to match. If the capturing group did not take part in the match thus far, the “else” part must match for the overall regex to match. | \(?&lt;one&gt;a\)?\(?\(one\)b\|c\) matches ab, the first c, and the second c in babxcac | YES | YES |
| [Named conditional](https://www.regular-expressions.info/conditional.html) | \(?\(&lt;name&gt;\)then\|else\) where name is the name of a capturing group and then and else are any valid regexes | If the capturing group with the given name took part in the match attempt thus far, the “then” part must match for the overall regex to match. If the capturing group did not take part in the match thus far, the “else” part must match for the overall regex to match. | \(?&lt;one&gt;a\)?\(?\(&lt;one&gt;\)b\|c\) matches ab, the first c, and the second c in babxcac | V2 | no |
| [Named conditional](https://www.regular-expressions.info/conditional.html) | \(?\('name'\)then\|else\) where name is the name of a capturing group and then and else are any valid regexes | If the capturing group with the given name took part in the match attempt thus far, the “then” part must match for the overall regex to match. If the capturing group did not take part in the match thus far, the “else” part must match for the overall regex to match. | \(?'one'a\)?\(?\('one'\)b\|c\) matches ab, the first c, and the second c in babxcac | V2 | no |
| [Conditional](https://www.regular-expressions.info/conditional.html) | \(?\(1\)then\|else\) where 1 is the number of a capturing group and then and else are any valid regexes | If the referenced capturing group took part in the match attempt thus far, the “then” part must match for the overall regex to match. If the capturing group did not take part in the match thus far, the “else” part must match for the overall regex to match. | \(a\)?\(?\(1\)b\|c\) matches ab, the first c, and the second c in babxcac | YES | YES |
| [Relative conditional](https://www.regular-expressions.info/conditional.html) | \(?\(-1\)then\|else\) where -1 is a negative integer and then and else are any valid regexes | Conditional that tests the capturing group that can be found by counting as many opening parentheses of named or numbered capturing groups as specified by the number from right to left starting immediately before the conditional. If the referenced capturing group took part in the match attempt thus far, the “then” part must match for the overall regex to match. If the capturing group did not take part in the match thus far, the “else” part must match for the overall regex to match. | \(a\)?\(?\(-1\)b\|c\) matches ab, the first c, and the second c in babxcac | V2 | no |
| [Forward conditional](https://www.regular-expressions.info/conditional.html) | \(?\(+1\)then\|else\) where +1 is a positive integer and then and else are any valid regexes | Conditional that tests the capturing group that can be found by counting as many opening parentheses of named or numbered capturing groups as specified by the number from left to right starting at the “then” part of conditional. If the referenced capturing group took part in the match attempt thus far, the “then” part must match for the overall regex to match. If the capturing group did not take part in the match thus far, the “else” part must match for the overall regex to match. | \(\(?\(+1\)b\|c\)\(d\)?\){2} matches cc and cdb in bdbdccxcdcxcdb | V2 | no |
| [Conditional](https://www.regular-expressions.info/conditional.html) | \(?\(+1\)then\|else\) where 1 is the number of a capturing group and then and else are any valid regexes | The + is ignored and the number is taken as an absolute reference to a capturing group. If the referenced capturing group took part in the match attempt thus far, the “then” part must match for the overall regex to match. If the capturing group did not take part in the match thus far, the “else” part must match for the overall regex to match. | \(a\)?\(?\(+1\)b\|c\) matches ab, the first c, and the second c in babxcac | no | no |
| Feature | Syntax | Description | Example | [JGsoft](https://www.regular-expressions.info/jgsoft.html) | [.NET](https://www.regular-expressions.info/dotnet.html) |
