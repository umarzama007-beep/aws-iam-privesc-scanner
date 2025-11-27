-> AWS IAM Privilege Escalation Scanner

A Python-based tool that detects multi-step IAM privilege-escalation paths inside an AWS account using Boto3, NetworkX, and Matplotlib.
The scanner automatically maps all users, roles, policies, and sts:AssumeRole relationships to reveal any hidden escalation chains.


-> What this project does

Reads IAM users, roles, and policies from an AWS account

Detects where a user or role can AssumeRole → another role

Builds a directed graph of all IAM trust & policy relationships

Highlights privilege-escalation paths, e.g.:

student-user → LibraryAssistantRole → ... → AdminRole

Generates a visual graph (PNG) showing the final escalation chain

This helps administrators and security teams identify misconfigurations that allow attackers to escalate privileges.

 
 -> How the AWS IAM environment was built for this project

To simulate a realistic cloud security scenario, a multi-step privilege escalation structure was intentionally created inside AWS.

1. Starting identity (low privilege)

student-user

No administrative permissions

Only allowed to assume one basic role at the start of the chain

2. Custom multi-hop IAM roles created

Each role was configured so that the previous identity in the chain could assume it.

The roles created were:

LibraryAssistantRole

CourseAssistantRole

DeptAdminRole

ITSupportRole

SecurityOfficerRole

AdminRole (final admin role with AdministratorAccess)

3. Two things configured for each hop

For every step in the chain:

(A) Trust Policy on the target role

Example: CourseAssistantRole trusts LibraryAssistantRole

This tells AWS which identity is allowed to assume this role

(B) Inline AssumeRole Policy on the caller

Example: LibraryAssistantRole is allowed to assume CourseAssistantRole

This gives the previous role permission to make the switch


-> Why this project is useful

Privilege escalation in IAM is often multi-hop and hidden deep across trust policies.
Audit teams normally inspect IAM manually — and miss chained paths.

- This project provides:

Automatic detection of multi-step IAM ladders

Visual evidence for reports, audits, and assignments

Reproducible demonstrations for universities & cybersecurity coursework

A safe way to analyze IAM without modifying anything in the cloud

- Useful for:

Cloud security learning

Red team simulation

University security projects

Portfolio / GitHub showcase

Understanding IAM at a deeper level


-> How to get started

1. Clone the repository

sh
git clone https://github.com/umarzama007-beep/aws-iam-privesc-scanner.git
cd aws-iam-privesc-scanner

2. Open the Notebook

Open the file in Google Colab (recommended):

aws_iam_privesc_scanner.ipynb

3. Install required libraries

If needed:

python
!pip install boto3 networkx matplotlib jmespath

4. Add AWS scanner credentials

In Colab:

python
import os 
os.environ["AWS_ACCESS_KEY_ID"] = "YOUR_KEY"
os.environ["AWS_SECRET_ACCESS_KEY"] = "YOUR_SECRET"
os.environ["AWS_DEFAULT_REGION"] = "us-east-1"

Never commit real AWS keys to GitHub.

5. Run all cells

The notebook will:

Connect to AWS

Read IAM configuration

Build the graph

Find escalation paths

Generate iam_privesc_graph.png


-> Where to get help

- If you need support:

Open an Issue in this GitHub repository

Read AWS IAM documentation (Trust Policies / AssumeRole)

Search “AWS IAM privilege escalation vectors”

Ask your project supervisor or teaching assistant


-> Maintainer & contributor

Umar Zama

GitHub: @umarzama007-beep

Email: umarzama007@gmail.com


-> Project structure

aws-iam-privesc-scanner/
│
├── aws_iam_privesc_scanner.ipynb     # Main Colab notebook
├── iam_privesc_graph.png             # Output graph
├── README.md                         # Documentation
├── LICENSE                           # MIT License
└── .gitignore                        # (Python) Ignore unnecessary files
