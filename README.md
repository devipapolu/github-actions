# github-actions
“GitHub Actions is an automation tool inside GitHub that is used to automate the CI/CD pipeline by defining workflows in YAML files.”

**Breakdown:**
is an automation tool inside GitHub → GitHub lo built-in automation tool.
used to automate the CI/CD pipeline → build, test, deploy automatic cheyadaniki.
by defining workflows in YAML files → .github/workflows/*.yml lo instructions rayadam.

steps:
  - name: Checkout repo
    uses: actions/checkout@v3

  - name: Setup Node.js
    uses: actions/setup-node@v3
    with:
      node-version: '18'

  - name: Install dependencies
    run: npm install

  - name: Run tests
    run: npm test

  - name: Deploy to server
    uses: appleboy/ssh-action@v0.1.7
    with:
      host: ${{ secrets.SERVER_IP }}
      username: ${{ secrets.SERVER_USER }}
      key: ${{ secrets.SERVER_KEY }}
      script: |
        cd /var/www/myproject
        git pull
        npm install
        pm2 restart all


        1️⃣ name:
name: Project Name
Workflow ki name istamu

GitHub Actions UI lo easy ga identify cheyyadaniki

2️⃣ on: (Triggers)
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]


push: Repo ki code push ayye branch select cheyyali
main → main branch
master → master branch (repo ki depend avuthundi)
pull_request: PR create ayye branch
Usually main branch ki PR create ayye appudu run avvadi
Note: Triggers antey workflow start avvadani ki condition.

3️⃣ jobs:
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest


Job = group of steps
Example: build-and-deploy job → build chesi deploy cheyyadam
runs-on: → runner environment (Ubuntu recommended)

Ne workflow step-by-step:
Checkout repo → Code fetch
actions/checkout@v3 use chestamu.

Antey: GitHub repo nundi latest code fetch chesukoni workflow environment lo ready cheyadam.
Analogy: Maven lo source code fetch cheyadam.
Setup environment → Node/Maven/JDK ready
actions/setup-node@v3 (Node.js project)
Maven/Java project lo setup-java@v3 use chestaru.

Antey: Code run cheyyadaniki required environment ready cheyadam.
Install dependencies → npm install / mvn install
Node.js → npm install
Maven → mvn install

Antey: Project ki required libraries/modules install cheyadam.
Run tests → code correct check
Node → npm test
Maven → mvn test

Optional → SonarQube check
Antey: Code quality check, errors & bugs detect cheyadam.
Only tests pass ayithe next step ki move avvadam.

Deploy → AWS/Server
Server details (sensitive info) GitHub Secrets lo store chesam:
Secret Name	Value
SERVER_IP	AWS EC2 Public IP
SERVER_USER	EC2 username (ubuntu)
SERVER_KEY	Private key (.pem content)
d /var/www/myproject → Project folder path (manam AWS lo create chesam)
git pull → Latest code server lo fetch
npm install → Dependencies install
pm2 restart/start → Node process run

4️⃣ steps:
Deploy step lo:
- name: Deploy to AWS EC2
  uses: appleboy/ssh-action@v0.1.7
  with:
    host: ${{ secrets.SERVER_IP }}
    username: ${{ secrets.SERVER_USER }}
    key: ${{ secrets.SERVER_KEY }}
    script: |
      cd /var/www/myproject      # clone chesina folder
      git pull                    # latest code pull
      npm install                 # dependencies update
      pm2 restart all || pm2 start index.js --name myproject
      Step = single task

Example:
Checkout repo → code fetch
Setup Node → environment ready
Install dependencies → npm install
Run tests → code correct check
Deploy → AWS server lo latest code push & run



