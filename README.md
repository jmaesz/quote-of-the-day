# 🤖 Telegram Bot

A Telegram bot written in Python that delivers inspiring quotes.

## 📝 Description

This Telegram bot delivers inspiring quotes directly to your chat. Start your day with a "quote of the day" or get a random quote whenever you need a bit of motivation.

The bot is built with Python and the `python-telegram-bot` library, making it easy to run and customize.

## ✨ Features

*   **Quote of the Day:** Automatically sends a new quote every day.
*   **Random Quotes:** Users can request a random quote at any time.
*   **Easy to set up:** Simple configuration and deployment.

## 🚀 Installation

1.  **Clone this repository:**
    ```bash
    git clone https://github.com/jmaesz/quote-of-the-day
    ```
2.  **Create a virtual environment:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```
3.  **Install the dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

## 💻 Local Development

To run the bot locally for development, you can use polling.

1.  Set your environment variables. You can use a `.env` file for this. See the **Configuration** section.
2.  Run the bot:
    ```bash
    python bot.py
    ```

## ☁️ Deployment (AWS Lambda)

This bot is designed to be deployed as a serverless webhook on AWS Lambda.

### 1. Create the Deployment Package 📦

1.  Install the dependencies in a `package` directory:
    ```bash
    pip install --target ./package -r requirements.txt
    ```
2.  Create a zip file with the contents of the `package` directory and your Python bot file (`bot.py` or similar).
    ```bash
    cd package
    zip -r ../deployment.zip .
    cd ..
    zip -g deployment.zip bot.py
    ```

### 2. Set up AWS Lambda ⚙️

1.  **Create a Lambda Function:**
    *   Open the [AWS Lambda console](https://console.aws.amazon.com/lambda/).
    *   Click "Create function".
    *   Select "Author from scratch".
    *   **Function name:** Choose a name (e.g., `telegram-quote-bot`).
    *   **Runtime:** Select "Python 3.9" (or a compatible version).
    *   **Architecture:** Choose `x86_64`.
    *   Click "Create function".

2.  **Upload Code:**
    *   In the "Code source" section, click "Upload from".
    *   Select ".zip file".
    *   Upload your `deployment.zip` file.

3.  **Configure Handler:**
    *   In "Runtime settings", set the **Handler** to `bot.handler`. This assumes your Python file is named `bot.py` and your handler function is named `handler`.

4.  **Add Environment Variables:**
    *   Go to the "Configuration" tab and select "Environment variables".
    *   Add your environment variables (e.g., `TELEGRAM_BOT_TOKEN`, `OPENAI_API_KEY`). See the **Configuration** section for more details.

### 3. Set up API Gateway 🌉

1.  **Create an API Gateway:**
    *   In the Lambda function overview, click "Add trigger".
    *   Select "API Gateway".
    *   Choose "Create a new API".
    *   **API type:** HTTP API.
    *   **Security:** Open.
    *   Click "Add".

2.  **Get your Endpoint URL:**
    *   After the trigger is created, you will see an "API endpoint" URL. Copy this URL.

### 4. Set the Webhook 🎣

Finally, you need to tell Telegram where to send updates. Set the webhook by visiting the following URL in your browser. Replace `<YOUR_BOT_TOKEN>` and `<YOUR_API_GATEWAY_URL>` with your actual values.

```
https://api.telegram.org/bot<YOUR_BOT_TOKEN>/setWebhook?url=<YOUR_API_GATEWAY_URL>
```

You should see a message `{"ok":true,"result":true,"description":"Webhook was set"}`.

## 🔧 Configuration

The bot is configured using environment variables.

*   `TELEGRAM_BOT_TOKEN`: Your Telegram bot token from BotFather.
*   `OPENAI_API_KEY`: Your OpenAI API key (if used).

### Local Development

For local development, you can create a `.env` file in the root of the project:

```
TELEGRAM_BOT_TOKEN=your-token
OPENAI_API_KEY=your-openai-key
```

### AWS Lambda

In the AWS Lambda function configuration, add the following environment variables:

*   `TELEGRAM_BOT_TOKEN`: `your-token`
*   `OPENAI_API_KEY`: `your-openai-key`

## 📦 Dependencies

*   [python-telegram-bot](https://python-telegram-bot.org/)
