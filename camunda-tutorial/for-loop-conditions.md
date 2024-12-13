### **For-Loop with If Condition Expression in Camunda BPMN Using JSON Structure**

In BPMN, a **for-loop** combined with an **if condition** can be implemented using a **multi-instance task** or **multi-instance subprocess** along with **expression conditions**. Since BPMN itself doesn't support direct for-loop constructs like programming languages, this behavior can be achieved using multi-instance activities and gateways to simulate a loop with conditions.

In this explanation, we’ll use JSON-like structure examples to demonstrate how to implement this logic in Camunda BPMN.

---

### **Understanding the Process:**
- **Collection**: A collection (list) of items is provided to iterate over (e.g., users, orders).
- **Element Variable**: For each iteration, the current item is captured in an **element variable** (e.g., `user`, `order`).
- **Condition**: An **if condition** is applied to each item to decide whether to continue with a certain operation or skip to the next iteration.

---

### **JSON Structure for BPMN Elements**

The JSON structure below will show how a **multi-instance task** can be defined with a **conditional check**.

---

### **Scenario 1: Send Notifications to Users Based on Age (Only Users Above 18)**

**Scenario**:  
A list of users is provided. You need to send a notification only to users aged 18 or above. If the user’s age is below 18, they should not receive the notification.

#### **Steps**:
1. Retrieve the list of users (`userList`), each with a `name` and `age`.
2. Iterate through the `userList`.
3. For each user, check if the age is greater than or equal to 18.
4. If true, send a notification; if false, skip that user.

#### **JSON Representation for BPMN Process**

```json
{
  "bpmn:process": {
    "id": "SendNotificationProcess",
    "name": "Send Notifications",
    "bpmn:serviceTask": {
      "id": "SendNotification",
      "name": "Send Notification",
      "bpmn:multiInstanceLoopCharacteristics": {
        "isSequential": true,
        "loopCardinality": "${userList.size()}",
        "inputDataItem": "user"
      },
      "bpmn:extensionElements": {
        "camunda:class": "com.example.SendNotificationDelegate"
      },
      "bpmn:scriptTask": {
        "id": "CheckAge",
        "name": "Check Age",
        "scriptFormat": "javascript",
        "script": "var user = execution.getVariable('user'); if (user.age >= 18) { execution.setVariable('sendNotification', true); } else { execution.setVariable('sendNotification', false); }"
      },
      "bpmn:sequenceFlow": [
        {
          "id": "Flow1",
          "sourceRef": "SendNotification",
          "targetRef": "CheckAge"
        },
        {
          "id": "Flow2",
          "sourceRef": "CheckAge",
          "targetRef": "SendEmailTask",
          "conditionExpression": "${sendNotification == true}"
        }
      ],
      "bpmn:userTask": {
        "id": "SendEmailTask",
        "name": "Send Email",
        "bpmn:extensionElements": {
          "camunda:class": "com.example.SendEmailDelegate"
        }
      }
    }
  }
}
```

#### **Explanation**:
- **Collection**: `userList` (contains a list of users).
- **Element Variable**: `user` (each iteration represents a user).
- **Loop Cardinality**: The size of the `userList` determines the number of iterations.
- **Condition**: The **if condition** checks whether the `user.age >= 18`. If true, the variable `sendNotification` is set to `true`, and the user will receive an email.

---

### **Scenario 2: Process Orders and Apply Discounts Based on Order Total**

**Scenario**:  
An e-commerce system processes a list of orders. If the order total is greater than $100, a discount is applied; otherwise, the order is processed without a discount.

#### **Steps**:
1. Retrieve the list of orders (`orderList`).
2. Iterate through the `orderList`.
3. For each order, check if the `order.total` is greater than $100.
4. If true, apply a discount to the order; if false, skip the discount.

#### **JSON Representation for BPMN Process**

