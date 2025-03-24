# **Automated Workflow for Date, Time, and Interval Processing**

## **Overview**
This project demonstrates how to build an automated workflow in **n8n** that processes date and time data by adding five days to an input date from a **Customer Datastore** node. The workflow then checks if the calculated date occurs after 1959. If true, it waits for one minute before setting the final output value. The workflow is triggered every 30 minutes.
![main workflow](https://github.com/user-attachments/assets/9d6974bf-893b-4033-9967-fb661a422286)

## **Workflow Objectives**
- Retrieve all customer data.
- Add five days to the created date.
- Round up the date to the **End of Month**.
- Check if the updated date is **after January 1, 1960**.
- Introduce a **1-minute delay** if the condition is met.
- Set the calculated date as an output field.
- Schedule the workflow to run **every 30 minutes**.

---

## **Step-by-Step Implementation**

### **1️⃣ Add the Customer Datastore (n8n Training) Node**
1. Insert a **Customer Datastore (n8n Training)** node.
2. Choose the action **Get All People**.
3. Select the option **Return All** to fetch all data.
  ![customer datastore(n8n training)](https://github.com/user-attachments/assets/70c09b34-5f72-4cc2-9224-3cb1bb25cea4)
---

### **2️⃣ Add a Date & Time Node to Round the Date**
1. Add a **Date & Time** node and connect it to the **Customer Datastore** node.
2. Select the option **Round a Date**.
3. Choose the **created date** as the input field.
4. Set the **Mode** to `Round Up`.
5. Set the **To** option to `End of Month`.
6. Name the output field as `new-date`.
7. In **Options**, select `Add Option` → `Include Input Fields`.
![Date and Time](https://github.com/user-attachments/assets/b21c31fe-03fe-44f1-8e67-b9aae0cd9246)

---

### **3️⃣ Add an If Node to Check the Date Condition**
1. Add an **If node** and connect it to the **Date & Time** node.
2. Use the `new-date` field as the first condition.
3. Set the **Comparison Operator** to `Date & Time > is after`.
4. Enter `1960-01-01 00:00:00` as the second part of the condition.
5. This step will generate two branches:
   - **True Branch**: If `new-date` is after `1960-01-01`.
   - **False Branch**: If `new-date` is before `1960-01-01`.
![if node](https://github.com/user-attachments/assets/e414ca12-cb5d-4cdd-95c5-9c5e939e8776)
---

### **4️⃣ Add a Wait Node to Delay Processing**
1. Connect a **Wait node** to the **True output** of the **If node**.
2. Set **Resume** to `After Time Interval`.
3. Configure the following parameters:

   - **Wait Amount**: `1.00`
   - **Wait Unit**: `Minutes`
![if node](https://github.com/user-attachments/assets/0e352d77-3a1c-4d1d-8a63-2d92991bc420)
---

### **5️⃣ Add an Edit Fields (Set) Node**
1. Add a **Set node (Edit Fields)** after the **Wait node**.
2. Use either **JSON Mode** or **Manual Mapping Mode**.
3. Create a new field named `outputValue`.
4. Set its value to `new-date`.
5. Select the option `Include Other Input Fields` → `Include All Fields`.

![edit fields set node](https://github.com/user-attachments/assets/4f6be743-c0b9-42df-a999-2cbc75f00b61)


---

### **6️⃣ Add a Schedule Trigger Node**
1. Insert a **Schedule Trigger node** at the beginning of the workflow.
2. Set **Trigger Interval** to `Minutes`.
3. Configure **Minutes Between Triggers** to `30` (runs every 30 minutes).
4. Connect the **Schedule Trigger node** to the **Customer Datastore (n8n Training) node**.
![schedule trigger](https://github.com/user-attachments/assets/acdf2dee-c0bc-4e58-9a48-ce178548c0b7)

---

## **Final Steps**
✅ Activate your workflow to start running every **30 minutes**.
✅ Use the **Manual Trigger node** for testing purposes.
✅ Ensure all nodes are connected correctly.

---

## **Expected Workflow Behavior**
- The workflow **retrieves customer data** from the datastore.
- It **adds five days** and **rounds up** the date to the **end of the month**.
- If the **new date is after 1960**, it introduces a **1-minute delay**.
- After waiting, the workflow **sets the final output value**.
- This process **repeats every 30 minutes** automatically.

---

# Date & Time Node

The **Date & Time** node in n8n allows you to manipulate date and time values, convert between different formats, and perform various date-related calculations.

## Timezone Settings

The node relies on the **timezone setting** configured in n8n. The priority order is:

1. **Workflow timezone** (if set in Workflow settings).
2. **Instance timezone** (default is **America/New York** for self-hosted instances and **GMT** for n8n Cloud users if not detected at signup).
3. Self-hosted users can change the timezone via **environment variables**.
4. n8n Cloud admins can modify the timezone in the **Admin dashboard**.

## Working with Date and Time in n8n

In addition to this node, you can work with date and time in:
- **Code Node** (via JavaScript)
- **Expressions in any node** (using [Luxon](https://moment.github.io/luxon/#/))

## Supported Operations

The **Date & Time** node provides the following operations:

### 1. Add to a Date
- **Date to Add To**: Specify the base date.
- **Time Unit**: Choose from seconds, minutes, hours, days, etc.
- **Duration**: Specify how much time to add.
- **Output Field Name**: Define the field to store the result.
- **Options**: Include input fields.

### 2. Subtract from a Date
- **Date to Subtract From**: Specify the base date.
- **Time Unit**: Choose the time unit.
- **Duration**: Specify the value to subtract.
- **Output Field Name**: Define the field for the result.
- **Options**: Include input fields.

### 3. Extract Part of a Date
- **Date**: Specify the date.
- **Part**: Choose the part to extract (Year, Month, Day, Hour, etc.).
- **Output Field Name**: Define the field for the result.
- **Options**: Include input fields.

### 4. Format a Date
- **Date**: Specify the date to format.
- **Format**: Choose from preset options or use Luxon custom format tokens.
- **Output Field Name**: Define the field for the result.
- **Options**: Include input fields, specify the original format, or use workflow timezone.

### 5. Get Current Date
- **Include Current Time**: Option to include time.
- **Output Field Name**: Define the result field.
- **Options**: Include input fields, specify timezone.

### 6. Get Time Between Dates
- **Start Date**: Specify the first date.
- **End Date**: Specify the second date.
- **Units**: Choose the units (Seconds, Minutes, Hours, etc.).
- **Output Field Name**: Define the field for the result.
- **Options**: Include input fields, output as ISO string.

### 7. Round a Date
- **Date**: Specify the date to round.
- **Mode**: Choose whether to round **up** or **down**.
- **To Nearest**: Select the unit (Year, Month, Week, Day, etc.).
- **Output Field Name**: Define the field for the result.
- **Options**: Include input fields.

## Summary

The **Date & Time** node is a powerful tool in n8n for handling various time-based operations. With flexible options for formatting, calculation, and extraction, it helps automate workflows that rely on precise date and time management.

For advanced use, consider integrating Luxon with the **Code Node** to extend date manipulation capabilities further.



## **Conclusion**
This workflow automates **date-based processing** in n8n, ensuring efficient handling of time-based conditions and scheduled execution. 🚀

For any issues, feel free to reach out or check the n8n documentation!

---

**Happy Automating! 🎯**
