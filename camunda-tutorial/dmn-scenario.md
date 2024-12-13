### **Camunda DMN Features**

**Decision Model and Notation (DMN)** in Camunda is a powerful tool for modeling, managing, and automating business decision-making processes. It allows organizations to separate business rules from the workflow logic, making processes more flexible and easier to manage. Here are the core features of **Camunda DMN**, with relevant use cases, scenarios, and examples using JSON structures.

---

### **1. Decision Tables**

**Feature**:  
A **Decision Table** defines a set of rules that can be applied to make decisions based on input values. Each row represents a rule, and the columns represent the conditions and outcomes.

**Use Case**:  
- **Scenario**: In an **Insurance Claims Approval Process**, a decision table could be used to determine whether a claim is approved or denied based on the claim amount, type of insurance, and policy conditions.

**Example JSON**:
```json
{
  "decisionTable": {
    "id": "claimApproval",
    "input": [
      {
        "name": "Claim Amount",
        "type": "number"
      },
      {
        "name": "Insurance Type",
        "type": "string"
      }
    ],
    "output": {
      "name": "Approval Status",
      "type": "string"
    },
    "rules": [
      {
        "inputs": [5000, "Health"],
        "output": "Approved"
      },
      {
        "inputs": [10000, "Health"],
        "output": "Pending"
      },
      {
        "inputs": [5000, "Car"],
        "output": "Denied"
      }
    ]
  }
}
```

**Explanation**:  
In this **Claim Approval** decision table:
- The **input** fields are "Claim Amount" and "Insurance Type."
- The **output** field is "Approval Status."
- The **rules** specify conditions for different claim scenarios, determining if a claim should be approved, pending, or denied.

---

### **2. Decision Requirements Diagram (DRD)**

**Feature**:  
The **Decision Requirements Diagram (DRD)** provides a high-level view of how multiple decision tables and decisions are related to each other. It shows the relationships between decisions and input data.

**Use Case**:  
- **Scenario**: In a **Loan Approval System**, you could use a DRD to show how different decisions (e.g., credit score check, debt-to-income ratio check) feed into a final loan approval decision.

**Example JSON**:
```json
{
  "decisionRequirementsDiagram": {
    "id": "loanApprovalDRD",
    "decisions": [
      {
        "id": "creditScoreCheck",
        "type": "decisionTable"
      },
      {
        "id": "debtToIncomeCheck",
        "type": "decisionTable"
      },
      {
        "id": "finalLoanApproval",
        "type": "decision"
      }
    ],
    "relationships": [
      {
        "from": "creditScoreCheck",
        "to": "finalLoanApproval"
      },
      {
        "from": "debtToIncomeCheck",
        "to": "finalLoanApproval"
      }
    ]
  }
}
```

**Explanation**:  
In this **Loan Approval DRD**:
- There are three decisions: **creditScoreCheck**, **debtToIncomeCheck**, and **finalLoanApproval**.
- The **creditScoreCheck** and **debtToIncomeCheck** decisions feed into the **finalLoanApproval** decision.

---

### **3. Input and Output Variables**

**Feature**:  
DMN allows the definition of **input** and **output** variables for decisions, which can be used to parameterize the decision logic.

**Use Case**:  
- **Scenario**: In a **Tax Calculation** system, the decision model can take input variables like income and tax deductions to calculate the tax amount.

**Example JSON**:
```json
{
  "decisionTable": {
    "id": "taxCalculation",
    "input": [
      {
        "name": "Income",
        "type": "number"
      },
      {
        "name": "Tax Deductions",
        "type": "number"
      }
    ],
    "output": {
      "name": "Tax Amount",
      "type": "number"
    },
    "rules": [
      {
        "inputs": [50000, 1000],
        "output": 9000
      },
      {
        "inputs": [70000, 2000],
        "output": 12000
      }
    ]
  }
}
```

**Explanation**:  
In this **Tax Calculation** decision table:
- The **input** fields are "Income" and "Tax Deductions."
- The **output** field is "Tax Amount."
- The rules specify tax calculations based on different income and deduction scenarios.

---

### **4. Decision Expressions (FEEL)**

