---
title: "Automate Your Customer Support With AI"
date:  2024-08-4
categories: [AI, Programming, Go High Level]
tags: [AI, Programming, GHL]

---

I automated customer support with AI via SMS.

It saved hours of repetitive work for a business.

I'll show you exactly how I did it, and the best part: not using Zapier. I don’t like those tools with usage caps, regardless of the standard plan—just like serverless functions.

Grab a cup of coffee and get comfortable because I’m going to give you everything.

Here's the tech stack to implement this:

* Go High Level: For automations, organization, and SMS gateway.

* OpenAI's Assistant API: We'll use GPT-4o-mini. Sure, you can use open-source models too, and I’m planning to do a version with Llama 3.1 405B.

* VPS: I used Replit, but you can use any other similar solution on the market as long as you create API endpoints accessible via the Internet.

Here's the documentation for each API and the GitHub repo. You'll need them if you want to tweak the code and customize it for your context.

---

[Assistants API](https://platform.openai.com/docs/assistants/overview)

[GHL API](https://public-api.gohighlevel.com/)

[Github repo](https://github.com/kevinvarc/CustomerSupportAutomation)

[Mozart playlist to feel like a renaissance scientist](https://www.youtube.com/watch?v=Rb0UmrCXxVA&t=694s)


---

Let me give you a brief explanation of why we chose High Level instead of Twilio.

GHL is a great CRM with a very powerful automation feature. We are going to use this feature to send webhooks to our endpoints, where we'll retrieve the assistant's response.

I'm not sure if you can implement the same setup with other CRMs, but as long as you can send, receive, and store the data transferred via webhooks, you shouldn't have any problems.

Let's talk about the GHL setup before diving into the code. And if you're familiar with GHL, no, we are not going to use the built-in GPT feature because it is not powerful enough for our purpose.


You will create three custom fields. A custom field is a value associated with each contact where you can store any information you want.

We need to store the AI's response, the thread ID (I'll explain why this is important later), and the number of messages sent by the bot.

It should look like this:

---

![Alt text](/assets/img/GHL1.png)

---

Ignore the 'Type Of Transaction' field because I don't remember why I created it, and I'm afraid to delete it and break my whole CRM, lol.

It's time to create the automation workflow. When a customer replies to you via SMS, we need to send that message to your code so the AI can respond.

Once the AI responds, we need to send that message again via SMS to the customer. It sounds like a lot, but this is the final workflow.

---

![Alt text](/assets/img/GHL2.png)

---

We need two triggers: one for when the customer replies, and another for when the AI processes the message and sends it back to the customer.

The 'Customer replied' condition, as you can see, sends two webhooks via POST to your endpoint URL. These webhooks contain data in JSON format that we will parse in our backend.

The first webhook sends the Thread_id custom field we created earlier.

When you use the Assistant's API, you need to create a thread where you will append messages to the conversation. In simple terms, let's call it the 'conversation memory.'

After the server responds, it proceeds to send the customer’s message. When the server responds with the assistant's answer, this condition ends. The AI's answer will be stored in the 'GPT Assistant's Response Message' custom field or any other name you have assigned it.

This triggers the second condition, 'AI replies,' which is when the GPT response field is updated. This condition adds one to the 'Message Sent by Bot' custom field and sends an SMS to the customer with the value contained in the 'GPT Response Message' custom field.

And then the cycle continues.

Before jumping into the code, there are some details to consider to make this work. You have to create a tag, which will be used as an 'emergency brake' to stop the AI if it hallucinates.

---

![Alt text](/assets/img/GHL3.png)

---

This is the criteria for triggering the condition when a customer replies.

Secondly, make sure the webhook connection is closed; otherwise, the customer will be stuck in the webhook part, and the message will never be sent.

Now, the moment you've been waiting for: when we challenge the low-code, fancy automation tools that get greedy and aren't agency-friendly.

I used Python, Flask, OpenAI, and GHL APIs.

I won't go deep into the code because I've documented everything you need to know in the GitHub repo. I'll just give you a quick overview.

You'll set up two endpoints: one that will receive the thread_id and initialize the conversation, and the second that will process the message, send it to the Assistant's API, and retrieve the answer.

The reason we use the Assistant API this way is that we can use function calling to reduce randomness and only answer based on database data. For example, if a customer needs to know their invoice history, the assistant will run the function you've defined with the appropriate SQL query to your database and only respond based on that information.

This is how I implemented 'RAG' using AI, reducing 'hallucination.' Sure, you can create a vector database and use embeddings, but I think function calling with SQL queries is a straightforward solution with great results. Don't worry, you don't need to be an AI developer—though it helps.

At this stage, you are only limited by the functionalities you want to add to your bot. Do you want it to set up an appointment, answer FAQs, or perform complex workflows? You can do it. Imagination is the limit.

Finally, the reason we are using the GHL API is to interact directly with the CRM using code. If you want to implement this with your CRM, just read the documentation and modify the code in the repo. I don’t guarantee it will work because I created it with GHL in mind.

I hope I’ve given you a clear path to implementing these powerful tools in your business and clarified that you don’t need to spend hundreds of dollars to leverage AI.

Your expenses with this setup will depend on the number of API requests to OpenAI, the number of SMS messages sent and received, and the platform used to host your backend. You don’t need to rely on third-party apps to enhance your systems.

Check the repo, modify your code, host it, create the automation in your CRM, and automate your customer support. If you face any challenges, just shoot me a DM.
