1/13/2023

I was sick with COVID this week. Do not recommend!

___
## Nested HTML and Markdown rendering + GPT-3
I wonder how Markdown parser/compilers work under the hood.
___
Let's create a problem for the sake of making something interesting. I have been playing around trying to push Markdown formatting to its limits. I feel limited with the options native to Markdown, but a limitted set HTML tags can be used to offer a bit more functionality. However, I find myself wanting to use the HTML features in addition to Markdown features, for example the details tag works fine, until we want to put inside of the details a Markdown header.

<details><summary>Toggle me!</summary>Peek a boo!</details>

```markdown
<details><summary>Toggle me!</summary>Peek a boo!</details>
```

<details><summary>#### Markdown is Cool</summary>![[Pasted image 20230113021509.png</details>

```markdown
<details><summary>#### Markdown is Cool</summary>![[Pasted image 20230113021509.png</details>
```

My instincts tell me this is just a nightmare case for a compiler to consider. For the same reason, consider the task of compiling this LaTeX snippet in Markdown. This works fine in LaTeX but not in Markdown, of course, because Markdown $\ldots$ , I'm not sure. I know Markdown enters math mode, and definitely not a normal Tex mode enviroment, so we can't go into a second layer of math mode, because we never left the first one.

```markdown
$$
Section Text
$$x ^ n + y ^ n =  z ^ n$$
Section Text 2
$$
```

(This doesn't work)
![[vscode_markdown.png]]

More practically, we don't enter a second layer of math mode because the parser is only looking for a start and stop, which actually puts `Section Text` inside of math mode, and the equation we wanted to write on the outside. 
```markdown
```$$
Section Text
$$
```

But how does Markdown render the TeX equations at all? I doubt it uses a full TeX compiler because it just nows how to do math equations. Is it connected to some smaller feature set of TeX or is it all interal to Markdown, just mimicing with an extended set of formatting rules? My first impression is that this functionality is not entirely contained in the Markdown compiler, but connecting to TeX would be too complicated? I have no idea, but I think this is the Markdown parser Github uses to look into more: https://github.github.com/gfm/. Heres an alternative: https://kramdown.gettalong.org/

Revisting HTML and Markdown, if I wanted to accomplish this, I think I would need some command to elevate the enviroment back to Markdown once inside HTML mode. Turns out a [solution](https://stackoverflow.com/questions/67114931/how-to-render-markdown-language-in-html-blocks) for this is really simple, just add more blank lines! However, this only works on Github not on Obsidian!!

```xml
<h4>

$(\Omega, \mathcal{F}, \mathbb{P})$

</h4>
```
<h4>

$(\Omega, \mathcal{F}, \mathbb{P})$

</h4>
Another example:

<details>
<summary>

## Markdown is Cool

</summary>

![[wow.png]]

</details>

```markdown
<details>
<summary>

#### Markdown is Cool

</summary>

![[Pasted image 20230113021509.png]]

</details>
```

I can't believe how silly of a problem that was to solve. This makes me wonder how the parser is working. Either inside of HTML mode, the parser sees the spaces and exits to go back to Markdown mode, or inside of HTML mode, the parser sees the spaces and elevates to a nested Markdown mode. I feel like this is overly complicated, but if we simply exit HTML mode, there are other complications related to stitching together the before and after nested Markdown section. It may be the case that the latter is not actually that difficult to implement.

I tried debugging this problem using the help of GPT-3. I gave it intentionally confusing and complex prompts for the sake of experimentation, and unsurprisingly it did not come up with a good answer.

___
## AI Generated Nonsense
Using GPT-3, temperature 0.8
___

<div>
Here is my markdown text for a Github README.md file. I want the prompt part and response part to be separated using an HTML details and summary tags. However, inside of this HTML section, I need to write text formatted for Markdown. How can I fix this problem, in other words how can I  render Markdown inside of HTML inside of Markdown? The example I am providing I have already fed through this exact same prompt once. But now I have wrapped it in another layer of html using the ordered list.

Example:
<ol>
<li></li>
Give me an list of a sentences that have a mispelling. The mispelling is into a different but spelled correctly word. Ths error keeps the word grammatically correct, but is logically incorrect decuded by context.

Rewrite this prompt to be more understandable by GPT-3.
Response:

  Please provide a list of sentences that contain a spelling mistake, in which the wrong word is spelled correctly but does not make sense in the context of the sentence.

Now reply to the prompt in your previous response.

  Response:
1. She always made shore to arrive on time. (Should be 'sure') 
2. He had a habit of packing his bags the nite before. (Should be 'night') 
3. She had a nice collection of sea shells that she had found on the bay. (Should be 'beach') 
4. He was told to go to the principal's office and wait their. (Should be 'there') 
5. The bees buzzed around the flowers in the garden. (Should be 'gardens') 
6. He had to have his car inspected by the garage. (Should be 'garages') 
7. She was always polite and said her pleases and thank yous. (Should be 'pleases and thank you's') 
8. The teacher asked to see the students paper. (Should be 'student's') 
9. She drove her car down the highway at a high speed. (Should be 'highway's') 
10. The children played in the park all day. (Should be 'park's')


___

<li></li>

Here is my markdown text for a Github README.md file. I want the prompt part and response part to be separated using an HTML details and summary tags. However, inside of this HTML section, I need to write text formatted for Markdown. How can I fix this problem, in other words how can I  render Markdown inside of HTML inside of Markdown?

Example:

<details open><summary><div>
Reply to this prompt with all of the text of both this prompt (including this sentence) and your response, including 1 iteration of recursion.  However for the prompt part of your answer, reverse the order of the sentences, with minor instructions as needed so the instructions still make sense. Do your best. You can add color to a README by using placeholder images to delinate a change in speaker, I'll be neon blue and you be pastel green: 
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `Me:`
- ![#c5f015](https://placehold.co/15x15/c5f015/c5f015.png) `You:`

Response: <\div></summary>
<div>
- ![#c5f015](https://placehold.co/15x15/c5f015/c5f015.png) `You:` Reply to this prompt with all of the text of both this prompt (including this sentence) and your response, including 1 iteration of recursion.  However for the prompt part of your answer, reverse the order of the sentences, with minor instructions as needed so the instructions still make sense. Do your best.
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `Me:` You can add color to a README by using placeholder images to delinate a change in speaker, I'll be neon blue and you be pastel green: 
</div>
</details>

To render Markdown inside of HTML inside of Markdown, you need to wrap the HTML tags in triple backticks (```). For example, the HTML code in the example above should be wrapped in triple backticks, so it looks like this:

