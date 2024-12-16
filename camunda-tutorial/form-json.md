The **"Invalid Form Schema"** error in the Camunda SaaS platform typically occurs when the form configuration or schema defined for a User Task is incorrect or incompatible with the expectations of the Camunda engine or Tasklist. Here’s how you can troubleshoot and resolve this issue:

---

### **Steps to Resolve "Invalid Form Schema" Error**

#### 1. **Validate the Form Key**
   - Ensure the `formKey` specified in your BPMN XML is correctly formatted and points to a valid external or embedded form.
   - Example for an **embedded form**:
     ```xml
     <bpmn:userTask id="UserTask_1" name="Example Task">
       <bpmn:extensionElements>
         <zeebe:formDefinition formKey="embedded:deployment:exampleForm.json" />
       </bpmn:extensionElements>
     </bpmn:userTask>   
     ```
   - Example for an **external form**:
     ```xml
     <bpmn:userTask id="UserTask_2" name="External Form Task">
       <bpmn:extensionElements>
         <zeebe:formDefinition formKey="https://example.com/form.html?taskId={taskId}&processInstanceId={processInstanceId}" />
       </bpmn:extensionElements>
     </bpmn:userTask>
     ```

   - If you're using `embedded:deployment`, ensure the form JSON is correctly deployed alongside the process.

---

#### 2. **Verify Form JSON Schema**
   - If you're using a JSON-based form (e.g., embedded form), ensure it adheres to the correct schema structure.
   - Example of a valid **Form JSON** schema:
     ```json
     {
       "components": [
         {
           "key": "employeeName",
           "type": "textfield",
           "label": "Employee Name",
           "validate": {
             "required": true
           }
         },
         {
           "key": "employeeRole",
           "type": "select",
           "label": "Role",
           "data": {
             "values": [
               { "label": "Developer", "value": "developer" },
               { "label": "Manager", "value": "manager" }
             ]
           }
         }
       ]
     }
     ```

---

#### 3. **Check External Form URL**
   - If you're embedding an external form:
     - Verify that the URL is correct and publicly accessible.
     - Ensure the form correctly handles the `taskId` and `processInstanceId` parameters passed in the query string.

---

#### 4. **Deploy the Form Correctly**
   - If using `embedded:deployment`, ensure that the form JSON file is included in the deployment.
   - Deployment example using the CLI:
     ```bash
     zbctl deploy myProcess.bpmn myForm.json
     ```

---

#### 5. **Enable External Form Loading**
   - On the Camunda SaaS platform, ensure that **Tasklist settings** allow loading of external forms. Misconfiguration here can lead to issues.
   - **Where to check**:
     - Log in to the SaaS platform.
     - Go to **Tasklist Configuration**.
     - Ensure that CORS and external access are enabled for the domain hosting the form.

---

#### 6. **Review Logs for Detailed Error Messages**
   - The SaaS platform usually logs more details about the "Invalid Form Schema" error.
   - Check:
     - Form Key syntax.
     - Missing or invalid attributes in the JSON.
     - Deployment issues.

---

#### 7. **Common Mistakes and Fixes**
   - **Mistake 1**: Missing `components` in the JSON.
     - Fix: Ensure the `components` array is present.
   - **Mistake 2**: Unsupported field types or incorrect keys in the JSON.
     - Fix: Use supported field types (`textfield`, `select`, `email`, etc.) and ensure proper `key` usage.
   - **Mistake 3**: Incorrect `formKey` syntax.
     - Fix: Double-check the syntax and ensure proper usage of `embedded:deployment` or an external URL.

---

### **Example Working Scenario**

#### BPMN XML with Form Key
```xml
<bpmn:userTask id="UserTask_1" name="Submit Details">
  <bpmn:extensionElements>
    <zeebe:formDefinition formKey="embedded:deployment:submitForm.json" />
  </bpmn:extensionElements>
  <bpmn:incoming>Flow_1</bpmn:incoming>
  <bpmn:outgoing>Flow_2</bpmn:outgoing>
</bpmn:userTask>
```

#### Form JSON
```json
{
  "components": [
    {
      "key": "name",
      "type": "textfield",
      "label": "Name",
      "validate": {
        "required": true
      }
    },
    {
      "key": "email",
      "type": "email",
      "label": "Email",
      "validate": {
        "required": true
      }
    }
  ]
}
```

#### Deployment Command
```bash
zbctl deploy process.bpmn submitForm.json
```

---

### **Final Checklist**
1. Verify the `formKey` in your BPMN model.
2. Confirm the form JSON schema structure is valid.
3. Ensure the form is deployed correctly if using `embedded:deployment`.
4. Check that external forms have proper CORS and accessibility configurations.
5. Debug using logs for detailed error information.

If the issue persists, share the exact error message or logs for further assistance!