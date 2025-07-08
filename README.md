Project Setup
📁 Folder Structure
mern-bug-tracker/
├── client/        # React frontend
├── server/        # Node.js backend
└── README.md
🔧 Backend Setup (server/)
Initialize:

bash
mkdir server && cd server
npm init -y
Install dependencies:

bash
npm install express mongoose cors
npm install --save-dev jest supertest jest-mock
Create core files:

index.js for entry point

routes/bugs.js for RESTful API

models/Bug.js

controllers/bugController.js

🖥️ Frontend Setup (client/)
Initialize Vite + React:

bash
npm create vite@latest client --template react
cd client && npm install
npm install axios react-router-dom
npm install --save-dev @testing-library/react jest
🐞 Application Features
✅ Bug Functionality:
Report new bugs

View bugs list

Update bug status: open, in-progress, resolved

Delete bugs

Each bug object might look like:

js
{
  title: "Login button not responsive",
  description: "Button stops working on mobile",
  status: "open",
  createdBy: "Joshua"
}
🧪 Testing Requirements
🔙 Backend
✅ Unit Tests
Test pure functions (e.g., input validation):

js
function isValidTitle(title) {
  return typeof title === 'string' && title.trim().length > 0;
}
Test using Jest:

js
test('Validates bug title', () => {
  expect(isValidTitle('Crash')).toBe(true);
});
✅ Integration Tests
Test Express routes with Supertest:

js
const request = require('supertest');
const app = require('../index');

describe('POST /api/bugs', () => {
  it('should create a bug', async () => {
    const res = await request(app)
      .post('/api/bugs')
      .send({ title: 'Bug A', description: '...', status: 'open' });

    expect(res.statusCode).toBe(201);
    expect(res.body.title).toBe('Bug A');
  });
});
✅ Mocking
Use jest.mock() to mock MongoDB calls:

js
jest.mock('../models/Bug', () => ({
  create: jest.fn(() => Promise.resolve({ title: 'Mocked Bug' })),
}));
🌐 Frontend
✅ Unit Tests
Use React Testing Library:

js
test('should disable submit button if title is empty', () => {
  render(<BugForm />);
  const button = screen.getByRole('button', { name: /submit/i });
  expect(button).toBeDisabled();
});
✅ Integration Tests
Verify bug submission flow:

js
test('submits form and displays new bug', async () => {
  render(<BugForm />);
  fireEvent.change(screen.getByPlaceholderText(/title/i), { target: { value: 'New Bug' } });
  fireEvent.click(screen.getByRole('button'));
  expect(await screen.findByText('New Bug')).toBeInTheDocument();
});
✅ State-Based Rendering
Test UI when bug list is empty:

js
test('shows empty state', () => {
  render(<BugList bugs={[]} />);
  expect(screen.getByText(/no bugs reported/i)).toBeInTheDocument();
});
🧹 Debugging Tasks
💥 Intentional Bugs
Introduce errors (e.g., wrong prop types, typos, missing keys).

🔍 Tools
Console Logs: console.log({ bug })

Chrome DevTools: Inspect state, props, network failures.

Node.js Inspector:

bash
node --inspect index.js
chrome://inspect
Error Boundaries (React):

js
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError() { return { hasError: true }; }
  render() { return this.state.hasError ? <Fallback /> : this.props.children; }
}
🚨 Error Handling
Backend (Express Middleware)
js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
});
Frontend (React)
Wrap components in ErrorBoundary.

📄 README.md Structure
🧭 Project Overview
"MERN Bug Tracker helps teams log and track issues in real time with reliability ensured through testing and debugging."

🛠️ Installation
bash
git clone https://github.com/yourname/mern-bug-tracker
cd server && npm install
cd ../client && npm install
🚀 Run Project
bash
# Server
npm run dev

# Client
npm run dev
🧪 Testing
Backend:

bash
npm run test
Frontend:

bash
npm run test
🧠 Debugging Techniques
Used Chrome DevTools, Node Inspector, Console Logs

Introduced intentional bugs to test resilience

Error boundaries implemented to capture React crashes

✅ Testing Approach
Unit tests for helper logic

Integration tests for REST APIs and UI actions

Mocks to isolate DB and external services

Coverage reports generated using Jest
