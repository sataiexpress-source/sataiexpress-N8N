# SatAI Express – n8n Workflow Collection

> A curated collection of powerful n8n workflows for automation and AI integration
>
> [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
> [![n8n](https://img.shields.io/badge/n8n-workflows-FF6D5A)](https://n8n.io)
>
> ## 🚀 Overview
>
> This is my personal collection of n8n workflows for automation and AI integration. These workflows are designed to automate various tasks including travel planning, content generation, data processing, and AI-powered operations.
>
> **Note**: This is a personal repository. You're welcome to use these workflows, but I'm not accepting external contributions at this time.
>
> ## 📦 Available Workflows
>
> ### 🧠 Personal Knowledge Base (Agentic RAG System)
>
> A multi-agent Retrieval-Augmented Generation (RAG) system for querying a personal knowledge base stored in Supabase. The system uses local Ollama models for both embeddings and inference, routing queries intelligently across three separate knowledge stores: notes, bookmarks, and references.
>
> | Workflow Name | File | Description | Status |
> |---|---|---|---|
> | Main: Personal KB Assistant | `Main_ Personal KB Assistant.json` | Orchestrator that receives chat messages, classifies the query using a RouterAgent, and fans out to one or more specialist sub-agents (Notes, Bookmarks, References). Combines results into a single response. | ✅ Active |
> | Agent: Bookmark Specialist | `Agent_ Bookmark Specialist.json` | Sub-agent called by the Main assistant. Searches the `bookmarks_kb` Supabase vector store for saved articles, blog posts, and web content. Returns cited answers grounded only in retrieved chunks. | ✅ Active |
> | Agent: Notes Specialist | `Agent_ Notes Specialist.json` | Sub-agent called by the Main assistant. Searches the `notes_kb` Supabase vector store for personal notes, journal entries, and written ideas. Returns cited answers with source metadata. | ✅ Active |
> | Agent: Reference Specialist | `Agent_ Reference Specialist.json` | Sub-agent called by the Main assistant. Searches the `references_kb` Supabase vector store for books, academic papers, and authoritative long-form material. Returns scholarly, cited answers. | ✅ Active |
> | SimpleRAGSetup | `SimpleRAGSetup.json` | Standalone single-agent RAG chatbot that directly queries all three Supabase vector stores (notes, bookmarks, references) in one workflow. Useful as a simpler alternative or starting point before using the full multi-agent setup. | ✅ Active |
>
> ### 📥 Supabase Ingestion Workflows
>
> These three workflows handle populating each knowledge base. Each presents a web form where you paste a title and content; the content is chunked, embedded via Ollama (`llama3.2:1b`), and inserted into the corresponding Supabase vector table.
>
> | Workflow Name | File | Target Table | Status |
> |---|---|---|---|
> | Supabase: Ingest: Notes | `Supabase_Ingest_ Notes.json` | `notes_kb` | ✅ Active |
> | Supabase: Ingest: Bookmarks | `Supabase_Ingest_ bookmarks.json` | `bookmarks_kb` | ✅ Active |
> | Supabase: Ingest: References | `Supabase_Ingest_ References.json` | `references_kb` | ✅ Active |
>
> ### ✈️ Other Workflows
>
> | Workflow Name | File | Description | Status |
> |---|---|---|---|
> | Travel Planner | `Travel Planner.json` | Automated travel planning and itinerary generation | ✅ Active |
>
> ---
>
> ## 🛠️ Prerequisites
>
> - n8n instance (self-hosted or cloud)
> - - Node.js 18+ (for self-hosted installations)
>   - - [Ollama](https://ollama.ai) running locally with the following models pulled:
>     -   - `qwen2.5:7b` or `llama3.1:8b` — chat/reasoning model
>         -   - `llama3.2:1b` — embedding model
>             - - [Supabase](https://supabase.com) project with vector store tables set up (`notes_kb`, `bookmarks_kb`, `references_kb`)
>               - - Required API keys (configure based on workflow needs):
>                 -   - OpenAI API key (optional, for AI-powered workflows not using Ollama)
>                     -   - Supabase API key and URL
>                      
>                         - ## 📥 Installation
>                      
>                         - ### Option 1: Import Individual Workflows
>                      
>                         - 1. Open your n8n instance
> 2. Click on **Workflows → Import from File**
> 3. 3. Select the desired `.json` file from this repository
>    4. 4. Configure your credentials and environment variables
>      
>       5. ### Option 2: Clone Repository
>      
>       6. ```bash
>          git clone https://github.com/sataiexpress-source/sataiexpress-N8N.git
>          cd sataiexpress-N8N
>          ```
>
> Then import workflows manually through the n8n UI.
>
> ## ⚙️ Configuration
>
> ### Environment Variables
>
> Create a `.env` file (not committed to repo) with your API keys:
>
> ```env
> OPENAI_API_KEY=your_key_here
> N8N_WEBHOOK_URL=your_webhook_url
> SUPABASE_URL=your_supabase_url
> SUPABASE_KEY=your_supabase_key
> ```
>
> ### Credentials Setup
>
> Each workflow requires specific credentials to be configured in n8n:
>
> 1. **Ollama Account**: Connection to your local Ollama instance (used for both chat and embedding models)
> 2. 2. **Supabase API** (`RAG-Supabase`): API key and URL for your Supabase project — used by all RAG and ingestion workflows
>    3. 3. **OpenAI Account**: For any AI-powered nodes not using Ollama
>       4. 4. **HTTP Request Auth**: For external API calls
>         
>          5. Configure credentials in n8n's credential manager before activating any workflow.
>         
>          6. ## 📖 Usage
>         
>          7. ### Personal Knowledge Base System
>
> The recommended setup order is:
>
> 1. **Set up Supabase** — create the three vector tables (`notes_kb`, `bookmarks_kb`, `references_kb`) with the `pgvector` extension enabled and matching RPC functions (`match_notes_kb`, `match_bookmarks_kb`, `match_references_kb`).
> 2. 2. **Run ingestion workflows** — use the three `Supabase_Ingest_` workflows to populate your knowledge bases via their web forms.
>    3. 3. **Activate the Main assistant** (`Main_ Personal KB Assistant.json`) and the three specialist agents.
>       4. 4. **Chat** via the n8n chat trigger to query your knowledge base.
>         
>          5. Use `SimpleRAGSetup.json` as a simpler single-workflow alternative that queries all three stores directly without sub-agents.
>         
>          6. ### Travel Planner Workflow
>         
>          7. 1. Import `Travel Planner.json` into your n8n instance
> 2. Configure required credentials
> 3. 3. Customize workflow parameters to match your needs
>    4. 4. Test with sample data before production use
>      
>       5. ## 🔒 Security Best Practices
>      
>       6. - ⚠️ Never commit API keys to the repository
>          - - Use n8n's credential system for sensitive data
>            - - Review workflows before importing to production
>              - - Keep your n8n instance updated
>                - - Regularly audit workflow permissions
>                 
>                  - ## 📝 About This Repository
>                 
>                  - This is a personal collection of n8n workflows. Feel free to use and adapt these workflows for your own projects, but please note that this repository is maintained solely by the owner.
>                 
>                  - ## 📄 License
>
> This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
>
> ## 🙏 Acknowledgments
>
> - [n8n](https://n8n.io) - Workflow automation platform
> - - [Ollama](https://ollama.ai) - Local LLM inference
>   - - [Supabase](https://supabase.com) - Vector store and database
>     - - [OpenAI](https://openai.com) - AI capabilities
>      
>       - ## 📞 Questions or Issues?
>      
>       - If you find issues with these workflows or have questions about how they work, feel free to open an issue. However, please note this is a personal project and response times may vary.
>      
>       - ## 🗺️ Future Plans
>
> - Add more AI-powered workflows
> - - Create detailed workflow documentation
>   - - Add workflow templates library
>     - - Expand automation capabilities
>      
>       - ---
>
> Made with ❤️ using n8n
