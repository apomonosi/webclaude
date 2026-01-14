# Getting Started with Claude Code Web

Welcome to Claude Code Web! This guide will walk you through your first interaction with Claude and demonstrate how to use it effectively for coding tasks.

## Your First Session

### Accessing Claude Code Web

1. Open your web browser (Chrome, Firefox, Safari, or Edge)
2. Navigate to Claude Code Web platform
3. Start a new conversation
4. You're ready to code!

No installation, no configuration, no setup required.

## Simple Demo: Building a REST API Task Manager

Let's build a simple REST API for a task management system. This demo will showcase Claude Code Web's capabilities in understanding requirements, generating code, and providing explanations.

### Step 1: Define Your Requirements

Start by describing what you want to build:

!!! example "Your Request"
    "I need to build a simple REST API for a task manager using Python and Flask. It should support creating, reading, updating, and deleting tasks. Each task should have an id, title, description, and completion status."

### Step 2: Claude Generates the Code

Claude will provide you with a complete implementation:

```python
from flask import Flask, request, jsonify
from datetime import datetime
import uuid

app = Flask(__name__)

# In-memory storage (use a database in production)
tasks = {}

@app.route('/tasks', methods=['GET'])
def get_tasks():
    """Get all tasks"""
    return jsonify(list(tasks.values())), 200

@app.route('/tasks/<task_id>', methods=['GET'])
def get_task(task_id):
    """Get a specific task"""
    task = tasks.get(task_id)
    if not task:
        return jsonify({'error': 'Task not found'}), 404
    return jsonify(task), 200

@app.route('/tasks', methods=['POST'])
def create_task():
    """Create a new task"""
    data = request.get_json()

    # Validation
    if not data or 'title' not in data:
        return jsonify({'error': 'Title is required'}), 400

    task_id = str(uuid.uuid4())
    task = {
        'id': task_id,
        'title': data['title'],
        'description': data.get('description', ''),
        'completed': False,
        'created_at': datetime.utcnow().isoformat()
    }

    tasks[task_id] = task
    return jsonify(task), 201

@app.route('/tasks/<task_id>', methods=['PUT'])
def update_task(task_id):
    """Update an existing task"""
    task = tasks.get(task_id)
    if not task:
        return jsonify({'error': 'Task not found'}), 404

    data = request.get_json()
    task['title'] = data.get('title', task['title'])
    task['description'] = data.get('description', task['description'])
    task['completed'] = data.get('completed', task['completed'])

    return jsonify(task), 200

@app.route('/tasks/<task_id>', methods=['DELETE'])
def delete_task(task_id):
    """Delete a task"""
    if task_id not in tasks:
        return jsonify({'error': 'Task not found'}), 404

    del tasks[task_id]
    return jsonify({'message': 'Task deleted successfully'}), 200

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

### Step 3: Ask for Improvements

You can now iterate and ask for improvements:

!!! example "Follow-up Request"
    "Can you add input validation and error handling for invalid JSON?"

Claude will provide an updated version with enhanced error handling:

```python
@app.route('/tasks', methods=['POST'])
def create_task():
    """Create a new task with enhanced validation"""
    try:
        data = request.get_json()
    except Exception:
        return jsonify({'error': 'Invalid JSON format'}), 400

    # Validation
    if not data:
        return jsonify({'error': 'Request body is required'}), 400

    if 'title' not in data:
        return jsonify({'error': 'Title is required'}), 400

    if not isinstance(data['title'], str) or not data['title'].strip():
        return jsonify({'error': 'Title must be a non-empty string'}), 400

    if 'description' in data and not isinstance(data['description'], str):
        return jsonify({'error': 'Description must be a string'}), 400

    task_id = str(uuid.uuid4())
    task = {
        'id': task_id,
        'title': data['title'].strip(),
        'description': data.get('description', '').strip(),
        'completed': False,
        'created_at': datetime.utcnow().isoformat()
    }

    tasks[task_id] = task
    return jsonify(task), 201
```

### Step 4: Request Tests

Now ask Claude to write tests for your API:

!!! example "Testing Request"
    "Can you write unit tests for this API using pytest?"

```python
import pytest
import json
from app import app, tasks

@pytest.fixture
def client():
    app.config['TESTING'] = True
    with app.test_client() as client:
        yield client
    tasks.clear()

