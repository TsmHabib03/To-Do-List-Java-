# To-Do List Java Application

A simple, beginner-friendly graphical To-Do List application built with Java Swing. This project is a great example for learning Object-Oriented Programming (OOP) concepts through a practical, real-world application.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Code Structure](#code-structure)
3. [Classes Explanation](#classes-explanation)
4. [Code Block Explanation](#code-block-explanation)
5. [Object-Oriented Programming Concepts Used](#object-oriented-programming-concepts-used)
6. [How the System Works](#how-the-system-works)
7. [Simple Demo Scenario](#simple-demo-scenario)
8. [How to Run the Project](#how-to-run-the-project)

---

## Project Overview

This application is a **graphical To-Do List manager** built using Java's built-in GUI toolkit called **Swing**. It allows users to:

- **Add** new tasks by clicking the "Add Task" button.
- **Type** a task description into a text field.
- **Mark** a task as completed using a checkbox (the text gets a strikethrough effect).
- **Delete** a task by clicking the "X" button next to it.

It is a desktop application, meaning it opens as a standalone window on your computer. No internet connection or database is required — the tasks exist only while the program is running.

---

## Code Structure

```
To-Do-List-Java-/
└── To-do-List/
    ├── To-do-List.iml          # IntelliJ IDEA project file
    ├── out/                    # Compiled .class files (generated after build)
    └── src/                    # All Java source code lives here
        ├── App.java            # Entry point — starts the application
        ├── CommonConstants.java # Shared size/dimension constants for the UI
        ├── TaskComponent.java  # Represents a single task row in the list
        ├── ToDoLIstGui.java    # The main application window (GUI)
        └── poppins/            # Poppins font files used for styling the UI
            ├── Poppins-Regular.ttf
            └── ... (other font weight variants)
```

### Key Points About the Structure

| File | Role |
|---|---|
| `App.java` | The **main entry point** — the program starts here |
| `CommonConstants.java` | Stores **UI size values** so they can be reused without magic numbers |
| `TaskComponent.java` | A **reusable component** for a single task row (checkbox + text + delete button) |
| `ToDoLIstGui.java` | The **main window** that holds all other components together |
| `poppins/` | **Font assets** used to style the application text with the Poppins typeface |

---

## Classes Explanation

### 1. `App` — The Entry Point

**File:** `App.java`

**Purpose:** This is the starting class. Every Java application needs a `main` method — this is where the JVM begins execution. It launches the GUI on the correct thread.

**Methods:**

| Method | Description |
|---|---|
| `main(String[] args)` | The entry point of the program. Uses `SwingUtilities.invokeLater()` to safely create the GUI on the Swing Event Dispatch Thread (EDT). |

**Why `invokeLater`?** Swing is not thread-safe. The `invokeLater` call ensures the GUI is created and updated on the dedicated Swing thread, preventing visual glitches.

---

### 2. `CommonConstants` — UI Configuration

**File:** `CommonConstants.java`

**Purpose:** Acts as a **central store for all UI sizing constants**. Instead of scattering hard-coded numbers (like `540`, `760`) throughout the code, they are defined once here and referenced everywhere. This makes it easy to change the layout in one place.

**Attributes (all `public static final`):**

| Constant | Value | Description |
|---|---|---|
| `GUI_SIZE` | `540 × 760` px | The overall window size |
| `BANNER_SIZE` | `540 × 50` px | The "To Do List" title banner at the top |
| `TASKPANEL_SIZE` | `510 × 585` px | The scrollable panel that holds all tasks |
| `ADDTASK_BUTTON_SIZE` | `540 × 50` px | The "Add Task" button at the bottom |
| `TASKFIELD_SIZE` | `~408 × 50` px | The text input area of each task |
| `CHECKBOX_SIZE` | `~20 × 50` px | The checkbox in each task row |
| `DELETE_BUTTON_SIZE` | `~48 × 50` px | The delete ("X") button in each task row |

**No methods** — this class only holds constants.

---

### 3. `TaskComponent` — A Single Task Row

**File:** `TaskComponent.java`

**Purpose:** Represents one task in the to-do list. Each time the user clicks "Add Task", a new `TaskComponent` is created and displayed. It contains three UI elements: a **checkbox**, a **text area**, and a **delete button**.

**Inherits from:** `JPanel` (a Swing container panel)  
**Implements:** `ActionListener` (to respond to user interactions)

**Attributes:**

| Attribute | Type | Access | Description |
|---|---|---|---|
| `checkBox` | `JCheckBox` | `private` | Checkbox to mark a task as complete |
| `taskField` | `JTextPane` | `private` | Text area where the task description is typed |
| `deleteButton` | `JButton` | `private` | The "X" button to remove this task |
| `parentPanel` | `JPanel` | `private` | Reference to the container panel so the task can remove itself |

**Methods:**

| Method | Description |
|---|---|
| `TaskComponent(JPanel parentPanel)` | Constructor — sets up and arranges the checkbox, text field, and delete button inside the panel |
| `getTaskField()` | Getter method — returns the `taskField` so other classes can access it (e.g., to set focus) |
| `actionPerformed(ActionEvent e)` | Called when the checkbox or delete button is clicked. Handles strikethrough text or removes the task |

---

### 4. `ToDoLIstGui` — The Main Application Window

**File:** `ToDoLIstGui.java`

**Purpose:** This is the **main window** of the application. It sets up the overall layout and is responsible for adding new tasks when the user clicks "Add Task".

**Inherits from:** `JFrame` (a Swing top-level window)  
**Implements:** `ActionListener` (to respond to the "Add Task" button click)

**Attributes:**

| Attribute | Type | Access | Description |
|---|---|---|---|
| `taskPanel` | `JPanel` | `private` | Outer panel that wraps the task list (used with `JScrollPane`) |
| `taskComponentPanel` | `JPanel` | `private` | Inner panel that directly holds all `TaskComponent` objects, arranged vertically |

**Methods:**

| Method | Description |
|---|---|
| `ToDoLIstGui()` | Constructor — configures window settings (title, size, close behavior) and calls `addGuiComponents()` |
| `addGuiComponents()` | Private method — builds the entire UI: adds the banner label, scrollable task area, and "Add Task" button |
| `createFont(String resource, float size)` | Private helper — loads a Poppins `.ttf` font file from the `poppins/` folder and returns a `Font` object |
| `actionPerformed(ActionEvent e)` | Handles the "Add Task" button click — creates a new `TaskComponent` and adds it to the panel |

---

## Code Block Explanation

### Block 1 — Launching the GUI on the Swing Thread (`App.java`)

```java
SwingUtilities.invokeLater(new Runnable() {
    @Override
    public void run() {
        new ToDoLIstGui().setVisible(true);
    }
});
```

- `SwingUtilities.invokeLater(...)` schedules the code inside `run()` to execute on the **Event Dispatch Thread (EDT)** — the dedicated thread Swing uses for all UI operations.
- Without this, creating GUI components from the `main` thread can cause unpredictable rendering bugs.
- `new ToDoLIstGui()` creates the main window, and `.setVisible(true)` makes it appear on screen.

---

### Block 2 — Strikethrough Effect When a Task is Completed (`TaskComponent.java`)

```java
if (checkBox.isSelected()) {
    String taskText = taskField.getText().replaceAll("<[^>]*>", "");
    taskField.setText("<html><s>" + taskText + "</s></html>");
} else if (!checkBox.isSelected()) {
    String taskText = taskField.getText().replaceAll("<[^>]*>", "");
    taskField.setText(taskText);
}
```

- When the **checkbox is checked**, the task text is wrapped in HTML `<s>` (strikethrough) tags.
- `replaceAll("<[^>]*>", "")` is a **regular expression** that strips any existing HTML tags from the text before re-applying the strikethrough. This prevents tags from piling up on repeated clicks.
- When the **checkbox is unchecked**, the tags are stripped and the plain text is restored.
- `JTextPane` supports HTML rendering, which is why this HTML approach works.

---

### Block 3 — Deleting a Task (`TaskComponent.java`)

```java
if (e.getActionCommand().equalsIgnoreCase("X")) {
    parentPanel.remove(this);
    parentPanel.repaint();
    parentPanel.revalidate();
}
```

- When the "X" button is clicked, its action command is `"X"`.
- `parentPanel.remove(this)` removes this `TaskComponent` from its parent container.
- `repaint()` tells Swing to redraw the panel visually.
- `revalidate()` recalculates the layout so the remaining tasks shift up to fill the gap.
- The `parentPanel` reference is passed in via the constructor — this is how the component knows which container to remove itself from.

---

### Block 4 — Adding a New Task (`ToDoLIstGui.java`)

```java
if (command.equalsIgnoreCase("Add Task")) {
    TaskComponent taskComponent = new TaskComponent(taskComponentPanel);
    taskComponentPanel.add(taskComponent);

    if (taskComponentPanel.getComponentCount() > 1) {
        TaskComponent previousTask = (TaskComponent) taskComponentPanel.getComponent(
                taskComponentPanel.getComponentCount() - 2);
        previousTask.getTaskField().setBackground(null);
    }

    taskComponent.getTaskField().requestFocus();
    repaint();
    revalidate();
}
```

- A new `TaskComponent` is created and added to `taskComponentPanel`.
- The **previously added task** has its background reset to `null` (default/gray) to visually indicate it is no longer the active task.
- `requestFocus()` automatically places the cursor in the new task's text field so the user can type immediately.
- `repaint()` and `revalidate()` refresh the layout to display the new task.

---

### Block 5 — Loading a Custom Font (`ToDoLIstGui.java`)

```java
private Font createFont(String resource, float size) {
    String filePath = getClass().getClassLoader().getResource(resource).getPath();
    if (filePath.contains("%20")) {
        filePath = filePath.replaceAll("%20", " ");
    }
    try {
        File customFontFile = new File(filePath);
        Font customFont = Font.createFont(Font.TRUETYPE_FONT, customFontFile).deriveFont(size);
        return customFont;
    } catch (Exception e) {
        System.out.println("Error: " + e);
    }
    return null;
}
```

- `getClass().getClassLoader().getResource(resource)` locates the font file inside the project's classpath.
- The `%20` check handles folder paths that contain **spaces** (spaces are encoded as `%20` in URLs/paths).
- `Font.createFont(Font.TRUETYPE_FONT, file)` loads the `.ttf` file as a Java `Font` object.
- `.deriveFont(size)` creates a version of the font at the specified point size.
- If the font fails to load, `null` is returned and Java falls back to its default font.

---

## Object-Oriented Programming Concepts Used

### 1. Encapsulation

**Definition:** Hiding the internal details of a class and providing controlled access through methods.

**Where it appears:**
- In `TaskComponent`, the fields `checkBox`, `taskField`, `deleteButton`, and `parentPanel` are all declared `private`. They cannot be accessed directly from outside the class.
- The `getTaskField()` method is a **getter** that provides read access to `taskField` without exposing it directly. `ToDoLIstGui` uses this to call `requestFocus()` and `setBackground()` on the field.

```java
// Private field — hidden from outside
private JTextPane taskField;

// Public getter — controlled access
public JTextPane getTaskField() {
    return taskField;
}
```

---

### 2. Inheritance

**Definition:** A class acquires the properties and behaviors of another class using the `extends` keyword.

**Where it appears:**

| Child Class | Parent Class | What it inherits |
|---|---|---|
| `TaskComponent` | `JPanel` | All panel behavior: layout, painting, adding child components |
| `ToDoLIstGui` | `JFrame` | All window behavior: title bar, close button, content pane |

```java
public class TaskComponent extends JPanel ...
public class ToDoLIstGui extends JFrame ...
```

By extending `JPanel`, `TaskComponent` **is** a panel and can be added directly to other containers like `taskComponentPanel`. By extending `JFrame`, `ToDoLIstGui` **is** a window and can be shown on screen with `.setVisible(true)`.

---

### 3. Polymorphism

**Definition:** The ability of different objects to respond to the same method call in different ways.

**Where it appears — Interface Polymorphism:**

Both `TaskComponent` and `ToDoLIstGui` implement the `ActionListener` interface, which requires an `actionPerformed(ActionEvent e)` method. Even though the method signature is the same, each class handles events differently:

- `TaskComponent.actionPerformed()` — handles the checkbox (strikethrough) and delete button (remove task).
- `ToDoLIstGui.actionPerformed()` — handles the "Add Task" button (create a new task).

```java
// In TaskComponent
@Override
public void actionPerformed(ActionEvent e) {
    // handles checkbox and delete
}

// In ToDoLIstGui
@Override
public void actionPerformed(ActionEvent e) {
    // handles "Add Task" button
}
```

The Swing framework calls `actionPerformed()` on the correct listener without needing to know which specific class it belongs to — that's polymorphism in action.

---

### 4. Abstraction

**Definition:** Hiding complex implementation details and exposing only what is necessary.

**Where it appears:**

- **`ActionListener` interface:** Both `TaskComponent` and `ToDoLIstGui` implement `ActionListener`. The interface defines *what* must be done (`actionPerformed` must exist) but not *how*. Each class provides its own implementation — the complexity is hidden behind the interface.
- **`JPanel`, `JFrame`, `JButton`, etc.:** Swing's class hierarchy is a powerful example of abstraction. You call `add(component)` or `setVisible(true)` without needing to know how Java draws pixels on screen.
- **`CommonConstants`:** Abstracts away the raw numbers. Instead of writing `540` everywhere, you write `CommonConstants.GUI_SIZE.width`, making the intent clear and the code easier to maintain.

---

## How the System Works

Here is a step-by-step walkthrough of the program's flow from startup to user interaction:

```
Step 1: Program Starts
  └── App.main() is called by the JVM

Step 2: GUI is Created
  └── SwingUtilities.invokeLater() schedules GUI creation on the EDT
  └── new ToDoLIstGui() is executed:
       ├── Window title, size, and close behavior are configured
       └── addGuiComponents() is called:
            ├── Banner label "To Do List" is created and positioned
            ├── taskComponentPanel (BoxLayout, vertical) is created inside taskPanel
            ├── JScrollPane wraps taskPanel for scrolling
            └── "Add Task" button is added at the bottom

Step 3: Window is Shown
  └── setVisible(true) makes the window appear on screen

Step 4: User Clicks "Add Task"
  └── ToDoLIstGui.actionPerformed() is triggered
  └── A new TaskComponent is created with taskComponentPanel as its parent
  └── The new TaskComponent is added to taskComponentPanel
  └── If a previous task exists, its background is reset to default (gray)
  └── The new task's text field receives focus automatically
  └── repaint() and revalidate() refresh the display

Step 5: User Types in the Task Field
  └── The JTextPane is in focus (white background)
  └── The user types their task description
  └── When focus is lost (user clicks elsewhere), the background turns back to default

Step 6: User Checks the Checkbox
  └── TaskComponent.actionPerformed() is triggered
  └── If checked: task text is wrapped in <html><s>...</s></html> (strikethrough)
  └── If unchecked: HTML tags are stripped, plain text is restored

Step 7: User Clicks the "X" Delete Button
  └── TaskComponent.actionPerformed() is triggered
  └── parentPanel.remove(this) removes the TaskComponent from the panel
  └── repaint() and revalidate() refresh the display — the deleted task disappears
```

---

## Simple Demo Scenario

> **Goal:** Add three tasks, complete one, and delete another.

**1. Launch the application**

The "To Do List" window opens (540×760 px). The task area is empty. The "Add Task" button is at the bottom.

**2. Add a task**

Click **"Add Task"**. A new row appears with a checkbox, a white text field, and an "X" button. The cursor is already in the text field — just start typing.

Type: `Buy groceries`

**3. Add more tasks**

Click **"Add Task"** two more times. Type:
- `Finish homework`
- `Call the dentist`

The task list now shows three rows stacked vertically.

**4. Mark a task as complete**

Click the **checkbox** next to `Buy groceries`. The text immediately gains a strikethrough:  
~~`Buy groceries`~~

**5. Delete a task**

Click the **"X"** button next to `Call the dentist`. That row disappears, and the remaining two tasks stay in place.

**6. Scroll (if needed)**

If you add many tasks and they exceed the visible area, a **vertical scrollbar** appears on the right side. Scroll down to see all tasks.

---

## How to Run the Project

### Prerequisites

- **Java Development Kit (JDK) 8 or higher** must be installed.  
  Check by running: `java -version` and `javac -version` in your terminal.

### Option A: Using IntelliJ IDEA (Recommended)

1. Open IntelliJ IDEA.
2. Click **File → Open** and select the `To-do-List` folder.
3. IntelliJ will detect the `.iml` project file automatically.
4. Navigate to `src/App.java`.
5. Click the green **Run** button (▶) next to the `main` method.
6. The To-Do List window will appear.

### Option B: Using the Command Line

1. Open a terminal and navigate to the `src` directory:
   ```bash
   cd path/to/To-Do-List-Java-/To-do-List/src
   ```

2. Compile all Java files:
   ```bash
   javac App.java CommonConstants.java TaskComponent.java ToDoLIstGui.java
   ```

3. Run the application:
   ```bash
   java App
   ```

4. The To-Do List window will open.

> **Note:** The `poppins/` font folder must remain inside the `src/` directory alongside the `.java` files for the custom font to load correctly. If the font fails to load, the application will still run but will use Java's default font instead.
