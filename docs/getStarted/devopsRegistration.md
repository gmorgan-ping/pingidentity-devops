---
title: Ping Identity DevOps Registration
---
# Ping Identity DevOps Registration

Registering for Ping Identity's DevOps Program grants you credentials that, when supplied to product containers automatically, retrieve an evaluation license on startup.

To register for the DevOps Program:

* Make sure you have a registered account with Ping Identity.  If you're not sure, click the link to [Sign On](https://www.pingidentity.com/en/account/sign-on.html) and follow the instructions.
  * If you don't have an account, create one [here](https://www.pingidentity.com/en/account/register.html).
  * Otherwise, sign on.
  * When signing on, select **Support and Community** in the **Select Account** list.
  * After you're signed on, you're directed to your profile [page](https://support.pingidentity.com/s/).
  * In the right-side menu, click **REGISTER FOR DEVOPS PROGRAM**.
  ![Register for DevOps](../images/DEVOPS_REGISTRATION.png)
  * You'll receive a confirmation message, and your credentials will be forwarded to the email address associated with your Ping Identity account.

Example:

* `PING_IDENTITY_DEVOPS_USER=jsmith@example.com`
* `PING_IDENTITY_DEVOPS_KEY=e9bd26ac-17e9-4133-a981-d7a7509314b2`

## For Kubernetes

Our deployments, by default, look for a Kubernetes secret named `devops-secret`.

You can create a Kubernetes secret that contains the environment variables `PING_IDENTITY_DEVOPS_USER`, `PING_IDENTITY_DEVOPS_KEY`, and `PING_IDENTITY_ACCEPT_EULA`

   ```sh
    kubectl create secret generic devops-secret \
      --from-literal=PING_IDENTITY_DEVOPS_USER="<PING_IDENTITY_DEVOPS_USER>" \
      --from-literal=PING_IDENTITY_DEVOPS_KEY="<PING_IDENTITY_DEVOPS_KEY>"
      --from-literal=PING_IDENTITY_ACCEPT_EULA="YES"
   ```

