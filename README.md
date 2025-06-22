# AI Agent Project

# Overview
This project integrates multiple AI agents to streamline the process of searching, scraping, and comparing product information across various eCommerce websites. It leverages advanced tools and AI agents to suggest search queries, collect product information, and generate procurement reports, tailored for a company like Rankyx, which seeks to optimize its product search and purchase process.

# Requirements
Before you begin, make sure you have the following installed:

# CrewAI - Core framework for AI agents

pip install -qU crewai[tools,agentops]==0.114.0

# Tavily Python Client - For interacting with the Tavily search engine
pip install -qU tavily-python

# ScrapeGraph Python Client - For scraping web pages and extracting product details
pip install -qU scrapegraph-py

# Setup
Initialize the environment: You will need API keys for the services integrated into the agents.

. Agentops

. Tavily

. ScrapeGraph

# Set up environment variables for the API keys:

os.environ["GEMINI_API_KEY"] = userdata.get('gemenai-colab')
os.environ["AGENTOPS_API_KEY"] = userdata.get('agentops-colab')

# Create output directories to store the results:
output_dir = "./ai-agent-output"
os.makedirs(output_dir, exist_ok=True)

# Initialize the LLM and Clients for the agents:
basic_llm = LLM(model="gemini/gemini-2.0-flash", temperature=0)
tavily_client = TavilyClient(api_key=userdata.get('tavily-search'))
scrap_client = Client(api_key=userdata.get('scrapgraph'))

# Agents
The project involves the following agents:

# Agent A - Search Queries Recommendation
    This agent generates a list of suggested search queries based on the context provided by the company.

    Goal: Provide a list of suggested search queries for products.

    Task: Recommend up to 10 specific search queries based on the company's target websites and products.

# Agent B - Search Engine Agent
    This agent uses the search queries recommended by Agent A to search for products on various eCommerce websites.

    Goal: Search for products based on the suggested search queries.

    Tool: Tavily API for search-based queries.

# Agent C - Web Scraping Agent
    This agent extracts detailed product information from the eCommerce websites.

    Goal: Scrape and extract product details such as titles, images, prices, and specifications from the websites.

    Tool: ScrapeGraph API for smart scraping of web pages.

# Agent D - Procurement Report Author Agent
    After gathering product information, this agent generates a professional HTML procurement report.

    Goal: Create a professional procurement report, summarizing product details, prices, and recommendations.

    Task: Use Bootstrap CSS to create a structured HTML report with sections like Executive Summary, Findings, Analysis, and Recommendations.

# Running the AI Crew
The agents are orchestrated in a sequential process to ensure that tasks are executed step by step.

# Running the Crew
To run the AI crew and complete the tasks, use the following code:

ranex_crew = Crew(
    agents=[
        search_queries_recommendation_agent,
        search_engine_agent,
        scraping_agent,
        procurement_report_author_agent,
    ],
    tasks=[
        search_queries_recommendation_task,
        search_engine_task,
        scraping_task,
        procurement_report_author_task,
    ],
    process=Process.sequential
)

crew_results = ranex_crew.kickoff(
    inputs={
        "product_name": "coffee machine for office",
        "websites_list": ["www.amazon.eg", "www.jumia.eg"],
        "country_name": "Egypt",
        "no_keywords": 10,
        "language": "Arabic",
        "score_th": 0.10,
    }
)
## This will execute all the tasks, generating the procurement report with the necessary product details.

# Expected Output
.Suggested Search Queries: A list of search queries in a JSON file.

.Search Results: The search results from multiple eCommerce websites in a JSON file.

.Product Details: Detailed product specifications scraped from the websites.

.Procurement Report: A professionally formatted HTML report summarizing the procurement findings.