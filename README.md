# n8n AI Workflows

Three small AI automations built in n8n: a RAG chatbot, an email support agent, and a LinkedIn post writer.

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

## Stack
- **n8n**: workflow automation
- **Qdrant**: vector database
- **OpenAI**, **Gemini**, **Anthropic**: chat and embedding models
- **Tavily**: web search
- **Google Drive, Sheets and Gmail**: inputs and outputs

## Using the workflows
1. In n8n, choose **Import from file** and pick a `.json` file.
2. Connect your own credentials. None are included in these files.
3. In `LinkedIn Post.json`, replace `YOUR_SHEET_ID` with your own Google Sheet ID.
4. Use the same embedding model for inserting and searching, and make sure its size matches your Qdrant collection.
