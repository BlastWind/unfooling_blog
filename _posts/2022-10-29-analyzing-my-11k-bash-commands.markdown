---
layout: post
title: Analyzing my 11k bash commands
date: '2022-10-29 15:48:59'
tags:
- bit
---

`history | wc` informs me that I've typed exactly **11005** bash commands for the 278 days I've used Ubuntu (`ls -lt /var/log/installer`). However, I only expanded my `history` storage about 3 months ago, so I've probably lost out on 60% of the commands I've ever typed.

Still, I'm intrigued to investigate what commands have I been writing, so I set out to write some scripts to find out. I hypothesize the front runners to be &nbsp;`cd`, `ls`, and `docker`.

### Top 10 commands executed 

I wrote the following script:

    import os
    import sys
    from collections import Counter
    from itertools import dropwhile
    
    if __name__ == " __main__":
        history_path = os.path.join("", os.path.expanduser("~"), ".bash_history")
        raw_history = open(history_path, 'r').read().splitlines()
    
        def nonEmpty(str: str) -> bool:
            return len(str) > 0
    
        def isSegmentEnvVar(segment: str):
            return '=' in segment
    
        def isSegmentDate(segment: str):
            return segment[0].isnumeric()
    
        # remove blank lines and dates and env varibales
        history = list(map(lambda row: ' '.join(
            dropwhile(lambda segment: isSegmentDate(segment) or isSegmentEnvVar(segment), row.split())), raw_history
            ))
        history = list(filter(nonEmpty, history))
        
        def top_n_command_name(n: int):
            command_names = map(lambda row: row.split(" ")[0], history)
            return Counter(command_names).most_common(n)
            
        print(top_n_command_name(10))

Results:

<!--kg-card-begin: markdown-->

| Command | Times Executed |
| --- | --- |
| cd | 1522 |
| ls | 1446 |
| npm | 725 |
| sudo | 684 |
| python3 | 588 |
| git | 513 |
| cb-dev-kit | 449 |
| cb-cli | 444 |
| code | 396 |
| node | 296 |

<!--kg-card-end: markdown-->

`isSegmentEnvVar` serves to remove the leading environment variables (e.g, `ENV1=hello python3 driver.py`).

`isSegmentDate` serves to remove the [leading date](https://www.cherryservers.com/blog/a-complete-guide-to-linux-bash-history#add-date-and-timestamps-to-bash-history-output) information that might appear in a standard history entry.

Looks like I use a lot of node and python. Interesting, I would've thought that I've used more `gcc` than the other two.

### 10 most frequent commands with 2 keywords

        def top_n_k_keyword(n: int, k: int):
            keywords_list = [' '.join(row.split()[:k])
                              for row in history if len(row.split()) >= k]
            return Counter(keywords_list).most_common(n)
            
        print(top_n_k_keyword(10, 2))

<!--kg-card-begin: markdown-->

| Command | Times Executed |
| --- | --- |
| cd .. | 324 |
| code . | 142 |
| npm start | 74 |
| cd packages/server/ | 71 |
| ls -R | 44 |
| cb-cli init | 43 |
| cd ../.. | 42 |
| stack build | 39 |
| cb-dev-kit generate | 38 |
| git push | 37 |

<!--kg-card-end: markdown-->
### My full script with 3 more options
<figure class="kg-card kg-bookmark-card"><a class="kg-bookmark-container" href="https://github.com/BlastWind/unfooling-blog-snippets/tree/main/bash-history-analysis"><div class="kg-bookmark-content">
<div class="kg-bookmark-title">unfooling-blog-snippets/bash-history-analysis at main · BlastWind/unfooling-blog-snippets</div>
<div class="kg-bookmark-description">Code snippets for my blog posts at https://unfooling.com - unfooling-blog-snippets/bash-history-analysis at main · BlastWind/unfooling-blog-snippets</div>
<div class="kg-bookmark-metadata">
<img class="kg-bookmark-icon" src="https://github.com/fluidicon.png" alt=""><span class="kg-bookmark-author">GitHub</span><span class="kg-bookmark-publisher">BlastWind</span>
</div>
</div>
<div class="kg-bookmark-thumbnail"><img src="https://opengraph.githubassets.com/f0d78b96beb3a3e50f05bc80f451f7b2f700b49f4af5f42783f69d0b03cb6c57/BlastWind/unfooling-blog-snippets" alt=""></div></a></figure>

I designed 3 more options:

- Shortest N commands
- Longest N commands
- N most frequent full commands

Run the script I linked above with no arguments to get some default output. Join my [discord](https://discord.com/invite/RXDrfY3Svu) and let me know your results!