```
<details open><summary><div>
Reply to this prompt with all of the text of both this prompt (including this sentence) and your response, including 1 iteration of recursion.  However for the prompt part of your answer, reverse the order of the sentences, with minor instructions as needed so the instructions still make sense. Do your best. You can add color to a README by using placeholder images to delinate a change in speaker, I'll be neon blue and you be pastel green: 
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `Me:`
- ![#c5f015](https://placehold.co/15x15/c5f015/c5f015.png) `You:`

Response: <\div></summary>
<div>
- ![#c5f015](https://placehold.co/15x15/c5f015/c5f015.png) `You:` Reply to this prompt with all of the text of both this prompt (including this sentence) and your response, including 1 iteration of recursion.  However for the prompt part of your answer, reverse the order of the sentences, with minor instructions as needed so the instructions still make sense. Do your best.
- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `Me:` You can add color to a README by using placeholder images to delinate a change in speaker, I'll be neon blue and you be pastel green: 
</div>
</details>
```
</ol>

Reponse:

Once this code is wrapped in triple backticks, it can be rendered correctly as Markdown inside of HTML inside of Markdown.

That is not quite the effect I wanted so I am going to ignore that. Out of curiousity, how would you have responded differently if I had not asked for you to do your best? Moreover, tell me about what ideas did you considered after being specifically asked to do your best writing a response? Do you think it had the intended effect? Reply to all 3 questions using an ordered list HTML tag.

Response:
<ol>
<li>If I had not been asked to do my best in writing a response, I would have likely been less detailed in my explanation of the solution. I likely would have just mentioned that wrapping the HTML tags in triple backticks would solve the issue, without providing an example.</li>

<li>The ideas I considered after being asked to do my best with the response were to try and find the clearest and most concise way of explaining the solution, while still being thorough enough that the reader can understand. I also wanted to make sure the example I provided was easy to understand and follow.</li>

<li>I think the intended effect has been achieved, as the reader should now be able to understand how to render Markdown inside of HTML inside of Markdown.</li>
</ol>
</div>

___

## Auto Github Commit Message Details

Create a vscode extension which parses changed file comments to populate a commit's description. This is a neat feature built-in to Github I like to use for fun, https://github.blog/2013-01-22-closing-issues-via-commit-messages/.

I am very confident tools like this already exist, but I am going to hold off on looking any up because I want to try to tackle this problem independently. 
___
I was looking around at Github and found this code, https://github.com/microsoft/azuredatastudio/pull/16988/files while looking for ways to format text as markdown, inside of HTML tags, in a markdown file [Nested Markdown](### Nested Markdown )

Things I found interesting in this commit:

1. The file used to test the new formatting rules, `notebookMarkdown.test.ts`. Writing tests for code assignments was actually not that tedious. It felt like a natural part of the process to me, meaning I still get to think about writing interesting code while writing tests. My only experience writing unit tests is in Java. I think I could maybe benefit from testing certain parts of my school-related code, but if I am going to spend time with code maintaince, I think it would be better spend just writing better documentation rather than unit tests. For the sake of learning, I think I would like to do a little bit of unit testing in Python in the future; additionally, I should learn testing in Typescript as part of my full stack journey (which is what the originally noted file is). 

