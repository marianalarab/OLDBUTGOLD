## Workflow Exceptions

Reference: (http://help.sap.com/saphelp_nw04s/helpdata/en/c5/e4ad98453d11d189430000e829fbbd/frameset.htm)

This document details the procedure in defining exceptions in a method and using the same in a workflow.
Let us first look at raising exceptions in a method:
Go to Business Object Builder (Transaction SWO1).
Create a business object.

<img width="306" alt="image" src="https://github.com/user-attachments/assets/085c89ea-05dd-4ad6-a3a3-738c175f37fa" />
<br>

Now create a method.

<img width="351" alt="image" src="https://github.com/user-attachments/assets/12ae2327-09b7-42d1-ad55-d828bfd230e0" />

Now select the method you have created and click on button “Exceptions” available on the toolbar.
Following screen appears:

<img width="360" alt="image" src="https://github.com/user-attachments/assets/d281bbad-e14a-4b23-88bb-95958f1d6f1c" />

Now click on Create. Following popup screen appears:

<img width="297" alt="image" src="https://github.com/user-attachments/assets/b22dcba5-2f7c-4c86-b4bb-c89995b8c2e4" />

Enter the information as shown above. The error type “Temporary error” is chosen in the case wherein the record is locked by somebody else or some required resource is not available. The error type “Application Error” is to be chosen when there is no authorization for the document you are processing. The error type “System error” is to be chosen if there are no values passed for the mandatory parameters.
Click on continue.

<img width="369" alt="image" src="https://github.com/user-attachments/assets/4335fd1e-a783-45a0-bb88-1d2583d38a7c" />

Go back to the main screen.
Now in the method, provide the following code:

<img width="342" alt="image" src="https://github.com/user-attachments/assets/47008848-29fb-4f4f-ab9d-36f6090fe9c6" />

Save the business object and release the same.

<img width="342" alt="image" src="https://github.com/user-attachments/assets/1f9d508c-40e1-496f-aa22-7bba4dd85c18" />

Try executing the method by clicking on Test/Execute (F8). The exception would be raised as shown below:

<img width="270" alt="image" src="https://github.com/user-attachments/assets/4187caaa-f714-457e-b4ce-a593f98f8f1e" />

Capturing the exception raised by the method in the workflow:
Go to Workflow Builder (TCode: SWO1).
Create the step “Activity”

<img width="225" alt="image" src="https://github.com/user-attachments/assets/a1982e2f-2cdf-442c-a80a-d227c884d00e" />

Create a task and provide the following details in the new task:

<img width="234" alt="image" src="https://github.com/user-attachments/assets/db22eb08-7ca1-407a-a929-15341b4c0569" />

Save your entries and return to the previous screen.

<img width="297" alt="image" src="https://github.com/user-attachments/assets/a7a45042-8219-4b46-a1b2-0752ce1aee91" />

Click on “Outcomes” tab. Here you can observe the exception you defined in your method:

<img width="297" alt="image" src="https://github.com/user-attachments/assets/c0dfa584-f311-4664-9eaf-ac1d830e2607" />

As observed in the observe screen, the outcome is not active by default. If we do not handle the exception, the work item might go into the error status. Activate the outcome:

<img width="369" alt="image" src="https://github.com/user-attachments/assets/e2d32bab-a4d0-4507-9a43-1222edcf5548" />

Save your entries and go back to the main screen of Workflow builder. Please ensure that you have done the agent assignment for this task.

<img width="288" alt="image" src="https://github.com/user-attachments/assets/e1b1a775-fa07-44ca-9bab-26ebf24bf37b" />

You can now observe a new branch when the exception is raised. You can define your steps whenever the exception is raised.















