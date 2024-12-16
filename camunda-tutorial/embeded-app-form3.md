The issue likely lies in how Camunda handles the expressions `{{ execution.processInstanceId }}` and `{{ task.id }}` in embedded forms. Camunda doesn't resolve these expressions automatically in external or embedded JSON forms for user tasks. These placeholders are specific to templated forms or scripting in certain contexts but not directly in JSON-based embedded forms.

Here’s how you can ensure these values appear in an embedded form:

---

### **Steps to Display Process Instance ID and Task ID in Embedded Forms**

1. **Pass Variables to the Embedded Form**  
   Camunda passes context variables (like `processInstanceId` and `task.id`) to embedded forms via the `tasklist` API. You need to explicitly read and populate them in the form.

2. **Modify the Embedded Form Logic**  
   Use JavaScript to fetch the `task.id` and `processInstanceId` dynamically from the parent context and populate the form fields.

---

### **Example JSON-Based Embedded Form**

```html
<!DOCTYPE html>
<html>
<head>
  <title>Embedded Form</title>
  <script>
    // Load process instance ID and task ID dynamically
    window.addEventListener('message', function (event) {
      const { task } = event.data;

      if (task) {
        // Populate form fields with task.id and processInstanceId
        document.getElementById('processInstanceId').value = task.processInstanceId || 'N/A';
        document.getElementById('taskId').value = task.id || 'N/A';
      }
    });
  </script>
</head>
<body>
  <form id="embeddedForm">
    <div>
      <label for="processInstanceId">Process Instance ID:</label>
      <input
        type="text"
        id="processInstanceId"
        name="processInstanceId"
        readonly
      />
    </div>
    <div>
      <label for="taskId">Task ID:</label>
      <input
        type="text"
        id="taskId"
        name="taskId"
        readonly
      />
    </div>
  </form>
</body>
</html>
```

---

### **Explanation**

1. **`window.addEventListener`**:
   - The form listens for messages from the Camunda Tasklist.
   - The `event.data.task` object contains `task.id` and `processInstanceId`.

2. **Dynamic Population**:
   - The `task.id` and `task.processInstanceId` values are set into the form fields dynamically.

3. **Read-Only Fields**:
   - Both fields are marked as `readonly` to prevent user modification.

---

### **Adding the Form to the User Task**

- Update the BPMN model to reference this embedded form:

```xml
<bpmn:userTask id="UserTask_1" name="User Task">
  <bpmn:extensionElements>
    <zeebe:formDefinition formKey="embedded:app:forms/process-info-form.html" />
  </bpmn:extensionElements>
  <bpmn:incoming>Flow_1</bpmn:incoming>
  <bpmn:outgoing>Flow_2</bpmn:outgoing>
</bpmn:userTask>
```

---

### **Result**

- When the user task is opened in the Camunda Tasklist, the embedded form displays the `Process Instance ID` and `Task ID` in the read-only fields.
- These values are dynamically populated from the task context.

---

Let me know if you encounter issues with this approach!