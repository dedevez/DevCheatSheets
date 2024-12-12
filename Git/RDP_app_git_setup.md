# Git and GitHub Setup for React-Django-PostgreSQL Stack Application

## 1. Initialize Git Repository

1. Navigate to your project directory:
   ```bash
   cd /path/to/your/project
   ```
2. Initialize a Git repository:
   ```bash
   git init
   ```

## 2. Set Up .gitignore

Create a `.gitignore` file in the root of your project directory and add the following content to exclude sensitive and unnecessary files:

### General Exclusions
```
# Logs
*.log

# Dependency directories
node_modules/

# Build output
build/
dist/

# System files
.DS_Store
*.swp
*.swo
Thumbs.db
```

### Django-Specific Exclusions
```
# Python bytecode
*.pyc
*.pyo
__pycache__/

# Environment variables
.env

# Django secret files
secrets.json

# SQLite database files
*.sqlite3

# Static files
/static/
/media/
```

### React-Specific Exclusions
```
# Build directories
/build/

# Environment variables
.env.local
.env.development.local
.env.test.local
.env.production.local
```

### PostgreSQL-Specific Exclusions
```
# Database backup files
*.sql
*.backup
```

### Docker-Specific Exclusions (if using Docker)
```
# Docker files
*.pid
docker-compose.override.yml

# Container logs
*.log
```

## 3. Create GitHub Repository

1. Log in to your GitHub account.
2. Create a new repository with the desired name.
3. Follow the instructions on GitHub to add the remote repository:
   ```bash
   git remote add origin https://github.com/your-username/your-repo-name.git
   ```

## 4. Commit and Push Initial Code

1. Stage all files for commit:
   ```bash
   git add .
   ```
2. Commit the changes with a meaningful message:
   ```bash
   git commit -m "Initial commit: Setup React-Django-PostgreSQL project"
   ```
3. Push the commit to GitHub:
   ```bash
   git branch -M main
   git push -u origin main
   ```

## 5. Branching Strategy

Use a branching strategy to organize your work:
- **main**: Stable, production-ready code.
- **develop**: Active development branch.
- Feature branches: Create separate branches for new features.

Example of creating and switching to a new branch:
```bash
git checkout -b feature/new-feature-name
```

## 6. Regular Updates

1. Pull the latest changes from the remote repository:
   ```bash
   git pull origin main
   ```
2. Merge branches when ready:
   ```bash
   git merge develop
   ```

## 7. Additional Notes

- Ensure all sensitive data (like API keys) is stored in environment files and excluded using `.gitignore`.
- Regularly review `.gitignore` to ensure no sensitive files are tracked.
- Use GitHub Actions or another CI/CD tool to automate deployment and testing.

