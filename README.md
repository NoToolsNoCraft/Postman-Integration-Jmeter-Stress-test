## **GitHub Actions Workflow: Postman Functional/Integration + JMeter Stress Performance Test**

This GitHub Actions workflow is designed to run **functional/integration tests** using **Postman** and **stress/performance tests** using **JMeter**. Below is a breakdown of the workflow and its steps.

### **Workflow Trigger**
- **Trigger:** This workflow is triggered manually via `workflow_dispatch`. It needs to be initiated by a user through the GitHub Actions interface.

---

### **Jobs**

#### **`api-tests` Job:**
- **Runs on:** `ubuntu-latest` (virtual environment used for this job).

---

### **Steps**

1. **Checkout Repository:**
   - Uses `actions/checkout@v3` to check out the repository’s code, ensuring the job works with the latest code in the repository.

2. **Set Up Node.js:**
   - Uses `actions/setup-node@v3` to set up Node.js version `16.x`, which is required to run the Postman tests (Newman depends on Node.js).

3. **Install Dependencies:**
   - Runs `npm ci` to install dependencies based on the `package-lock.json` file, ensuring the necessary packages are available for the Postman tests.

4. **Run Postman Test:**
   - Executes the Postman collection `Retrieve all items in the database.postman_collection.json` using **Newman** (the command-line tool for Postman). This step is for running **functional/integration tests** on the API.

5. **Install OpenJDK 8:**
   - Installs **OpenJDK 8** via `sudo apt-get install openjdk-8-jdk`, which is necessary for running JMeter (a Java-based tool).

6. **Download and Install JMeter 5.5:**
   - Downloads the **JMeter 5.5** binary archive from Apache's website and extracts it.
   - Adds the JMeter binary path (`$PWD/apache-jmeter-5.5/bin`) to the GitHub Actions environment (`$GITHUB_PATH`) to make JMeter commands available in subsequent steps.

7. **Configure JMeter XStream Security:**
   - Configures XStream in JMeter to allow additional security settings, specifically permitting the class `org.apache.jmeter.save.ScriptWrapper`.

8. **List JMeter Directory:**
   - Runs `ls -R apache-jmeter-5.5` to list the files in the JMeter installation directory, ensuring the correct files are present.

9. **Run JMeter Tests:**
   - Runs JMeter in **non-GUI mode** (`-n`) with the test plan `Jmeter/API_Stress_Testing.jmx`, storing the results in a JTL file (`results.jtl`). This step performs **stress/performance testing** on the API.

---

### **Summary**
This workflow automates the process of:
1. Running **functional and integration tests** on an API using **Postman** (via Newman).
2. Performing **stress and performance testing** on the API using **JMeter**.

The workflow ensures both the correctness and scalability of the API are tested automatically.
