🪄 Storycraft Workshop

An AI-powered personalized children's content generator. This project is a Telegram bot built on a multi-agent n8n workflow that creates unique, illustrated children's stories in real-time.
🌟 Key Features

    Natural Language Request Parsing: The bot analyzes free-form user text (e.g., "a story about a clever fox and a man") using Google Gemini to extract characters, themes, and the desired style for the story.

    Personalized Story Generation: Creates unique tales based on the extracted data, including the child's name, age, interests, and a specific problem to address.

    Illustrations for Every Story: Generates a unique cover image for each tale, visually capturing its essence.

    Persistent Story Library: Every created story is automatically saved to a Supabase database, building a personal library for each user.

    Keyword Search: Allows users to easily find previously created stories with a simple query (e.g., "find the story about the fox").

⚙️ How It Works: The New Workflow

The entire logic is orchestrated within a single n8n workflow that connects multiple AI services.

    User Interaction (Telegram): The workflow starts when a user sends a message to the bot. A Telegram Trigger node captures this interaction.

    Intent Recognition and Data Parsing:

        An If node first checks if the message contains a search command (e.g., "find," "repeat").

        If it's a request for a new story, the message is passed to an LLM Chain that uses the Google Gemini model (gemini-2.5-flash-lite) to analyze the text and extract structured data (child's name, age, gender, theme, characters, style, etc.) in JSON format.

    User Creation and Updates (Supabase):

        The system searches for the user in the Supabase database by their user_id.

        If the user exists, their data (e.g., new preferences) is updated. If not, a new record is created.

    Story and Illustration Prompt Creation:

        The structured data is sent to an AI Agent, which uses Google Gemini as the "storyteller" to write a unique narrative.

        In parallel, the finished story text is sent to the Perplexity API to create a short, concise prompt for image generation.

    Image Generation:

        The generated prompt is sent via an HTTP Request node to an image generation API (e.g., Hugging Face with the Stable Diffusion model).

    Saving and Delivery:

        The final story text and the URL of the generated illustration are saved to the tales_content table in Supabase.

        The bot sends a message to the user in Telegram with the complete, illustrated story.

🛠️ Technology Stack

    Orchestration: n8n

    User Interface: Telegram Bot API

    Text Generation & Parsing: Google Gemini API (gemini-2.5-flash-lite)

    Image Prompt Generation: Perplexity API

    Image Generation: Hugging Face API (Stable Diffusion) or similar

    Database & Storage: Supabase

🚀 Getting Started

To run this project, you will need a self-hosted or cloud instance of n8n and the necessary API keys for all integrated services.

    Clone the Repository
    To get the workflow.json file, clone this project to your local machine.

bash
git clone https://github.com/YOUR-USERNAME/Tales_workshop.git

Import the Workflow
In your n8n instance, import the workflow.json file to create a copy of the entire workflow.

Configure Credentials
The workflow requires API credentials. In n8n, navigate to the "Credentials" section and add new credentials for each of the following services:

    Telegram API: Your bot token obtained from @BotFather.

    Google Gemini API: Your API key from Google AI Studio.

    Hugging Face API: An API token from your Hugging Face account settings.

    Perplexity API: Your API key from Perplexity's developer platform.

    Supabase API: Your project URL and anon key from your Supabase project settings.

Link Credentials to Nodes
Open the imported workflow in the n8n editor. For each node that requires authentication (e.g., the Google Gemini, Supabase nodes), select the credential you just created from the dropdown menu.

Activate the Workflow
Save and activate the workflow. Your Storycraft Workshop is now ready to work its magic in Telegram