**Feature**:  
**FEEL (Friendly Enough Expression Language)** is the language used in Camunda DMN to write expressions in decision tables and decision models. FEEL expressions enable flexible and dynamic decision logic.

**Use Case**:  
- **Scenario**: In a **Discount Eligibility** system, you can use FEEL expressions to determine if a customer is eligible for a discount based on their purchase amount and loyalty status.

**Example JSON**:
```json
{
  "decisionTable": {
    "id": "discountEligibility",
    "input": [
      {
        "name": "Purchase Amount",
        "type": "number"
      },
      {
        "name": "Loyalty Status",
        "type": "string"
      }
    ],
    "output": {
      "name": "Discount Percentage",
      "type": "number"
    },
    "rules": [
      {
        "inputs": [500, "Gold"],
        "output": 20
      },
      {
        "inputs": [300, "Silver"],
        "output": 10
      }
    ]
  }
}
```

**Explanation**:  
In this **Discount Eligibility** decision table:
- The **input** fields are "Purchase Amount" and "Loyalty Status."
- The **output** field is "Discount Percentage."
- The rules apply different discount rates based on the customer’s purchase amount and loyalty status.

---

### **5. Decision Services**

**Feature**:  
A **Decision Service** allows multiple decisions to be executed as part of a **BPMN process**, making it easy to integrate decision-making logic into business processes.

**Use Case**:  
- **Scenario**: In an **Invoice Approval System**, a decision service can be used to check the invoice amount, vendor type, and compliance before deciding whether to approve the invoice.

**Example JSON**:
```json
{
  "decisionService": {
    "id": "invoiceApprovalService",
    "decisions": [
      {
        "id": "invoiceAmountCheck",
        "type": "decisionTable"
      },
      {
        "id": "vendorComplianceCheck",
        "type": "decisionTable"
      },
      {
        "id": "finalApproval",
        "type": "decision"
      }
    ]
  }
}
```

**Explanation**:  
In this **Invoice Approval Decision Service**:
- It aggregates decisions for **invoiceAmountCheck**, **vendorComplianceCheck**, and **finalApproval** into a decision service.
- This decision service can be invoked during an invoice approval process in a BPMN workflow.

---

### **6. Decision Execution in BPMN**

**Feature**:  
DMN models can be embedded into **BPMN processes** to execute decisions automatically at certain points in the workflow.

**Use Case**:  
- **Scenario**: In a **Loan Application Process**, the decision to approve or reject the loan can be modeled using a DMN decision table and executed within the BPMN process after the applicant’s data is collected.

**Example JSON**:
```json
{
  "bpmnProcess": {
    "id": "loanApplicationProcess",
    "tasks": [
      {
        "id": "collectApplicationData",
        "type": "userTask"
      },
      {
        "id": "approveLoan",
        "type": "serviceTask",
        "decisionService": "loanApprovalService"
      }
    ]
  }
}
```

**Explanation**:  
In this **Loan Application Process**:
- The first task is to collect application data.
- The second task is a service task that uses the **loanApprovalService** (which includes decision logic to approve or reject the loan).

---

### **7. Versioning and Rule Management**

**Feature**:  
DMN models support **versioning**, which allows users to manage and deploy different versions of decision models over time.

**Use Case**:  
- **Scenario**: In a **Customer Credit Scoring System**, as business rules evolve, new versions of the decision table can be deployed to ensure the credit scoring system stays up to date.

**Example JSON**:
```json
{
  "decisionTable": {
    "id": "creditScoreCheck",
    "version": "v2",
    "input": [
      {
        "name": "Credit Score",
        "type": "number"
      }
    ],
    "output": {
      "name": "Credit Status",
      "type": "string"
    },
    "rules": [
      {
        "inputs": [750],
        "output": "Approved"
      },
      {
        "inputs": [600],
        "output": "Pending"
      }
    ]
  }
}
```

**Explanation**:  
In this **Credit Score Check** decision table, version "v2" is deployed, reflecting the latest rule changes for determining credit approval.

---

### **Conclusion**

**Camunda DMN** provides a robust framework for decision automation in business processes. By separating decision logic from the workflow, DMN enhances process flexibility, enables rule versioning, and allows for better transparency in decision-making. The features mentioned, along with use cases and scenarios, showcase the power of DMN in automating decisions in various business domains.