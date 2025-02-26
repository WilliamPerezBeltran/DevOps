# Jenkins Overview

Jenkins is an open-source automation software, written in Java and running on the JVM. It is highly extensible due to its plugin ecosystem, allowing users to write their own plugins in Java. The Jenkins community has developed numerous plugins that enhance its functionality.

Jenkins supports both horizontal and vertical scaling. It can run multiple jobs concurrently on a single machine, and additional resources can be allocated if needed. If one machine is insufficient, Jenkins allows horizontal scaling using "slaves," managing multiple nodes to distribute workloads efficiently.

Security updates and innovations are continuously added to Jenkins, making it a crucial component for enterprises. Since Jenkins contains all automation processes, it is a primary security target. In recent years, Jenkins has introduced "Pipelines as Code" to enable programmatic automation and reproducible job migration.

## Jobs and Builds

The core functionality of Jenkins revolves around jobs. Jenkins can execute multiple jobs simultaneously, managed by the **Build Executor**.

- **Job Execution:** Each job execution is called a **Build**.
- **Workspace:** For each job, Jenkins creates a folder inside `/var/lib/jenkins/workspace/`.
- **Build History:** Each job maintains a history of executed builds.

### Job Types
Jenkins supports various job types, including:
- **Freestyle Project**
- **Pipeline**
- **Folder**
- **Multi-Configuration Project**

Each build execution increments the **Build History**, which helps in tracking job success and failure for auditing purposes.

## Configuring Dependent Jobs

To manage job dependencies, Jenkins provides the **Parameterized Trigger** plugin. This allows triggering one job based on another job’s success.

### Steps to Configure:
1. Install the `Parameterized Trigger` plugin and restart Jenkins.
2. Create two jobs: `hello-platzi` and `watchers`.
3. In `watchers`, enable **Build after other projects are built** and set it to trigger after `hello-platzi` completes successfully.
4. Add the following in **Execute Shell**:
   ```sh
   echo "Running after hello-platzi success"
   ```
5. Create another job named `parameterized`, enable **This project is parameterized**, and add a parameter `ROOT_ID`.
6. In **Execute Shell**, add:
   ```sh
   echo "Called with $ROOT_ID"
   ```
7. In `hello-platzi`, under **Downstream Project**, configure it to trigger `parameterized`.
8. Add a build step **Trigger/call build on other projects** and configure it with:
   ```sh
   ROOT_ID=$BUILD_NUMBER
   ```
9. Run `hello-platzi`, and verify `parameterized` executes with the correct `BUILD_NUMBER`.

**Recommended Approach:** Use **Parameterized Jobs** instead of watchers for greater flexibility.

## GitHub Integration with Jenkins

To trigger a Jenkins build on each push to a GitHub repository, follow these steps:

### Jenkins Configuration:
1. Install the **GitHub Plugin**.
2. In the Job configuration:
   - Select **Git** under SCM and provide the repository URL.
   - Ensure Git is installed on the Jenkins host.
   - Leave **Branches to build** blank to include all branches.
   - Enable **GitHub hook trigger for GITScm polling** under **Build Triggers**.

### GitHub Configuration:
1. Navigate to **Settings → Webhooks**.
2. Add a new Webhook with:
   - **Payload URL** ending in `/github-webhook/`.
   - Select **Just the push event**.

## Jenkins Pipelines

Pipelines allow defining jobs using code rather than the UI. Jenkins supports two types of Pipelines:
- **Scripted Pipelines**
- **Declarative Pipelines**

### Pipeline Concepts:
1. **Pipeline:** Represents the entire CI/CD process.
2. **Node:** A machine capable of executing pipeline tasks.
3. **Stage:** Defines a logical step in the pipeline (e.g., Build, Test, Deploy).
4. **Step:** A specific task executed within a stage.

### Creating a Pipeline Job:
- Use **Pipeline Syntax** in Jenkins UI to generate code snippets.
- Click **Replay** on a completed build to edit and rerun the pipeline script.
- Example Pipeline Stage:
  ```groovy
  stage('Run Remote') {
      steps {
          sh 'echo "Executing remote script"'
      }
  }
  ```

## Distributed Builds with Slaves

Jenkins **Slaves** enable distributed job execution. A **Master** node delegates jobs to **Slave** nodes, which can be physical or virtual machines. This setup supports **horizontal scaling**, improving build performance and resource utilization.

---

By leveraging Jenkins' automation capabilities, parameterized jobs, GitHub integration, and distributed execution, teams can efficiently manage their CI/CD pipelines.





















