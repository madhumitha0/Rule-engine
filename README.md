# Rule-engine
This project is a 3-tier rule engine application using Flask, SQLAlchemy, and SQLite. It supports creating, combining, and evaluating rules through Abstract Syntax Trees (ASTs).

Setup
Install Dependencies:

bash
Copy code
pip install flask sqlalchemy
Run the Application:

bash
Copy code
python main.py
Components
Backend: main.py (Flask, SQLAlchemy, SQLite)
Frontend: rlg.py (Tkinter UI)
Automated Testing: test.py (Automates app testing with requests)
App Features
Create Rule: Creates a rule and assigns it an ID (e.g., Rule 1 and Rule 2).
Combine Rules: Combines rule IDs (comma-separated) into a "mega rule" with a unique ID in the Tkinter UI.
Evaluate Rule: Evaluates a rule using the mega rule ID and data in JSON format.
API Endpoints
Create Rule

URL: /create_rule
Method: POST
Data: JSON { "rule_string": "(age > 30 AND department = 'Sales') OR (salary > 50000)" }
Response: { "id": 1, "ast": "..." }
Combine Rules

URL: /combine_rules
Method: POST
Data: JSON { "rule_ids": [1, 2] }
Response: { "id": 3, "combined_ast": "..." }
Evaluate Rule

URL: /evaluate_rule
Method: POST
Data: JSON { "rule_id": 3, "data": { "age": 35, "department": "Sales", "salary": 60000, "experience": 6 } }
Response: { "result": true }
Modify Rule

URL: /modify_rule
Method: POST
Data: JSON { "rule_id": 1, "new_rule_string": "age > 40 AND department = 'HR'" }
Response: { "message": "Rule updated successfully" }
Sample curl Workflow
Create Rule 1:

bash
Copy code
curl -X POST http://127.0.0.1:5000/create_rule -H "Content-Type: application/json" -d '{"rule_string": "(age > 30 AND department = \"Sales\") OR (salary > 50000)"}'
Create Rule 2:

bash
Copy code
curl -X POST http://127.0.0.1:5000/create_rule -H "Content-Type: application/json" -d '{"rule_string": "experience > 5 AND department = \"Marketing\""}'
Combine Rules (use actual rule IDs from previous steps):

bash
Copy code
curl -X POST http://127.0.0.1:5000/combine_rules -H "Content-Type: application/json" -d '{"rule_ids": [1, 2]}'
Evaluate Combined Rule:

bash
Copy code
curl -X POST http://127.0.0.1:5000/evaluate_rule -H "Content-Type: application/json" -d '{ "rule_id": 3, "data": { "age": 35, "department": "Sales", "salary": 60000, "experience": 6 } }'
Modify Rule (use actual rule ID to modify):

bash
Copy code
curl -X POST http://127.0.0.1:5000/modify_rule -H "Content-Type: application/json" -d '{ "rule_id": 1, "new_rule_string": "age > 40 AND department = \"HR\"" }'
Testing
Use test.py to automate testing. Run it with:

bash
Copy code
python test.py
test.py Script Overview
python
Copy code
import requests

BASE_URL = "http://127.0.0.1:5000"

def test_create_rule(rule_string):
    response = requests.post(f"{BASE_URL}/create_rule", json={"rule_string": rule_string})
    print("Create Rule Response:", response.json())
    return response.json()['id']

def test_combine_rules(rule_id_1, rule_id_2):
    response = requests.post(f"{BASE_URL}/combine_rules", json={"rule_ids": [rule_id_1, rule_id_2]})
    print("Combine Rules Response:", response.json())
    return response.json()['id']

def test_evaluate_rule(rule_id, data):
    response = requests.post(f"{BASE_URL}/evaluate_rule", json={"rule_id": rule_id, "data": data})
    print("Evaluate Rule Response:", response.json())

def test_modify_rule(rule_id, new_rule_string):
    response = requests.post(f"{BASE_URL}/modify_rule", json={"rule_id": rule_id, "new_rule_string": new_rule_string})
    print("Modify Rule Response:", response.json())

if __name__ == "__main__":
    rule_id_1 = test_create_rule("(age > 30 AND department = 'Sales')")
    rule_id_2 = test_create_rule("(salary > 50000 OR experience > 5)")
    combined_rule_id = test_combine_rules(rule_id_1, rule_id_2)
    test_evaluate_rule(combined_rule_id, {"age": 35, "department": "Sales", "salary": 60000, "experience": 6})
    test_modify_rule(rule_id_1, "age > 40 AND department = 'HR'")
Summary: This guide covers setup, running, and testing for a rule engine application with ASTs. The test.py script automates testing to verify functionality. Follow these instructions to ensure the rule engine works correctly.
