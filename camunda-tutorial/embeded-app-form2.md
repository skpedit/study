To enable or disable the **Complete Task** button in a Camunda user task based on form validation in an embedded form, you can implement a combination of client-side JavaScript logic in the form and Camunda's Tasklist behavior.

Here's a detailed explanation:

---

### **Approach**

1. **Embed a Form with Validation Logic**  
   Use an external or embedded form and include JavaScript validation logic to manage the state of the **Complete Task** button.

2. **Leverage Camunda Form Lifecycle Events**  
   The embedded form can interact with the Camunda Tasklist API to validate fields and enable/disable the button.

3. **Submit Form Data via Camunda Tasklist**  
   Ensure the form only submits valid data when the **Complete Task** button is enabled.

---

### **Example Implementation**

#### **1. Create an Embedded Form**

```html
<!DOCTYPE html>
<html>
<head>
  <title>Task Form</title>
  <script>
    // Function to validate the form fields
    function validateForm() {
      const inputField = document.getElementById('inputField');
      const completeButton = document.getElementById('completeTaskButton');

      // Enable or disable the button based on validation
      if (inputField.value.trim() !== '') {
        completeButton.disabled = false;
      } else {
        completeButton.disabled = true;
      }
    }

    // Function to submit the form to Camunda
    function submitForm() {
      const inputField = document.getElementById('inputField').value;

      // Access the Camunda Tasklist API context
      window.parent.postMessage(
        {
          type: 'camForm.submit',
          variables: {
            inputValue: { value: inputField }
          }
        },
        '*'
      );
    }
  </script>
</head>
<body>
  <form id="taskForm">
    <label for="inputField">Enter Value:</label>
    <input 
      type="text" 
      id="inputField" 
      name="inputField" 
      oninput="validateForm()" 
    />
    <br />
    <button 
      type="button" 
      id="completeTaskButton" 
      disabled 
      onclick="submitForm()">
      Complete Task
    </button>
  </form>
</body>
</html>
```

---

#### **2. Explanation**

- **Field Validation (`validateForm`)**:  
  This function checks if the input field has a value and enables or disables the **Complete Task** button accordingly.

- **Camunda Form Submission (`submitForm`)**:  
  The `postMessage` API sends the form data back to the Camunda Tasklist. The `camForm.submit` message ensures the task completes with the submitted variables.

- **Disabling the Button**:  
  The button starts as `disabled` and is only enabled when the input field passes validation.

---

#### **3. Add Form to User Task**

- Link the form in the BPMN model:

```xml
<bpmn:userTask id="UserTask_1" name="User Task">
  <bpmn:extensionElements>
    <zeebe:formDefinition formKey="embedded:app:forms/validation-form.html" />
  </bpmn:extensionElements>
  <bpmn:incoming>SequenceFlow_1</bpmn:incoming>
  <bpmn:outgoing>SequenceFlow_2</bpmn:outgoing>
</bpmn:userTask>
```

Ensure the form is correctly packaged and deployed with the process.

---

### **Result**

- The user sees a form with an input field.
- The **Complete Task** button is disabled initially.
- The button is enabled only when the input field is valid.
- Upon clicking **Complete Task**, the form variables are submitted, and the task completes.

This setup ensures user input is validated before the task can proceed.