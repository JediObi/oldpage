## Topic List
/article/topic_list.json

You can add or modify topics here.

If you wanna add a new topic, you have to create a new directory and it's name should be same to the topic. Then you can modify introduction and specify icon for the new topic.

## Artice List

For example, if you wanna add a new article "This is Shell.md" under topic Shell. 

First, modify /article/articles_issues_map.json. Append a new item with ```article_name``` and ```issue_url``` in the maps under the topic Shell.
The ```article_name``` will lead the script find the article and render it.
The ```issue_url``` will lead the script find the url of a github issue url and render the content in as a comment list which means you have to create a new issue if you wanna the new article will contain a comment system.

## Ads Config
