### **Making Local Variables Available to a Process Instance Using Java in Camunda**

In **Camunda BPM**, **local variables** are variables that are scoped to a particular activity or task (e.g., a service task, user task). These variables exist only within the context of the task or activity. However, there are times when you want to make these variables available globally to the entire **process instance**. You can achieve this by using the **Java API** to interact with the **runtime variables** in the process instance and **output variables**.

### **Steps to Make Local Variables Available to the Process Instance**

1. **Define Local Variables in a Task**: 
   In BPMN, local variables are defined and used within a specific task, such as a service task or user task.

2. **Use the Java API to Set Variables in the Process Instance**:
   - To transfer the local variable to the global process instance, you can use the **execution API** to set the variable in the process instance scope.

3. **Use Output Variables**:
   - After setting the local variable globally, it can be used as an output variable for the process.

### **Use Case and Scenario**

Let’s consider a **loan approval** process where the local variable is used to hold a **credit score** in a service task. Once the task completes, the process needs to make this **credit score** available globally so that other tasks can use it to make decisions (e.g., whether to approve or reject the loan).

---

### **Scenario: Loan Approval Process**

1. **Process Definition**:  
   The loan approval process involves the following tasks:
   - **Credit Check Task** (Service Task): Checks the credit score and sets a local variable `creditScore`.
   - **Loan Approval Decision Task** (User Task): Makes a decision based on the `creditScore`.
   
2. **Objective**:  
   - The **creditScore** is a local variable in the **Credit Check Task**.
   - Once this task is completed, the **creditScore** needs to be made available globally in the process instance for further decision-making.

---

### **BPMN Process Flow**:

```json
{
  "bpmn:process": {
    "id": "LoanApprovalProcess",
    "name": "Loan Approval Process",
    "bpmn:serviceTask": {
      "id": "CreditCheckTask",
      "name": "Credit Check",
      "implementation": "JavaDelegate",
      "bpmn:localVariable": {
        "name": "creditScore",
        "value": 750
      }
    },
    "bpmn:userTask": {
      "id": "LoanApprovalTask",
      "name": "Loan Approval Decision",
      "bpmn:output": {
        "variable": "creditScore",
        "scope": "processInstance"
      }
    }
  }
}
```

### **Java Implementation for Making Local Variables Global**

1. **Service Task - Credit Check Implementation (JavaDelegate)**

   The `CreditCheckTask` service task needs to calculate the credit score and set it as a **local variable**. After this, we will make it available to the entire process instance using the Java API.

   ```java
   import org.camunda.bpm.engine.delegate.DelegateExecution;
   import org.camunda.bpm.engine.delegate.JavaDelegate;

   public class CreditCheckDelegate implements JavaDelegate {
       @Override
       public void execute(DelegateExecution execution) throws Exception {
           // Simulate credit score check
           int creditScore = 750;

           // Set the local variable 'creditScore' in the execution context (task-specific scope)
           execution.setVariableLocal("creditScore", creditScore);

           // Make the local variable available to the entire process instance
           execution.setVariable("creditScore", creditScore);
       }
   }
   ```

   **Explanation**:
   - **execution.setVariableLocal("creditScore", creditScore)**: This sets the `creditScore` variable locally within the scope of the `CreditCheckTask`.
   - **execution.setVariable("creditScore", creditScore)**: This makes the `creditScore` variable available globally to the entire process instance.

2. **User Task - Loan Approval Decision (JavaDelegate)**

   The `LoanApprovalTask` will use the `creditScore` variable that was set globally in the process instance. Based on the credit score, a decision will be made.

   ```java
   import org.camunda.bpm.engine.delegate.DelegateExecution;
   import org.camunda.bpm.engine.delegate.JavaDelegate;

   public class LoanApprovalDelegate implements JavaDelegate {
       @Override
       public void execute(DelegateExecution execution) throws Exception {
           // Retrieve the global 'creditScore' variable from the process instance
           int creditScore = (int) execution.getVariable("creditScore");

           // Decision logic based on the credit score
           if (creditScore > 700) {
               System.out.println("Loan Approved.");
           } else {
               System.out.println("Loan Rejected.");
           }
       }
   }
   ```

   **Explanation**:
   - **execution.getVariable("creditScore")**: This retrieves the globally available `creditScore` variable from the process instance.
   - The loan approval decision is made based on the `creditScore`.

---

### **Complete BPMN Model with JSON Representation**

Here’s a JSON representation of how the **CreditCheckTask** and **LoanApprovalTask** fit into the overall BPMN model, with the appropriate **service tasks**, **user tasks**, and **output variables**:

```json
{
  "bpmn:process": {
    "id": "LoanApprovalProcess",
    "name": "Loan Approval Process",
    "bpmn:startEvent": {
      "id": "StartEvent",
      "name": "Start Process"
    },
    "bpmn:serviceTask": {
      "id": "CreditCheckTask",
      "name": "Credit Check",
      "implementation": "JavaDelegate",
      "bpmn:localVariable": {
        "name": "creditScore",
        "value": 750
      },
      "bpmn:outgoingFlow": {
        "id": "FlowToLoanApproval",
        "sourceRef": "CreditCheckTask",
        "targetRef": "LoanApprovalTask"
      }
    },
    "bpmn:userTask": {
      "id": "LoanApprovalTask",
      "name": "Loan Approval Decision",
      "bpmn:output": {
        "variable": "creditScore",
        "scope": "processInstance"
      },
      "bpmn:incomingFlow": {
        "id": "FlowToLoanApproval",
        "sourceRef": "CreditCheckTask",
        "targetRef": "LoanApprovalTask"
      }
    },
    "bpmn:endEvent": {
      "id": "EndEvent",
      "name": "End Process"
    }
  }
}
```

---

### **Explanation of the BPMN Process JSON**:

1. **Start Event**: The process begins with the **Start Event**.
2. **CreditCheckTask** (Service Task):
   - This task calculates the credit score and sets the **local variable** `creditScore` using the `setVariableLocal` method.
   - The credit score is then made available globally to the process instance with `setVariable`.
3. **LoanApprovalTask** (User Task):
   - This task retrieves the global **`creditScore`** and makes the approval decision.
   - The **output variable** is set globally in the process instance using `bpmn:output`.
4. **End Event**: The process ends with the **End Event**.

---

### **Conclusion**

By using **Java delegates** in Camunda, you can make **local variables** available globally to the **process instance**. This is useful when you need to propagate data computed or gathered during a task to subsequent tasks or decisions in the workflow. In the **Loan Approval** scenario, the **credit score** is initially calculated as a local variable in the **CreditCheckTask**, and then it is made globally available for decision-making in the **LoanApprovalTask**.