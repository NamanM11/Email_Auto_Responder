# Step-by-Step Setup Guide

Follow these steps to get your Email Auto Responder Flow up and running:

## Prerequisites

- Python 3.10, 3.11, 3.12, or 3.13 installed
- A Gmail account
- API keys for the required services

## Step 1: Install Python Dependencies

The project uses `uv` for dependency management. Install dependencies using one of these methods:

### Option A: Using uv (Recommended)
```bash
# Install uv if you haven't already
pip install uv

# Install project dependencies
uv sync
```

### Option B: Using pip
```bash
# Install crewAI and dependencies
pip install crewai[tools]>=0.152.0 langchain-tools>=0.1.34 crewai-tools>=0.58.0 google-auth-oauthlib>=1.2.1 google-api-python-client>=2.145.0
```

## Step 2: Set Up Google Gmail API Credentials

1. **Go to Google Cloud Console**: Visit [Google Cloud Console](https://console.cloud.google.com/)
2. **Create a New Project** (or select an existing one)
3. **Enable Gmail API**:
   - Go to "APIs & Services" > "Library"
   - Search for "Gmail API"
   - Click "Enable"
4. **Create OAuth 2.0 Credentials**:
   - Go to "APIs & Services" > "Credentials"
   - Click "Create Credentials" > "OAuth client ID"
   - Choose "Desktop app" as the application type
   - Download the credentials file
5. **Rename and Place the File**:
   - Rename the downloaded file to `credentials.json`
   - Place it in the root directory of the project: `c:\Users\mahes\OneDrive\Desktop\email_auto_responder_flow\`

**Quick Reference**: Follow the [official Google instructions](https://developers.google.com/gmail/api/quickstart/python#authorize_credentials_for_a_desktop_application)

## Step 3: Obtain API Keys

You need to obtain API keys for the following services:

### OpenAI API Key
1. Visit [OpenAI Platform](https://platform.openai.com/)
2. Sign up or log in
3. Go to API Keys section
4. Create a new API key
5. Copy the key

### Serper API Key (for search functionality)
1. Visit [Serper.dev](https://serper.dev/)
2. Sign up for a free account
3. Get your API key from the dashboard
4. Copy the key

### Tavily API Key (for search functionality)
1. Visit [Tavily](https://tavily.com/)
2. Sign up for an account
3. Get your API key from the dashboard
4. Copy the key

## Step 4: Create Environment Variables File

1. Create a `.env` file in the root directory of the project
2. Add the following content (replace with your actual keys):

```env
OPENAI_API_KEY=your_openai_api_key_here
SERPER_API_KEY=your_serper_api_key_here
TAVILY_API_KEY=your_tavily_api_key_here
MY_EMAIL=your_email@gmail.com
```

**Important**: 
- Replace all placeholder values with your actual API keys
- Use the email address you want to monitor (the one connected to your Gmail account)
- Never commit the `.env` file to version control (it's already in `.gitignore`)

## Step 5: First-Time Gmail Authorization

When you run the application for the first time, it will:
1. Open a browser window
2. Ask you to sign in to your Google account
3. Request permissions to access your Gmail
4. Generate a `token.json` file (this will be created automatically)

**Note**: You only need to do this once. The `token.json` file will be saved for future runs.

## Step 6: Run the Application

From the root directory of the project, run:

```bash
# Using uv (recommended)
uv run kickoff

# OR using Python directly
python -m email_auto_responder_flow.main
```

## What Happens When You Run It

1. **Initial Check**: The flow checks for new emails in your Gmail inbox
2. **Email Processing**: For each new email (not from yourself), it:
   - Filters and categorizes the email
   - Determines if action is required
   - Generates a draft response
3. **Continuous Monitoring**: The flow runs in a loop, checking for new emails every 180 seconds (3 minutes)

## Troubleshooting

### Issue: "Module not found" errors
**Solution**: Make sure you've installed all dependencies. Try:
```bash
uv sync
# or
pip install -e .
```

### Issue: "Credentials not found"
**Solution**: Ensure `credentials.json` is in the root directory and named exactly `credentials.json`

### Issue: "API key not found" errors
**Solution**: Check your `.env` file exists and contains all required keys with correct variable names

### Issue: Gmail authentication fails
**Solution**: 
- Make sure you've enabled Gmail API in Google Cloud Console
- Check that your OAuth credentials are set up correctly
- Delete `token.json` and try again to re-authenticate

### Issue: "Permission denied" when accessing Gmail
**Solution**: Make sure you've granted the necessary permissions during the OAuth flow

## Optional: Visualize the Flow

To see a visual representation of your flow:

```bash
uv run plot
```

This will generate a diagram showing how the email auto responder flow works.

## Next Steps

- Customize agents and tasks in `src/email_auto_responder_flow/crews/email_filter_crew/`
- Adjust the flow logic in `src/email_auto_responder_flow/main.py`
- Modify the check interval (currently 180 seconds) in `main.py`

## Support

For more help:
- Check the [CrewAI Documentation](https://docs.crewai.com)
- Visit the [GitHub Repository](https://github.com/joaomdmoura/crewai)
- Join the [Discord Community](https://discord.com/invite/X4JWnZnxPb)