def test_create_task(client):
    """Test creating a new task"""
    response = client.post('/tasks',
                          data=json.dumps({'title': 'Test Task', 'description': 'Test Description'}),
                          content_type='application/json')

    assert response.status_code == 201
    data = json.loads(response.data)
    assert data['title'] == 'Test Task'
    assert data['description'] == 'Test Description'
    assert data['completed'] == False
    assert 'id' in data

def test_create_task_missing_title(client):
    """Test creating task without title"""
    response = client.post('/tasks',
                          data=json.dumps({'description': 'No title'}),
                          content_type='application/json')

    assert response.status_code == 400
    data = json.loads(response.data)
    assert 'error' in data

def test_get_all_tasks(client):
    """Test retrieving all tasks"""
    # Create two tasks
    client.post('/tasks',
                data=json.dumps({'title': 'Task 1'}),
                content_type='application/json')
    client.post('/tasks',
                data=json.dumps({'title': 'Task 2'}),
                content_type='application/json')

    response = client.get('/tasks')
    assert response.status_code == 200
    data = json.loads(response.data)
    assert len(data) == 2

def test_update_task(client):
    """Test updating a task"""
    # Create a task
    create_response = client.post('/tasks',
                                  data=json.dumps({'title': 'Original'}),
                                  content_type='application/json')
    task_id = json.loads(create_response.data)['id']

    # Update it
    response = client.put(f'/tasks/{task_id}',
                         data=json.dumps({'title': 'Updated', 'completed': True}),
                         content_type='application/json')

    assert response.status_code == 200
    data = json.loads(response.data)
    assert data['title'] == 'Updated'
    assert data['completed'] == True

def test_delete_task(client):
    """Test deleting a task"""
    # Create a task
    create_response = client.post('/tasks',
                                  data=json.dumps({'title': 'To Delete'}),
                                  content_type='application/json')
    task_id = json.loads(create_response.data)['id']

    # Delete it
    response = client.delete(f'/tasks/{task_id}')
    assert response.status_code == 200

    # Verify it's gone
    get_response = client.get(f'/tasks/{task_id}')
    assert get_response.status_code == 404
```

### Step 5: Running and Testing in the Environment

One of the powerful features of Claude Code Web is that each repository session comes with its own sandboxed environment. You can actually run and test code within this environment!

#### Understanding the Environment

Claude Code Web provides:

- **Isolated Cloud Container**: Your code runs in a Google Gvisor-based sandbox
- **Pre-installed Tools**: Common languages and tools (Python, Node.js, etc.)
- **Package Installation**: You can install dependencies via npm, pip, etc.
- **Command Execution**: Run tests, builds, and other shell commands
- **Temporary Storage**: Files persist during your session

!!! info "Environment Lifecycle"
    The environment is created when you start working with a repository and persists throughout your session. Once you close the session, the environment is cleaned up.

#### Example: Running the Task Manager API

Let's continue with our Flask API example and actually run it in the environment:

!!! example "Running the Application"
    "Can you run this Flask application and test it?"

Claude can execute commands in the environment:

```bash
# Install dependencies
pip install flask pytest

# Run the application in the background
python app.py &

# Wait a moment for the server to start
sleep 2

# Test the API with curl
curl -X POST http://localhost:5000/tasks \
  -H "Content-Type: application/json" \
  -d '{"title": "My First Task", "description": "Testing the API"}'

# Get all tasks
curl http://localhost:5000/tasks
```

**Expected Output:**
```json
[
  {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "title": "My First Task",
    "description": "Testing the API",
    "completed": false,
    "created_at": "2026-01-14T10:30:00"
  }
]
```

#### Example: Running Tests

You can run the test suite we created:

!!! example "Running Tests"
    "Run the pytest tests and show me the results"

```bash
# Run all tests with verbose output
pytest test_app.py -v

# Run with coverage report
pytest test_app.py --cov=app --cov-report=term-missing
```

**Example Output:**
```
test_app.py::test_create_task PASSED                    [ 16%]
test_app.py::test_create_task_missing_title PASSED      [ 33%]
test_app.py::test_get_all_tasks PASSED                  [ 50%]
test_app.py::test_update_task PASSED                    [ 66%]
test_app.py::test_delete_task PASSED                    [ 83%]
test_app.py::test_get_task_not_found PASSED             [100%]

