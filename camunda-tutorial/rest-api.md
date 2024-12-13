https://docs.camunda.io/docs/apis-tools/camunda-api-rest/camunda-api-rest-overview/

https://docs.camunda.io/docs/apis-tools/frontend-development/task-applications/user-task-lifecycle/

https://docs.camunda.io/docs/apis-tools/frontend-development/forms/embed-forms/embed-forms-in-javascript/


Camunda offers a variety of APIs to interact with tasks, processes, forms, and other components of the workflow engine. Below is a categorized list of important Camunda APIs for common operations, including loading form schemas for a user task and more.

---

### **1. Task Management APIs**
#### **Get a List of Tasks**
- **Endpoint**: `GET /task`
- **Description**: Retrieve a list of tasks.
- **Example**: Filter tasks by process instance or assignee.

#### **Get a Single Task**
- **Endpoint**: `GET /task/{taskId}`
- **Description**: Retrieve details of a specific task by its ID.

#### **Claim a Task**
- **Endpoint**: `POST /task/{taskId}/claim`
- **Description**: Assign a task to a specific user.

#### **Complete a Task**
- **Endpoint**: `POST /task/{taskId}/complete`
- **Description**: Mark a task as complete and optionally update process variables.

#### **Unclaim a Task**
- **Endpoint**: `POST /task/{taskId}/unclaim`
- **Description**: Remove the assignment of a task.

#### **Delegate a Task**
- **Endpoint**: `POST /task/{taskId}/delegate`
- **Description**: Delegate a task to another user.

---

### **2. Form Management APIs**
#### **Get Task Form Schema**
- **Endpoint**: `GET /task/{taskId}/form`
- **Description**: Retrieve the form schema for a specific user task.

#### **Submit Task Form**
- **Endpoint**: `POST /task/{taskId}/submit-form`
- **Description**: Submit form data for a task and complete it.

#### **Get Deployed Start Form**
- **Endpoint**: `GET /process-definition/{id}/startForm`
- **Description**: Retrieve the start form schema of a process.

#### **Submit Start Form**
- **Endpoint**: `POST /process-definition/{id}/submit-form`
- **Description**: Start a process instance by submitting its start form.

---

### **3. Variable Management APIs**
#### **Get Task Variables**
- **Endpoint**: `GET /task/{taskId}/variables`
- **Description**: Retrieve variables associated with a task.

#### **Update Task Variables**
- **Endpoint**: `PUT /task/{taskId}/variables`
- **Description**: Update or add variables for a task.

#### **Get Process Instance Variables**
- **Endpoint**: `GET /process-instance/{id}/variables`
- **Description**: Retrieve variables for a specific process instance.

#### **Update Process Instance Variables**
- **Endpoint**: `PUT /process-instance/{id}/variables`
- **Description**: Update or add variables for a process instance.

---

### **4. Process Instance Management APIs**
#### **Start a Process Instance**
- **Endpoint**: `POST /process-definition/{id}/start`
- **Description**: Start a process instance.

#### **Get Process Instance Details**
- **Endpoint**: `GET /process-instance/{id}`
- **Description**: Retrieve details about a specific process instance.

#### **Delete a Process Instance**
- **Endpoint**: `DELETE /process-instance/{id}`
- **Description**: Delete a process instance by ID.

---

### **5. Deployment APIs**
#### **Deploy a BPMN Model**
- **Endpoint**: `POST /deployment/create`
- **Description**: Deploy a BPMN model to the engine.

#### **Get Deployed Definitions**
- **Endpoint**: `GET /process-definition`
- **Description**: Retrieve a list of deployed process definitions.

---

### **6. Incident and Job Management APIs**
#### **Get Incidents**
- **Endpoint**: `GET /incident`
- **Description**: Retrieve a list of incidents.

#### **Set a Job to Retrigger**
- **Endpoint**: `POST /job/{jobId}/retries`
- **Description**: Set the number of retries for a job.

---

### **7. Historic Data APIs**
#### **Get Historic Task Instances**
- **Endpoint**: `GET /history/task`
- **Description**: Retrieve historic data for tasks.

#### **Get Historic Process Instances**
- **Endpoint**: `GET /history/process-instance`
- **Description**: Retrieve historic data for process instances.

#### **Get Historic Variable Instances**
- **Endpoint**: `GET /history/variable-instance`
- **Description**: Retrieve historic data for variables.

---

### **8. External Task APIs**
#### **Fetch and Lock External Tasks**
- **Endpoint**: `POST /external-task/fetchAndLock`
- **Description**: Fetch and lock external tasks for worker processing.

#### **Complete External Task**
- **Endpoint**: `POST /external-task/{id}/complete`
- **Description**: Mark an external task as complete.

#### **Report External Task Failure**
- **Endpoint**: `POST /external-task/{id}/failure`
- **Description**: Report a failure for an external task.

---

### **9. User and Authorization Management APIs**
#### **Get Users**
- **Endpoint**: `GET /user`
- **Description**: Retrieve a list of users.

#### **Create User**
- **Endpoint**: `POST /user/create`
- **Description**: Create a new user.

#### **Get User Authorization**
- **Endpoint**: `GET /authorization`
- **Description**: Retrieve authorization details for users.

---

### **10. Decision Management APIs**
#### **Evaluate a Decision**
- **Endpoint**: `POST /decision-definition/{id}/evaluate`
- **Description**: Evaluate a decision (DMN model).

#### **Get Decision Table Definitions**
- **Endpoint**: `GET /decision-definition`
- **Description**: Retrieve deployed decision table definitions.

---

### **11. Event Subscription APIs**
#### **Get Event Subscriptions**
- **Endpoint**: `GET /event-subscription`
- **Description**: Retrieve event subscriptions in the system.

---

### **12. Signal and Message APIs**
#### **Send a Signal**
- **Endpoint**: `POST /signal`
- **Description**: Trigger a signal event.

#### **Correlate a Message**
- **Endpoint**: `POST /message`
- **Description**: Correlate a message to an event in the process.

---

### **Documentation Reference**
For detailed information on request/response formats, headers, and payloads, refer to the official [Camunda REST API Documentation](https://docs.camunda.org/manual/latest/reference/rest/).