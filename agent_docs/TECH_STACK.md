Frontend & Visualization
Site Builder: Framer (for the marketing/landing page) or Next.js (for the functional dashboard).

Visualization: React Flow (to visualize the Threat Correlation Graph) and D3.js (for the Signal Fusion Heatmap).

Styling: Tailwind CSS + Shadcn UI for a clean, professional "Command Center" look.

Backend & Orchestration
Primary Runtime: Python 3.13 (required for the latest ML and MCP SDK features).

Agent Gateway: Dedalus Labs SDK (manages multi-model orchestration and secure auth).

Web Framework: FastAPI (handles asynchronous communication and streaming logs to the UI).

Dependency Management: uv (extremely fast; essential for rapid environment setup).

Data & State Management
Time-Series DB: TimescaleDB (PostgreSQL). Essential for the Signal Fusion Engine to store and query high-velocity logs and latency metrics.

Vector DB: ChromaDB (local/serverless). Used by the Binary Sentry to index and search for semantic code patterns and historical vulnerabilities.

In-Memory Store: Redis. Used for the Conway Decision Engine to store real-time risk scores and session state for the "Human-in-the-loop" approval flow.

Infrastructure & Tools (MCP Servers)
Containerization: Docker + Docker Compose. Required to sandbox the MCP servers and isolate the agent's execution environment.

Binary Analysis: Ghidra (Headless) + GhidrAssist MCP server. Allows the AI to decompile and audit machine code.

Cloud Control: K8s-MCP or AWS-MCP to read logs and scale pods.

Secure Vault Proxy: Custom FastMCP server that signs requests with local .env credentials.

🚀 Environment Setup Guide
Follow these steps to get your environment ready for the first hour of hacking.

1. Install the Project Manager
We use uv because it can install Python 3.13 and all dependencies in seconds.

Bash
# Install uv (macOS/Linux)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Initialize your project
uv init mosaic-aegis
cd mosaic-aegis
uv python pin 3.13
2. Launch the Infrastructure
Create a docker-compose.yml to spin up your data layers.

YAML
services:
  timescaledb:
    image: timescale/timescaledb:latest-pg16
    ports: ["5432:5432"]
    environment:
      - POSTGRES_PASSWORD=password
  redis:
    image: redis:alpine
    ports: ["6379:6379"]
3. Install Backend Dependencies
Bash
uv add fastapi uvicorn dedalus-labs mcp-sdk pydantic-settings loguru redis psycopg2-binary
4. Set Up Your Secure Vault
Create a .env file in your root directory. This is where your secrets live. Never feed these directly to the AI.

Code snippet
DEDALUS_API_KEY="your_key"
GITHUB_TOKEN="ghp_your_token"
AWS_ACCESS_KEY_ID="your_id"
🛠️ Recommended Developer Workflow
Step 1: Use Claude 3.5 Sonnet (via Dedalus) to write your first MCP tool for signal ingestion.

Step 2: Test your tools using the MCP Inspector to ensure they return the data you expect.

Step 3: Build the Signal Fusion logic as a standalone Python class so it's deterministic and easy to debug.