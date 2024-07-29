---
title: "Function Calling | Steroids for your LLMs"
date:  2024-06-18
categories: [AI]
tags: [ai]

---

Among all the extra capabilities and techniques available for LLMs, function calling is worth understanding and implementing. The primary goal of these techniques is to enhance the capabilities of current models, either by providing specific data and retrieving it with RAG or by enabling your model to run internal functions in your program and execute specific actions.

After reading this article you will have a solid foundation in this capability, enabling you to make your models 10 times more effective and accurate.

What is function calling and how is it different from RAG?

* Examples

* Conclusion

You are probably thinking, "What makes it different from RAG?" What RAG does is process unstructured data and store the text embeddings in a vector database. This approach is great for data that doesn't change often, providing a window-context for the LLM and answers accordingly.

Let's say you have multiple PDFs explaining your refund policy and shipping costs for different states. This data doesn't really change often, making RAG ideal for this task. But what happens when the LLM requires real-time data to answer a question? Here's where function calling is useful.

The goal of any extra capability for LLMs is always the same: to provide a clear prompt with context. What distinguishes one capability from another is the approach. Use function calling if data is constantly changing, and RAG if data does not change often.

Function calling is more specific because it executes code in the form of a function, receiving an expected parameter given by the prompt. This function will return a value that informs the LLM whether it was executed correctly or not.

Everything will make sense with this example:

Let's say we want to enable an LLM to scan a WordPress website and check for any potential vulnerabilities based on its version. The parameter the LLM expects to perform this action is the domain of the website. Once it receives the domain, we can use function calling to determine if the website is running WordPress. If it is, we can extract its version and use WPScan to check for any vulnerabilities in their database.

---

![Alt text](/assets/img/function.png)

---

This is the function the LLM will call. As we can see in the first line, we are formatting the URL parameter because the user is likely to provide input like 'example.com'. The first advantage of function calling is its ability to maintain data format consistency, ensuring that we always receive expected responses.

Then, we call a nested function process_wp_version(url). Here's another advantage of function calling: it allows you to create more robust workflows to achieve your goals. In this case, we invoke this function, which in turn calls another function fetch_website_html(url) that sends a GET request to the URL, returning the HTML. This allows us to scrape the website data to find the content meta tag that usually contains the WordPress version. This extraction is handled by the extract_wp_version(html) function, as you can see. You can start reading the code from the bottom; people often complain when I define a function randomly in the program without any specific order, I know.

---



---

This is what happens behind the scenes when you ask the LLM for this information. The LLM initially has no information about the website, so it needs to receive the right information through the outputs of the functions.

Finally, we can ask the LLM about the version of a WordPress website and check for any vulnerabilities associated with that version. Let's see what version NASA's website is using and if it has any vulnerabilities.

---



---

This is the response of the LLM. With function calling, we can provide instructions on how to find the necessary information to answer the question. In the example above, I used the Assistant API from OpenAI. I plan to create a complete guide on this API in the future because it deserves its own article.

### ***Conclusion:***


This capability opens up a world of unlimited possibilities and is one of the foundations of AI agents. Using function calling in conjunction with RAG will definitely elevate your chatbots, enabling them to autonomously perform complex tasks. If you would like to know how to implement this, DM me on X @kevinvarc, and we can talk!

test 

```python

import requests

conn = requests.get("google.com")

```