```json
{
  "bpmn:process": {
    "id": "ProcessOrders",
    "name": "Process Orders",
    "bpmn:serviceTask": {
      "id": "ProcessOrder",
      "name": "Process Order",
      "bpmn:multiInstanceLoopCharacteristics": {
        "isSequential": false,
        "loopCardinality": "${orderList.size()}",
        "inputDataItem": "order"
      },
      "bpmn:scriptTask": {
        "id": "ApplyDiscount",
        "name": "Apply Discount",
        "scriptFormat": "javascript",
        "script": "var order = execution.getVariable('order'); if (order.total > 100) { order.total = order.total - (order.total * 0.1); execution.setVariable('updatedOrder', order); }"
      },
      "bpmn:sequenceFlow": [
        {
          "id": "Flow1",
          "sourceRef": "ProcessOrder",
          "targetRef": "ApplyDiscount"
        },
        {
          "id": "Flow2",
          "sourceRef": "ApplyDiscount",
          "targetRef": "OrderProcessed",
          "conditionExpression": "${updatedOrder != null}"
        }
      ],
      "bpmn:endEvent": {
        "id": "OrderProcessed",
        "name": "Order Processed"
      }
    }
  }
}
```

#### **Explanation**:
- **Collection**: `orderList` (a list of orders).
- **Element Variable**: `order` (each iteration represents a specific order).
- **Loop Cardinality**: The size of the `orderList` determines the number of iterations.
- **Condition**: The **if condition** checks whether `order.total > 100`. If true, a discount is applied (10% discount).

---

### **Scenario 3: Approving Purchase Orders with Different Approval Levels**

**Scenario**:  
A purchase order needs to be approved by multiple stakeholders, but only if the order amount is above a certain threshold (e.g., $5000). The workflow should only proceed if all necessary approvals are obtained.

#### **Steps**:
1. Retrieve a list of purchase orders (`purchaseOrdersList`).
2. For each purchase order, check if the order amount is above $5000.
3. If true, use a multi-instance task for each stakeholder (approver) to approve the purchase.
4. Only proceed if all approvals are received.

#### **JSON Representation for BPMN Process**

```json
{
  "bpmn:process": {
    "id": "ApprovePurchaseOrders",
    "name": "Approve Purchase Orders",
    "bpmn:serviceTask": {
      "id": "ApproveOrder",
      "name": "Approve Purchase Order",
      "bpmn:multiInstanceLoopCharacteristics": {
        "isSequential": true,
        "loopCardinality": "${purchaseOrdersList.size()}",
        "inputDataItem": "purchaseOrder"
      },
      "bpmn:scriptTask": {
        "id": "CheckAmount",
        "name": "Check Order Amount",
        "scriptFormat": "javascript",
        "script": "var purchaseOrder = execution.getVariable('purchaseOrder'); if (purchaseOrder.amount > 5000) { execution.setVariable('eligibleForApproval', true); } else { execution.setVariable('eligibleForApproval', false); }"
      },
      "bpmn:sequenceFlow": [
        {
          "id": "Flow1",
          "sourceRef": "ApproveOrder",
          "targetRef": "CheckAmount"
        },
        {
          "id": "Flow2",
          "sourceRef": "CheckAmount",
          "targetRef": "ApprovalTask",
          "conditionExpression": "${eligibleForApproval == true}"
        }
      ],
      "bpmn:userTask": {
        "id": "ApprovalTask",
        "name": "Approve Order",
        "bpmn:extensionElements": {
          "camunda:class": "com.example.ApproveOrderDelegate"
        }
      },
      "bpmn:endEvent": {
        "id": "OrderApproved",
        "name": "Order Approved"
      }
    }
  }
}
```

#### **Explanation**:
- **Collection**: `purchaseOrdersList` (a list of purchase orders).
- **Element Variable**: `purchaseOrder` (each iteration represents a purchase order).
- **Loop Cardinality**: The size of the `purchaseOrdersList` determines the number of iterations.
- **Condition**: The **if condition** checks whether `purchaseOrder.amount > 5000`. If true, it sets `eligibleForApproval` to `true`, allowing the approval process to proceed.

---

### **Conclusion**

In each of these scenarios, we use a **multi-instance loop** to iterate over a collection of items and apply an **if condition** to decide whether to continue with an action or skip it. The condition is typically evaluated using a **script task** or **gateway** in the BPMN model. 

By using **JSON-like structure** for the BPMN process definition, we simulate conditional loops and ensure dynamic decision-making within workflows, enabling more flexible process automation in Camunda.