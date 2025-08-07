- `type` must be one of:  
     `feat` (new feature)  
     `fix` (bug fix)  
     `docs` (documentation)  
     `style` (formatting)  
     `refactor` (code restructuring)  
     `test` (testing)  
     `chore` (maintenance)  
   - `scope` is optional (module/file affected)  
   - `description` is imperative tense, under 72 chars  

2. **Body** (optional)  
   - Explains WHAT changed and WHY  
   - Wrapped at 72 characters  
   - Uses bullet points if complex  

3. **Footer** (optional)  
   - References issues (e.g., `Closes #123`)  
   - Documents breaking changes  

**Example of my perfect form:**
graph TD
    A[Client] --> B[API Gateway]
    B --> C[Auth Service]
    C --> D[Database]
    ALTER SYSTEM SET max_connections = 200;
    node --inspect=9229 app.js
    ALTER SYSTEM SET max_connections = 200;
    # Clone repository  
git clone https://github.com/org/repo.git --branch develop  

# Initialize services  
docker-compose -f docker-compose.dev.yml up --build  

# Verify installation  
curl http://localhost:8080/healthcheck
graph TD
    A[Client] --> B[API Gateway]
    B --> C[Auth Service]
    C --> D[Database]