---------- coverage: platform linux, python 3.10.0 -----------
Name      Stmts   Miss  Cover   Missing
---------------------------------------
app.py       45      2    96%   12-13
---------------------------------------
TOTAL        45      2    96%

6 passed in 0.43s
```

#### Example: Testing Different Scenarios

You can test edge cases and error conditions:

```bash
# Test with invalid JSON
curl -X POST http://localhost:5000/tasks \
  -H "Content-Type: application/json" \
  -d 'invalid json'

# Test with missing required field
curl -X POST http://localhost:5000/tasks \
  -H "Content-Type: application/json" \
  -d '{"description": "No title provided"}'

# Test getting non-existent task
curl http://localhost:5000/tasks/invalid-id
```

#### Setting Up Complex Environments

For more complex projects, you can set up the full environment:

!!! example "Node.js + React Project Setup"
    ```bash
    # Install backend dependencies
    cd backend
    npm install

    # Install frontend dependencies
    cd ../frontend
    npm install

    # Run tests
    npm test

    # Build the project
    npm run build

    # Check for linting errors
    npm run lint
    ```

#### Automating Environment Setup with Hooks

You can automate environment setup using SessionStart hooks in `.claude/settings.json`:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "setup.sh"
          }
        ]
      }
    ]
  }
}
```

**setup.sh:**
```bash
#!/bin/bash
echo "Setting up environment..."

# Install Python dependencies
if [ -f requirements.txt ]; then
  pip install -r requirements.txt
fi

# Install Node dependencies
if [ -f package.json ]; then
  npm install
fi

# Run database migrations
if [ -f migrate.sh ]; then
  ./migrate.sh
fi

echo "Environment ready!"
```

#### What You Can Do in the Environment

✅ **Supported Operations:**

- Run tests (pytest, jest, mocha, etc.)
- Execute builds (npm build, cargo build, etc.)
- Install packages (pip, npm, cargo, gem, etc.)
- Run linters and formatters (eslint, black, prettier, etc.)
- Execute scripts and commands
- Test CLI tools and applications
- Run development servers (limited - background processes)
- Validate configuration files
- Generate documentation
- Run database migrations (with SQLite or in-memory databases)

❌ **Limitations:**

- No persistent storage between sessions
- No external network access (in most cases)
- No GUI applications
- Limited long-running background processes
- No access to external databases or services
- Resource constraints (CPU, memory, disk)

#### Best Practices for Environment Usage

1. **Test Before Committing**: Always run tests in the environment before finalizing code

2. **Validate Dependencies**: Check that all required packages install correctly
   ```bash
   pip install -r requirements.txt --dry-run
   ```

3. **Check Build Output**: Verify builds complete without errors
   ```bash
   npm run build 2>&1 | tee build.log
   ```

4. **Use Linters**: Catch issues early with automated linting
   ```bash
   flake8 src/ tests/
   eslint src/**/*.js
   ```

5. **Run Full Test Suite**: Don't just test happy paths
   ```bash
   pytest --cov=. --cov-report=html
   npm test -- --coverage
   ```

#### Debugging in the Environment

When things don't work, use the environment for debugging:

```bash
# Check Python version
python --version

# Verify package installation
pip list | grep flask

# Check for syntax errors
python -m py_compile app.py

# Run with debug output
python -v app.py

# Check environment variables
env | grep -i python
```

#### Example: Full Development Workflow

Here's a complete workflow using the environment:

```bash
# 1. Setup
pip install -r requirements.txt

# 2. Run linter
flake8 app.py test_app.py

# 3. Format code
black app.py test_app.py

# 4. Run tests with coverage
pytest --cov=app --cov-report=term-missing test_app.py

# 5. Type checking (if using type hints)
mypy app.py

# 6. Security check
bandit -r app.py

# 7. Run the application
python app.py
```

#### Continuous Integration Simulation

You can simulate CI/CD pipelines:

```bash
#!/bin/bash
# ci-test.sh

set -e  # Exit on any error

echo "Running CI checks..."

echo "1. Installing dependencies..."
pip install -r requirements.txt
pip install pytest coverage flake8 black mypy

echo "2. Code formatting check..."
black --check app.py test_app.py

echo "3. Linting..."
flake8 app.py test_app.py

echo "4. Type checking..."
mypy app.py

echo "5. Running tests..."
pytest --cov=app --cov-report=term test_app.py

echo "6. Coverage check (minimum 80%)..."
pytest --cov=app --cov-fail-under=80 test_app.py

echo "✅ All CI checks passed!"
```