2. I found inside of the code this block a part of the comment that matches the description in the pull request.
```javascript
    // {{SQL CARBON EDIT}} Porting marked.js fix for non-empty lines followed by elements to not render properly
    // PR where this fix ocurred here: https://github.com/markedjs/marked/pull/2052/files?file-filters%5B%5D=.js, shipped in markedjs 2.0.4
    + '|</?(tag)(?: +|\\n|/?>)[\\s\\S]*?(?:(?:\\n *)+\\n|$)' // (6)
		+ '|<(?!script|pre|style)([a-z][\\w-]*)(?:attribute)*? */?>(?=[ \\t]*(?:\\n|$))[\\s\\S]*?(?:(?:\\n *)+\\n|$)' // (7) open tag
		+ '|</(?!script|pre|style)[a-z][\\w-]*\\s*>(?=[ \\t]*(?:\\n|$))[\\s\\S]*?(?:(?:\\n *)+\\n|$)' // (7) closing tag
    // {{SQL CARBON EDIT}} end fixes
```

It is unclear to me what the `{{SQL CARBON EDIT}}` part of the comment does. I don't think it is used to automate putting the rest of the comment into the pull request, but I could be wrong.
![[example_pr.png]]

Here is what a commit with a description looks like:
![[example_commit.png]]

Now imagine in my code, limitting ourselves to just Python code for starters, I put a comment like this
```python
def calculate_score(score, points): 
""" Calculates total score given a score and a number of points Args: score (int): Current score points (int): Points to be added Returns: int: Total score """ 
total_score = score + points 
# Fixed issue #24 by ensuring that total_score is returned instead of score.
return total_score 
```

We build a tool to then automatically write this commit description:
![[example_description.png]]

Do we want to close the issue from a PR comment, which closes the issue when the PR is merged or close the issue from the commit message, which closes the issue when the commit is pushed. What I don't know is 
1. If closing from a commit message, does it close on if pushed to the main branch?
2. If closing from a commit message, does the keyword (e.g. "closes #8"), have to be in the title or can it also be in the description?
3. If a fork contains multiple commits with these messages, are the issues closed when the PR is accepted and merged into main?

My guess to all these is probably, but I would need to test for myself. Rather than spamming commits, I should practice using a fork and squashing commits on merge.

___

## Auto Update Markdown Files to Portfolio Site Blog Section
Portfolio Typescript + Github Actions to build a blog webpage using this repo of markdown posts.
___
The repo that this file and others will live in will be here, https://github.com/evmckinney9/chatgpt-ouroboros. I know that compiling markdown posts to build is a popular option when using Github Pages, for example with Jekyll[^2], but I'd rather implement something myself from scratch using Typescript. I'll say that I currently have no idea how to go about doing this.

1. I'm going to use Obsidian as my Markdown editor. The reason I prefer this over vscode is that I can drop images into the editor to automatically update it. The image should be collected in a directory, so that the in-line references to the images work in both on Obsidian and Github. I just need to make sure the file paths are sensically relative. In addition to images, I should make sure that references to other sections (ToC) works.
2. The title of each blog post is each file name, ordered by date. The date can either be when the commit was made on this file (we can track both the original and most recent dates). Alternatively, the date can be given by the first line in the file (this file as an example) if it exists. 
3. We should use keywords in the commit messages to indicate if the post should become public or remain as a private draft. I'm sure we can borrow ideas from [the previous idea](#auto-github-commit-message-details) and also use it as an opportunity to learn about [Github Actions](https://github.com/features/actions).
4. More things to find as I come across them.

___
#### Footnotes

Footnotes are annoying me in Obsidian. Look into using [this community plug-in](https://github.com/akaalias/obsidian-footnotes). 

1. Can I put an image in a footnote [^2]?
2. Can we dynamically update references when new footnotes are added?

Also, linking to other headers is annoying too:
3. To work properly on Github, requires a single `#` and lower-case. 
```markdown
[the previous idea](#auto-github-commit-message-details)
```
4. There is no auto-complete (spoiled!) preview. However, there is a preview if referenceing to an external file with double brackets: ![[Pasted image 20230113020122.png]]
 I should look for a better way to do these things (hotkeys or make our own plug-in?) which builds auto-complete + functional formatting.

#### Footnotes pt 2.
[^1: https://qiskit.org/documentation/tutorials/circuits_advanced/04_transpiler_passes_and_passmanager.html]

[^2: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/setting-a-markdown-processor-for-your-github-pages-site-using-jekyll]

[^3: ![[regex.png]]]