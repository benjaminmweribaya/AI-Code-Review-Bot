# AI Code Review Bot

## **Overview**
AI Code Review Bot is a GitHub-integrated tool designed to automate and enhance the code review process using AI-powered analysis. The bot listens to GitHub pull requests, performs static analysis, and leverages AI to provide feedback on code quality, refactoring suggestions, and security vulnerabilities.

## **Tech Stack**
- **Backend:** Node.js (NestJS/Express)
- **AI Processing:** OpenAI API
- **Database:** PostgreSQL
- **GitHub Integration:** GitHub Webhooks
- **Authentication:** OAuth (GitHub Login)
- **Frontend (Optional):** React.js/Next.js (for dashboard and analytics)

## **Key Features**
- **GitHub PR Listener:** Automatically detects and reviews pull requests.
- **Static Analysis:** Detects syntax errors, code smells, and unused variables.
- **AI-Powered Code Review:** Provides AI-generated suggestions for improvements.
- **Security Vulnerability Detection:** Identifies risks like SQL injection and hardcoded credentials.
- **Code Quality Scoring:** Assigns a score based on readability, maintainability, and best practices.
- **Customizable Rules:** Developers can configure linting and AI behavior.
- **Automated Comments:** Posts inline feedback directly on GitHub PRs.
- **Notifications:** Sends alerts via Slack, Discord, or email.
- **CI/CD Integration:** Blocks merges if severe issues are detected (configurable).

## **Project Structure**
```
📦 ai-code-review-bot
 ┣ 📂 backend
 ┃ ┣ 📂 src
 ┃ ┃ ┣ 📂 controllers       # API controllers
 ┃ ┃ ┣ 📂 services          # Business logic
 ┃ ┃ ┣ 📂 models            # Database models
 ┃ ┃ ┣ 📂 utils             # Helper functions
 ┃ ┃ ┣ 📂 middleware        # Authentication and validation
 ┃ ┃ ┣ index.ts            # App entry point
 ┃ ┃ ┗ app.module.ts       # Main module setup (NestJS)
 ┃ ┣ 📂 config             # Configuration files
 ┃ ┣ 📂 database           # Database setup and migrations
 ┃ ┣ 📂 tests              # Unit and integration tests
 ┃ ┣ package.json          # Dependencies and scripts
 ┃ ┣ tsconfig.json         # TypeScript configuration
 ┃ ┗ .env                  # Environment variables
 ┣ 📂 frontend (optional)
 ┃ ┣ 📂 src
 ┃ ┃ ┣ 📂 components        # Reusable UI components
 ┃ ┃ ┣ 📂 pages             # Dashboard views
 ┃ ┃ ┣ 📂 hooks             # Custom React hooks
 ┃ ┃ ┣ 📂 utils             # Helper functions
 ┃ ┃ ┣ index.tsx           # App entry point
 ┃ ┃ ┗ App.tsx             # Main application file
 ┃ ┣ package.json          # Dependencies
 ┃ ┣ tailwind.config.js    # Tailwind CSS configuration
 ┃ ┗ .env                  # Frontend environment variables
 ┣ 📂 docs                 # Documentation and API reference
 ┣ .github/workflows       # GitHub Actions for CI/CD
 ┣ docker-compose.yml      # Docker setup
 ┣ README.md               # Project documentation
 ┗ LICENSE                 # License file
```

## **Database Schema (PostgreSQL)**
### **Tables and Relationships**
1. **Users Table (`users`)**
   - `id` (PK)
   - `github_id` (Unique)
   - `name`
   - `email`
   - `role` (`admin`, `developer`)

2. **Repositories Table (`repositories`)**
   - `id` (PK)
   - `github_repo_id` (Unique)
   - `name`
   - `owner_id` (FK → `users.id`)

3. **Pull Requests Table (`pull_requests`)**
   - `id` (PK)
   - `github_pr_id` (Unique)
   - `repository_id` (FK → `repositories.id`)
   - `author_id` (FK → `users.id`)
   - `status` (`open`, `merged`, `closed`)
   - `created_at`, `updated_at`

4. **Code Reviews Table (`code_reviews`)**
   - `id` (PK)
   - `pull_request_id` (FK → `pull_requests.id`)
   - `reviewer_id` (FK → `users.id`)
   - `score` (Quality score out of 100)
   - `security_issues` (JSON array)
   - `suggestions` (JSON array)
   - `created_at`

5. **Comments Table (`comments`)**
   - `id` (PK)
   - `review_id` (FK → `code_reviews.id`)
   - `file_path`
   - `line_number`
   - `comment_text`
   - `severity` (`info`, `warning`, `error`)
   - `created_at`

6. **Notifications Table (`notifications`)**
   - `id` (PK)
   - `user_id` (FK → `users.id`)
   - `message`
   - `sent_at`

7. **Settings Table (`settings`)**
   - `id` (PK)
   - `user_id` (FK → `users.id`)
   - `repository_id` (FK → `repositories.id`)
   - `linting_rules` (JSON)
   - `ai_model_config` (JSON)

## **Setup & Installation**
### **Prerequisites**
- Node.js (v16+)
- PostgreSQL
- Docker (optional)
- GitHub Developer Account

### **Installation Steps**
#### **Backend Setup**
```sh
git clone https://github.com/benjaminmweribaya/ai-code-review-bot.git
cd ai-code-review-bot/backend
cp .env.example .env  # Set environment variables
npm install
npm run migrate  # Run database migrations
npm run dev  # Start development server
```
#### **Frontend Setup (Optional)**
```sh
cd ../frontend
cp .env.example .env
npm install
npm start
```

## **API Endpoints**
| Method | Endpoint | Description |
|--------|---------|-------------|
| POST | `/webhook/github` | Receives PR events from GitHub |
| GET | `/repositories` | Lists all linked repositories |
| GET | `/pull-requests/:id` | Retrieves details of a pull request |
| GET | `/reviews/:id` | Fetches AI-generated review details |
| POST | `/reviews/:id/comments` | Adds a review comment |

## **GitHub Webhook Configuration**
1. Go to your repository **Settings** > **Webhooks**.
2. Click **Add Webhook**.
3. Set **Payload URL** to `https://yourbackend.com/webhook/github`.
4. Select **Content type** as `application/json`.
5. Choose **Trigger events**: `Pull Request`, `Push`.
6. Click **Add Webhook**.

## **Contributing**
1. Fork the repository.
2. Create a new branch: `git checkout -b feature-branch`.
3. Commit your changes: `git commit -m "Add feature"`.
4. Push to the branch: `git push origin feature-branch`.
5. Open a Pull Request.

## **License**
This project is licensed under the MIT License.

---

This README provides a **detailed overview of the project**, including its **architecture, database schema, setup instructions, API reference, and GitHub integration**. Let me know if you need any modifications!

