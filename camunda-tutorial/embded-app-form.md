To dynamically load a nested embedded form with dynamically populated form fields in Camunda during multiple `userTask` executions, you can follow these steps:

---

### **1. Understand the Scenario**
- **Objective**: Multiple `userTask`s are executed in sequence, each requiring a dynamically loaded embedded form with fields populated using data retrieved via the Camunda API.
- **Key Components**:
  - **Camunda BPMN Process**: Contains the `userTask`s.
  - **Forms**: Embedded HTML/JS forms to be rendered dynamically.
  - **Camunda REST API**: Used for data retrieval and task operations.
  - **Dynamic Data**: Fetched from external APIs or process variables.

---

### **2. Set Up the Camunda Process**
1. **Design BPMN Diagram**:
   - Include a series of `userTask`s in your process.
   - Configure `userTask` forms to reference an embedded form (e.g., `embedded:app:forms/myDynamicForm.html`).

2. **Configure Process Variables**:
   - Define variables that will store data to be used in the forms (e.g., task-specific details).

---

### **3. Develop the Embedded Form**
Create an embedded form (e.g., `myDynamicForm.html`) with placeholders for dynamic fields.

Example:
```html
<form class="form-horizontal" role="form">
  <div id="dynamic-fields">
    <!-- Dynamic fields will be injected here -->
  </div>
  <button type="submit" class="btn btn-primary" cam-button="complete">Submit</button>
</form>

<script>
  // JavaScript logic to fetch and render form fields dynamically
  const injectDynamicFields = async (taskId) => {
    try {
      // Fetch process variables via Camunda API
      const response = await fetch(`/engine-rest/task/${taskId}/variables`);
      const variables = await response.json();

      // Build form fields dynamically
      const dynamicFields = document.getElementById("dynamic-fields");
      dynamicFields.innerHTML = "";

      Object.keys(variables).forEach((key) => {
        const field = document.createElement("div");
        field.classList.add("form-group");
        field.innerHTML = `
          <label for="${key}">${key}</label>
          <input type="text" id="${key}" name="${key}" value="${variables[key].value}" class="form-control" />
        `;
        dynamicFields.appendChild(field);
      });
    } catch (error) {
      console.error("Error loading dynamic fields:", error);
    }
  };

  // Hook into Camunda form lifecycle
  camForm.on("form-loaded", function () {
    const taskId = camForm.taskId;
    injectDynamicFields(taskId);
  });
</script>
```

---

### **4. Retrieve and Populate Form Data**
Use the Camunda REST API to dynamically fetch task-specific data.

#### **REST API Example**
- **Endpoint**: `/engine-rest/task/{taskId}/variables`
- **Purpose**: Retrieve process variables for the given task.
- **Response**:
  ```json
  {
    "variable1": { "type": "String", "value": "value1" },
    "variable2": { "type": "Integer", "value": 123 }
  }
  ```

---

### **5. Handle Multiple Nested `userTask`s**
1. **Process Logic**:
   - Ensure process variables are carried forward between tasks.
   - Set or update variables in each task based on user input or system logic.

2. **Passing Data**:
   - Use process variables to pass data between tasks (`execution.setVariable()` or API calls).

---

### **6. Submit Form Data**
Upon form submission, Camunda will automatically map form fields back to process variables.

1. **Submit Event**:
   - Use the built-in `cam-button="complete"` attribute to complete the task.

2. **Custom Logic (Optional)**:
   - Attach a JavaScript handler for additional processing before submission.

Example:
```javascript
document.querySelector("form").addEventListener("submit", (event) => {
  // Pre-submit logic here
});
```

---

### **7. Test and Deploy**
1. **Testing**:
   - Deploy the process definition.
   - Trigger the process and verify dynamic form loading for each `userTask`.

2. **Debugging**:
   - Use browser dev tools and Camunda cockpit to debug form rendering and process execution.

3. **Deployment**:
   - Bundle forms into your application and deploy with your Camunda process.

---

### **8. Optional Enhancements**
- Use **external systems** for additional data sources.
- Add **validation** for dynamic fields in the form.
- Enhance the UI with frameworks like **React**, **Angular**, or **Vue** if needed.

---

By following these steps, you can implement a flexible and dynamic user interface for Camunda `userTask`s with embedded forms, allowing seamless interaction with process variables and dynamic content.