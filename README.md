# n8n AI Workflows

Five small AI automations built in n8n: a RAG chatbot, an email support agent, a LinkedIn post writer, an invoice processor, and an API practice workflow.

## Why I built these
I wanted to learn how AI agents, RAG and tools fit together by building them. I followed a tutorial course for the structure, then adapted the workflows: my own vector database (Qdrant), my own documents, and different AI models.

## 1. RAG chatbot
Ask questions about your own documents and get answers from them.

<img width="1670" height="756" alt="rag" src="https://github.com/user-attachments/assets/117dbbf3-9ed5-44db-9df4-7f9a4f4257bb" />

1. A new file in a Google Drive folder triggers the workflow.
2. The file is downloaded and split into chunks.
3. An OpenAI embedding model turns each chunk into numbers.
4. The chunks are stored in a Qdrant vector database.
5. When you chat, an AI agent (Gemini) searches Qdrant for the closest chunks and answers from them.

## 2. Customer support email agent
Reads incoming emails and replies to customer questions using a company FAQ.

<img width="1671" height="756" alt="customer_support" src="https://github.com/user-attachments/assets/cf0de84e-9160-4cfa-8c6d-060400c2a8bd" />

1. A Gmail trigger checks for new emails every minute.
2. A text classifier decides: **Customer Support** or **Other**.
3. Other emails are ignored.
4. For support emails, an AI agent searches the policy and FAQ chunks in Qdrant.
5. The agent writes a friendly reply, and a Gmail node sends it.

## 3. LinkedIn post writer
Turns a list of topics in a Google Sheet into LinkedIn posts.

<img width="1670" height="766" alt="linkedin_post" src="https://github.com/user-attachments/assets/588c3f53-8c9e-447b-baa3-527151546055" />

1. A Google Sheets node reads the topic rows.
2. An AI agent (OpenAI) researches the topic with Tavily web search.
3. The agent writes the post.
4. The post is saved back in the sheet's **Content** column.

## 4. Invoice processor
Reads invoice PDFs, saves their details in a spreadsheet, and tells the billing team.

![Invoice workflow](invoice.png)

1. A new PDF in a Google Drive folder triggers the workflow.
2. The file is downloaded, and its text is pulled out with the **Extract from File** node.
3. An **Information Extractor** (Gemini) finds the fields: invoice number, client name, email, address, phone, total amount, invoice date and due date.
4. The fields are added as a new row in an **Invoice DB** Google Sheet.
5. An OpenAI **Message a model** node writes a short notification email.
6. A Gmail node sends the email to the internal billing team.

## 5. API practice: HTTP requests
Practice calling APIs directly with the **HTTP Request** node, instead of using ready-made nodes.

![API workflow](API.png)

1. **Form trigger:** a form takes a topic.
2. An AI agent (Anthropic) researches it with Tavily and returns two separate fields, the LinkedIn post and an image prompt, using a **Structured Output Parser**.
3. An **HTTP Request** node sends the image prompt to the Gemini image API (a POST request with headers and a JSON body).
4. A **Convert to File** node turns the base64 text in the reply into an image file.
5. A Gmail node sends the post with the image attached.

A second, small flow on the canvas makes a plain POST request to the Perplexity API, as a first exercise.

The image step needs an API key that has image-generation access, which free keys may not include.

## Stack
- **n8n**: workflow automation
- **Qdrant**: vector database
- **OpenAI**, **Gemini**, **Anthropic**: chat, embedding and image models
- **Tavily**: web search
- **Google Drive, Sheets and Gmail**: inputs and outputs

## Using the workflows
1. In n8n, choose **Import from file** and pick a `.json` file.
2. Connect your own credentials. API keys are never stored in the workflow files, so add yours as n8n credentials.
3. In `LinkedIn Post.json`, replace `YOUR_SHEET_ID` with your own Google Sheet ID.
4. In `Invoice.json`, choose your own Drive folder and Google Sheet.
5. Use the same embedding model for inserting and searching, and make sure its size matches your Qdrant collection.
