For k8s read this: [Configure a Security Context for a Pod or Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

```bash
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

RUN chown -R appuser:appgroup /app

USER appuser
```

Let's break down the lines you've provided from the Dockerfile to understand what each command does:

### 1. `RUN addgroup -S appgroup && adduser -S appuser -G appgroup`

- **`addgroup -S appgroup`**:
  - This command creates a new group named `appgroup`.
  - The `-S` flag indicates that it should create a system group. System groups typically have a GID (Group ID) less than 1000, which is a convention in many Linux distributions to differentiate system users/groups from regular users/groups.

- **`adduser -S appuser -G appgroup`**:
  - This command creates a new user named `appuser`.
  - The `-S` flag indicates that it should create a system user. Like system groups, system users have a UID (User ID) less than 1000.
  - The `-G appgroup` option specifies that the new user `appuser` should be added to the previously created `appgroup`.

### Summary of the First Line:
This line creates a system group called `appgroup` and a system user called `appuser`, assigning `appuser` to the `appgroup`. This is a common practice for setting up users and groups in a Docker container to enhance security by avoiding running processes as the root user.

---

### 2. `RUN chown -R appuser:appgroup /app`

- **`chown -R appuser:appgroup /app`**:
  - This command changes the ownership of the `/app` directory (and all its contents, due to the `-R` flag for recursive) to the user `appuser` and the group `appgroup`.
  - This ensures that the `appuser` has the necessary permissions to read, write, and execute files in the `/app` directory, which is important for applications that need to run without root privileges.

### Summary of the Second Line:
This line sets the ownership of the `/app` directory to `appuser` and `appgroup`, allowing the application to run with the permissions of the non-root user.

---

### 3. `USER appuser`

- **`USER appuser`**:
  - This command sets the default user for any subsequent instructions in the Dockerfile to `appuser`.
  - After this command, any commands that run in the container will execute with the privileges of `appuser` rather than the default root user.

### Summary of the Third Line:
This line changes the context of the Docker container to run as `appuser`, which enhances security by limiting the permissions of the running application.

---

### Overall Summary
Together, these lines in the Dockerfile accomplish the following:

1. Create a non-root user (`appuser`) and a group (`appgroup`) to enhance security.
2. Change the ownership of the `/app` directory to ensure that `appuser` has the necessary permissions to access and modify files within that directory.
3. Set the container to run as `appuser`, preventing the application from executing with root privileges.

This is a common pattern in Dockerfiles to ensure that applications run securely and with the least amount of privilege necessary.