!!! tip "Pro Tip"
    Ask Claude to create a comprehensive test script like the one above for your project. It ensures consistency and catches issues before they reach production.

## Understanding the Workflow

This demo illustrates the typical Claude Code Web workflow:

### 1. Natural Language Requests

You describe what you want in plain English. No need for technical specifications or formal syntax.

### 2. Complete Solutions

Claude provides working code, not just snippets. The code includes:

- Proper structure and organization
- Error handling
- Comments and documentation
- Best practices

### 3. Iterative Refinement

You can ask for improvements, additional features, or modifications. Claude maintains context and builds on previous responses.

### 4. Comprehensive Support

Beyond just code generation, Claude can:

- Write tests
- Generate documentation
- Explain algorithms
- Suggest optimizations
- Review and debug code

## Trying It Yourself

### Example Prompts to Try

Here are some prompts you can try in your first session:

=== "Code Generation"

    - "Create a Python function to validate email addresses using regex"
    - "Write a React component for a user profile card"
    - "Build a SQL query to find the top 10 customers by revenue"

=== "Debugging"

    - "This code throws a NullPointerException, can you help fix it?"
    - "Why is my loop running infinitely?"
    - "This function returns wrong results for edge cases"

=== "Explanation"

    - "Explain how quicksort works with an example"
    - "What's the difference between async/await and promises?"
    - "How does garbage collection work in Java?"

=== "Refactoring"

    - "Can you refactor this code to be more efficient?"
    - "How can I make this function more readable?"
    - "Convert this code to use design patterns"

## Best Practices for First-Time Users

### Be Specific

Instead of: "Make a website"

Try: "Create a landing page with HTML/CSS that has a hero section, features list, and contact form"

### Provide Context

Share relevant information:

- Programming language and version
- Framework or library being used
- Constraints or requirements
- Expected input/output

### Iterate Gradually

Start with a basic version, then ask for improvements:

1. Get working code first
2. Add error handling
3. Optimize performance
4. Add tests
5. Improve documentation

### Ask Questions

If something is unclear:

- "Why did you use this approach?"
- "What are the trade-offs of this solution?"
- "Are there alternative implementations?"

## Common First-Timer Mistakes to Avoid

!!! warning "Avoid These"

    ❌ **Being Too Vague**: "Make it better" doesn't give Claude enough direction

    ❌ **Expecting Magic**: Claude is powerful but needs clear requirements

    ❌ **Not Testing**: Always test the code Claude provides

    ❌ **Ignoring Context**: Provide necessary background information

    ❌ **Skipping Review**: Review and understand the code before using it

!!! success "Do This Instead"

    ✅ **Be Specific**: "Add input validation for email and phone fields"

    ✅ **Set Clear Goals**: Define what success looks like

    ✅ **Test Thoroughly**: Run tests and verify edge cases

    ✅ **Share Context**: Mention your tech stack and constraints

    ✅ **Learn and Understand**: Ask Claude to explain parts you don't understand

## Quick Reference Commands

Here are some useful command patterns:

```plaintext
# Code Generation
"Write a [language] function that [does something]"
"Create a [framework] component for [purpose]"
"Generate a [type] class with [features]"

# Debugging
"This code has a bug: [paste code]"
"Why does [code] throw [error]?"
"Fix the [issue] in this code"

# Explanation
"Explain how [concept] works"
"What does this code do: [paste code]"
"What's the difference between [A] and [B]?"

# Optimization
"Make this code more efficient: [paste code]"
"Refactor this to use [pattern/approach]"
"Optimize this for [metric like speed/memory]"

# Testing
"Write unit tests for: [paste code]"
"Create test cases for [functionality]"
"Generate integration tests for [API/component]"
```

## Next Steps

Now that you've completed your first demo:

1. **Practice**: Try building more complex projects
2. **Learn Effectively**: Read [How to Use Claude Web Effectively](effective-usage.md)
3. **Follow Best Practices**: Check out [Best Practices for Coding Repositories](best-practices.md)
4. **Explore Use Cases**: Discover [Research and Non-Code Applications](research-use-cases.md)

Ready to level up? Head to [Effective Usage](effective-usage.md) to learn advanced techniques.
