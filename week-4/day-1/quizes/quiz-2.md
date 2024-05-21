# Day-1 Quiz 2

## Question 1:

What is a proper definition of IAM Roles?

- A) An IAM entity that defines a set of permissions for making AWS service requests, that will be used by AWS services
  Correct: IAM Roles are entities that define a set of permissions for making AWS service requests and are used by AWS services to perform tasks on your behalf.

- B) IAM Users in multiple Groups
  Incorrect: IAM Users in multiple groups refer to user management, not to the definition of permissions.

- C) A password policy
  Incorrect: A password policy defines password requirements, not permissions for AWS service requests.

- D) Permissions assigned to Users to perform actions
  Incorrect: This describes IAM Policies, not IAM Roles.

## Question 2:

Which of the following is an IAM Security Tool?

- A) IAM Credentials Report
  Correct: The IAM Credentials Report is a security tool that provides a report on the status of your IAM credentials.

- B) IAM Root Account Manager
  Incorrect: There is no such tool as the IAM Root Account Manager in AWS.

- C) IAM Services Report
  Incorrect: This option does not exist; the correct tool is the IAM Credentials Report.

- D) IAM Security Advisor
  Incorrect: AWS does not offer a tool named IAM Security Advisor.

## Question 3:

Which answer is INCORRECT regarding IAM Users?

- A) IAM Users can belong to multiple groups
  Incorrect: This statement is true. IAM Users can belong to multiple groups.

- B) IAM Users don't have to belong to a group
  Incorrect: This statement is true. IAM Users are not required to belong to a group.

- C) IAM Users can have policies assigned to them
  Incorrect: This statement is true. IAM Users can have policies directly assigned to them.

- D) IAM Users access AWS with the root account credentials
  Correct: This statement is incorrect. IAM Users should not use root account credentials. They should have their own individual credentials.

## Question 4:

Which of the following is an IAM best practice?

- A) Don't use the root user account
  Correct: It is a best practice to avoid using the root user account for everyday tasks to enhance security.

- B) Create several users for a physical person
  Incorrect: Each physical person should have one unique IAM user to maintain accountability.

- C) Share credentials so a colleague can perform a task for you
  Incorrect: Sharing credentials is not a best practice. Each user should have their own credentials.

- D) Do not enable MFA for easier access
  Incorrect: Enabling Multi-Factor Authentication (MFA) is a best practice to enhance account security.

## Question 5:

What are IAM Policies?

- A) AWS services performable actions
  Incorrect: IAM Policies define permissions, not actions performable by AWS services.

- B) JSON documents to define Users, Groups or Roles' permissions
  Correct: IAM Policies are JSON documents that define the permissions for Users, Groups, or Roles.

- C) Rules to set up a password for IAM Users
  Incorrect: Rules to set up a password for IAM Users are defined by password policies, not IAM Policies.

## Question 6:

Under the shared responsibility model, what is the customer responsible for in IAM?

- A) Infrastructure security
  Incorrect: Infrastructure security is managed by AWS in the shared responsibility model.

- B) Compliance validation
  Incorrect: AWS manages compliance validation. Customers are responsible for ensuring compliance in their use of AWS services.

- C) Configuration and vulnerability analysis
  Incorrect: AWS manages the underlying infrastructure. Customers are responsible for the security of what they put in the cloud.

- D) Assigning users proper IAM Policies
  Correct: Customers are responsible for managing IAM users and assigning appropriate IAM policies.

## Question 7:

Which of the following statements is TRUE?

- A) The AWS CLI can interact with AWS using commands in your command-line shell, while the AWS SDK can interact with AWS programmatically.
  Correct: The AWS CLI is used for interacting with AWS from the command line, while the AWS SDK is used for programmatic interactions with AWS services.

- B) The AWS SDK can interact with AWS using commands in your command-line shell, while the AWS CLI can interact with AWS programmatically.
  Incorrect: This statement is false. The AWS SDK is for programmatic interactions, and the AWS CLI is for command-line interactions.

## Question 8:

Which principle should you apply regarding IAM Permissions?

- A) Grant most privilege
  Incorrect: Granting the most privilege goes against security best practices.

- B) Grant least privilege
  Correct: The principle of least privilege means granting only the permissions needed to perform a task.

- C) Grant permissions if your employee asks you to
  Incorrect: Permissions should be granted based on the principle of least privilege, not just on request.

- D) Restrict root account permissions
  Incorrect: The root account should not be used for everyday tasks, so this is not a principle for regular IAM permissions.

## Question 9:

What should you do to increase your root account security?

- A) Enable Multi-Factor Authentication (MFA)
  Correct: Enabling MFA provides an additional layer of security for the root account.

- B) Remove permissions from the root account
  Incorrect: You cannot remove permissions from the root account, but you can avoid using it for everyday tasks.

- C) Use AWS only through the Command Line Interface (CLI)
  Incorrect: Using the CLI alone does not inherently increase security.
