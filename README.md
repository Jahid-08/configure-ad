<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Domain Controller in Azure
- Setup our client/End User in Azure
- Configure Custom DNS to point Client to Domain Controller  
- Join Client to Domain and Verify Successful Domain Registration  
- Promote Server to Domain Controller & Install Active Directory Services  
- Create Organizational Units (_EMPLOYEES, _ADMINS, _CLIENTS)  
- Create and Configure Domain Admin Account (e.g., jane_admin)  
- Grant Domain Users Remote Desktop Access to Client-1  
- Generate Bulk Users via PowerShell Script for Testing  

<h2>Deployment and Walkthrough</h2>

<p>
<img width="237" alt="image" src="https://github.com/user-attachments/assets/82c02c34-2f77-497e-880b-2e6962f0121d" />
</p>
<p>
  
  In this lab, we’ll begin by **creating two virtual machines**:

- One for the **Domain Controller (DC)**
- One for the **Client**

> 📸 _A final screenshot of the completed setup will be provided above._

---

### 🎯 Purpose of This Setup

This foundational setup lays the groundwork for more advanced tasks in:

- 🛠️ **System Administration**
- 🌐 **Network Management**
- 🛡️ **Cybersecurity**

---

### 🖥️ Domain Controller (DC)

The **Domain Controller** acts as the **central authority** for the Active Directory domain. It serves key roles such as:

- Primary **DNS server**
- **File server** for the network
- Manager of **user accounts**, **security groups**, and **Group Policies**

Essentially, the DC functions as the **core of identity and access management** in the domain.

---

### 👤 Client Machine

The **Client VM** simulates a typical **end-user workstation** in a domain environment. It allows you to test and verify:

- ✅ **User authentication**
- 📂 **File share access**
- 🌍 **DNS resolution**
- 🧩 **Group Policy application**

This setup provides hands-on experience with real-world administrative tasks and security configurations in a simulated **corporate network environment**.
<br />
<br />
<br />

<p>
<img width="870" alt="image" src="https://github.com/user-attachments/assets/79172a71-3245-4864-9611-5fb8e3b70adc" />
</p>
<p>
  
Now that we've got the initial setup complete, it's time to assign a **static IP address** to the **Domain Controller (DC)**. This is a crucial step to ensure network stability and reliable domain operations.

---

### 🔒 Why a Static IP is Important

A **Domain Controller** is like the **brain of the network** — it manages:

- **Authentication**
- **Authorization**
- **DNS services**

All other devices (clients and servers) rely on the DC to:

- Log in  
- Locate network resources  
- Join or stay connected to the domain  

---

### ⚠️ What Could Go Wrong with a Dynamic IP?

If the DC’s IP address changes (as it might with DHCP), the following issues can arise:

- ❌ Devices won't know where to **find the DC**  
- ❌ **DNS resolution** could break  
- ❌ **Domain joins** and **logins** may fail  
- ❌ **Replication** between multiple DCs (if applicable) could be disrupted  

---

### ✅ Solution: Set a Static IP

Assigning a static IP to your Domain Controller ensures:

- Consistent and reliable **network communication**  
- Proper **DNS functionality**  
- Smooth domain operations across the environment
</p>
<br />
<br />
<br />

<p>
<img width="949" alt="image" src="https://github.com/user-attachments/assets/cfa993c0-f239-461b-9684-85234c312727" />
</p>
<p>
  
## 🔁 Pointing the Client to the Domain Controller

Now that our **Client VM** is set up, it’s time to **change its DNS server** from the default one provided by Microsoft (via the VNet) to our **Domain Controller**.

---

### 🧭 Why This Matters

Updating the DNS server ensures that the Client will:

- 🔍 Look to the **Domain Controller** for **DNS resolution**
- 🔗 Be able to **resolve names to IPs** and vice versa within the domain
- 🏢 Successfully **join the domain**

This step is essential for enabling proper communication between the Client and the DC, allowing domain services like:

- ✅ Authentication  
- ✅ Group Policy application  
- ✅ File sharing  
- ✅ Network resource discovery

---

### 🛠️ What You’re Doing

You're telling the Client:  
> “From now on, ask **the Domain Controller** for directions—not Microsoft.”

This change aligns the Client with your internal network structure and prepares it for seamless domain integration.
</p>
<br />
<br />
<br />

<p>
<img width="428" alt="image" src="https://github.com/user-attachments/assets/1addfe64-e736-4f31-b0f3-eb61d5dcf3b3" />
</p>
<p>
  
  ## 📡 Testing Connectivity from Client-1 to DC-1

Now we’ll log into **Client-1** and perform two key tasks to verify connectivity and DNS configuration:

---

### 🔗 1. Ping the Domain Controller

We'll attempt to **ping the private IP address** of **DC-1** to ensure that:

- 🟢 **Network communication** is working between the Client and the Domain Controller
- ❌ There are no firewall or routing issues preventing basic connectivity

---

### 🧾 2. Verify DNS Configuration

Run the following command in Command Prompt:

**ipconfig /all**

You can see that we were able to successfuly ping 10.0.0.4(DC-1) and you can see that our DNS server is 10.0.0.4 as well.
</p>
<br />
<br />
<br />

<p>
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>
<p>
  
 ## 🛠️ Installing Active Directory on DC-1

Now we’re going to install **Active Directory** on our Domain Controller (**DC-1**).

---

### 🧠 What is Active Directory?

**Active Directory (AD)** is a **directory service** developed by Microsoft that provides a **centralized system** for:

- 🔐 Managing and securing **user accounts**
- 🖥️ Organizing and controlling access to **devices**
- 🗂️ Administering **network resources** (e.g., shared folders, printers)

---

### 🎯 Why It Matters

With AD installed on DC-1, you gain the ability to:

- ✅ Centralize identity and access management  
- ✅ Enforce security policies across the network  
- ✅ Easily scale and organize IT infrastructure in a structured way  

This is a foundational step for building a real-world **enterprise network environment**.
</p>
<br />
<br />
<br />


<p>
<img width="590" alt="image" src="https://github.com/user-attachments/assets/280b3cc4-2595-47d1-a608-ed367aa28fb2" />
</p>
<p>
  
 ## 🚀 Kicking Off the Active Directory Installation

Let’s begin the process of installing **Active Directory Domain Services (AD DS)** on **DC-1** using the **Server Manager GUI**.

---

### 🖥️ Steps to Follow

1. **Log into DC-1**  
   Server Manager should automatically launch upon login.

2. **Click on the “Manage” tab**  
   You’ll find it in the **top-right corner** of the Server Manager window.

3. Select **“Add Roles and Features”**

4. In the wizard, click **Next** through the following screens:
   - **Before You Begin**
   - **Installation Type**
   - **Server Selection**

5. When you reach the **“Server Roles”** page:
   - ✅ **Check** the box next to **Active Directory Domain Services**

6. Continue clicking **Next** through the remaining prompts:
   - **Features**
   - **AD DS**
   - **Confirmation**

7. On the **Confirmation** tab, click **Install** to begin installing the necessary AD DS components. After it's finished, click **Promote this server to a domain controller** to make **DC-1** a Domain Controller, this will then take us to our next step.

---

### 🧩 What This Does

This installs the **core components** of Active Directory needed to promote this server to a **Domain Controller**.

Once complete, we'll be ready to configure our new domain.
</p>
<br />
<br />
<br />


<p>
<img width="565" alt="image" src="https://github.com/user-attachments/assets/0fd9d836-0250-4404-8b6a-72426124f89e" />
</p>
<p>
  
## 🏰 Promoting DC-1 to a Domain Controller

With **Active Directory Domain Services (AD DS)** installed, the next step is to **promote the server** to function as a **Domain Controller**.

---

### 🚀 Promotion Process

1. In **Server Manager**, click the flag icon in the top-right corner and select:  
   **"Promote this server to a domain controller"**

2. In the **Deployment Configuration** window:
   - Select **“Add a new forest”**
   - Set the **Root domain name** to:  
     ```
     mydomain.com
     ```

3. Click **Next**, then:
   - Set a **Directory Services Restore Mode (DSRM)** password  
   - Click **Next**

4. On the **DNS Options** screen:
   - Uncheck **"Create DNS delegation"**

5. Continue clicking **Next** through the remaining options until you reach the **Prerequisites Check**

6. Once the check completes, click **Install**

   > 📎 Note: The server may **automatically restart** after installation, or prompt you to do so.

---

### 🔐 Logging in After Promotion

After rebooting, **DC-1 is now officially a Domain Controller**.

You now have two login context options:

- 👤 **Local user account** (as before)  
- 🌐 **Domain user account**

#### 🧑‍💻 Domain User Login Format

To log in as a domain user, use this format for the username:

**Example: mydomain.com\labuser**
</p>
<br />
<br />
<br />

<p>
<img width="563" alt="image" src="https://github.com/user-attachments/assets/c0002cc2-4359-4ac3-9b9f-747a3b426012" />
</p>
<p>
  
## 👤 Creating a Domain Admin User in Active Directory

Now that our Domain Controller is live, we're going to create a **Domain Admin user** by first organizing our structure with **Organizational Units (OUs)**, and then creating the user account.

---

### 🗂️ Step 1: Create Organizational Units

We'll start by creating two OUs:

- `_EMPLOYEES`
- `_ADMINS`

#### 🧭 Navigation

1. On **DC-1**, click the **Windows Start icon**
2. Go to **Windows Administrative Tools**
3. Launch **Active Directory Users and Computers**

#### 📁 Create OUs

1. In the left-hand pane, expand and **right-click** on your domain:  

In our case its **mydomain.com**

2. Choose:  
**New → Organizational Unit**

3. Name the first OU:  **_EMPLOYEES**. Do the same steps and name the second OU **_ADMINS**


---

### 🧑‍💼 Step 2: Create the Domain Admin User

We’ll now create a user named **jane_admin** inside the `_ADMINS` OU.

#### 👣 Steps

1. Inside **Active Directory Users and Computers**, navigate to the `_ADMINS` OU
2. Right-click `_ADMINS` and choose:  
**New → User**

3. Fill out the user details:
- **First name**: Jane  
- **Last name**: Doe  
- **User logon name**: `jane_admin`

4. Click **Next**, then set the password:
- **Password**: `Cyberlab123!`

5. Configure password options:
- ❌ **Uncheck**: "User must change password at next logon"  
- ✅ **Check**: "Password never expires" (for lab purposes)

6. Click **Next**, then **Finish**

---

✅ We now have a domain admin user account (`jane_admin`) created and ready for administrative tasks within our domain environment.

</p>
<br />
<br />
<br />


<p>
<img width="571" alt="image" src="https://github.com/user-attachments/assets/d0fb08ec-b9c5-496c-b873-71ec7c532e3b" />
</p>
<p>
  
 ## 🛡️ Making `jane_admin` a True Domain Admin

Just because we named the account with "admin" in the username doesn't mean it actually has administrative privileges yet. We now need to **add the account to the built-in "Domain Admins" group**.

---

### 👣 Steps to Elevate Privileges

1. In **Active Directory Users and Computers**, navigate to the `_ADMINS` OU  
2. **Right-click** on **Jane Doe** and select **Properties**  
3. Click on the **Member Of** tab  
4. Click **Add**  
5. In the text box, type: **Domain Admins** 
6. Click **Check Names** – it should underline to confirm the group exists  
7. Click **OK**, then **Apply**, then **OK** again  

---

✅ `jane_admin` is now officially a **Domain Admin**!

This means the account can:

- Create and manage users  
- Modify group memberships  
- Configure Group Policies  
- Manage the domain environment  

---

### 🔁 Log In as `jane_admin`

Now that the account has elevated privileges:

- **Log out** of your current session  
- **Log back in** using the domain credentials: **mydomain.com\jane_admin**

We are now working under a **Domain Admin context**, ready to perform administrative tasks across the domain.

</p>
<br />
<br />
<br />


<p>
<img width="691" alt="image" src="https://github.com/user-attachments/assets/816e7399-0e7c-43c8-bf9d-986f41784215" />
</p>
<p>
  
 ## 🖥️ Joining Client-1 to the Domain

Now that we're logged in as `jane_admin`, the next step is to **join Client-1 to our domain**: `mydomain.com`.

---

### 🔐 Step-by-Step: Domain Join

1. **RDP into Client-1**
   - Login using the **local account**:  
     ```
     Username: labuser  
     Password: Cyberlab123
     ```

2. **Join the Domain**
   - Right-click the **Windows icon** (bottom left)  
   - Select **System**  
   - Click **Rename this PC (Advanced)**  
   - In the **Computer Name** tab, click **Change**  
   - Under **Member of**, select **Domain**  
   - Enter:  
     ```
     mydomain.com
     ```
   - Click **OK**

3. **Authenticate with Domain Credentials**
   - A prompt will appear asking for credentials with permission to join the domain  
   - Enter:
     ```
     Username: mydomain.com\jane_admin  
     Password: Cyberlab123!
     ```

4. ✅ **Success!**
   - You should see a welcome message:  
     _“Welcome to the mydomain.com domain”_  
   - The system will prompt you to **restart**

---

Once Client-1 reboots, it will officially be a **domain-joined machine**, ready for centralized management and policy application.

</p>
<br />
<br />
<br />


<p>
<img width="564" alt="image" src="https://github.com/user-attachments/assets/566e1805-29f7-46b8-b56d-710a961238b9" />
</p>
<p>
  
 ## 🔍 Verifying Domain Join from the Domain Controller

Now, we’re going to log back into **DC-1** as `jane_admin` to **verify that Client-1 successfully joined the domain**.

---

### 🧭 Navigation Steps:

1. Right-click the **Windows icon**
2. Go to **Windows Administrative Tools**
3. Open **Active Directory Users and Computers**
4. In the left pane:
   - Double-click your domain (e.g., `mydomain.com`)
   - Click on the **Computers** container

---

### ✅ What to Look For

You should now see **Client-1** listed under **Computers**, confirming that it’s **officially part of the domain**.

</p>
<br />

<p>
<img width="562" alt="image" src="https://github.com/user-attachments/assets/8997404b-dc47-47f7-9743-9b020fb4de15" />
</p>
<p>
  
## 🗂️ Organizing Domain Objects with OUs

Now, for **organizational purposes**, we'll create a new **Organizational Unit (OU)** called **_CLIENTS**.

---

### 🛠️ Creating the _CLIENTS OU

1. Open **Active Directory Users and Computers**
2. In the left pane, right-click on your domain name (e.g., `mydomain.com`)
3. Select **New → Organizational Unit**
4. Name the new OU: `_CLIENTS`
5. Click **OK**

---

### 🖱️ Moving Client-1 into the _CLIENTS OU

1. Navigate to the **Computers** container
2. Drag **Client-1** and **drop it** into the **_CLIENTS** OU

---

📌 This step helps keep the domain environment **organized** and **easier to manage** as you scale your network.
</p>
<br />
<br />
<br />


<p>
<img width="281" alt="image" src="https://github.com/user-attachments/assets/6dd3060c-6cb5-445f-adcc-120c49dad081" />
</p>
<p>
  
  ## 🧑‍💻 Allowing Domain Users to Log into Client-1

We’re now going to configure **Client-1** so that **non-administrative domain users** can log in. This is essential for future steps where we'll be creating **thousands of users using PowerShell** and testing their ability to log in remotely.

---

### 🔐 Granting Remote Desktop Access to Domain Users

1. **Login to Client-1** using the **jane_admin** account
2. Right-click the **Windows Start icon** and select **System**
3. Navigate to **Remote Desktop**
4. Under the **User accounts** section, click:  
   **"Select users that can remotely access this PC"**
5. Click **Add**
6. Type: `Domain Users`
7. Click **Check Names**, then **OK**, and **OK** again

---

✅ Now, all **domain users** should be able to **Remote Desktop into Client-1** by default.

This sets the stage for mass user testing and remote access simulations.

</p>
<br />
<br />
<br />


<p>
<img width="773" alt="image" src="https://github.com/user-attachments/assets/a588f74d-fd8c-40b5-b607-229ee2acb953" />
</p>
<p>

 ## 🛠️ Creating Bulk Users via PowerShell Script

Now we're going to generate a large number of users for testing purposes by running a PowerShell script on **DC-1**.

---

### 🧾 Steps to Run the Script

1. **Login to DC-1**
2. Open the **Start Menu** and type `PowerShell ISE`
3. **Right-click** and select **Run as Administrator**
4. Once PowerShell ISE opens:
   - Click **File** → **New**
   - Then click **File** → **Save As** and name the file:  
     `create-users.ps1`  
     (You can save it to the **Desktop**)
5. Copy and paste the script from this GitHub link into the ISE:  
   🔗 [Generate-Names-Create-Users.ps1](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
6. Click the **green play button** at the top to run the script

---

💡 Once the script runs, you'll start seeing a bunch of users being created in the Active Directory console at the bottom of the ISE.

This step sets up a realistic lab environment by simulating a populated enterprise domain. Once this finishes, we will open up Active Directory and observe the accounts in the **_EMPLOYEES** OU and then attempt to login to **Client-1** with one of the accounts.

</p>
<br />
<br />
<br />


<p>
<img width="557" alt="image" src="https://github.com/user-attachments/assets/efaca3b3-2af7-40a6-b647-68db9bbc0786" />
</p>
<p>
  
Now if we navigate back to **Active Directory Users and Computers (ADUC)**, we can click on the **_EMPLOYEES** Organizational Unit and see all of the randomly generated users that were created by our script. These users are all part of the domain and have been successfully added to the directory.

Each of these accounts has a default password of **Password1**.

Next, we’re going to test domain login functionality by selecting one of these users and logging into **Client-1**.</p>
<br />
<br />
<br />


<p>
<img width="698" alt="image" src="https://github.com/user-attachments/assets/1b5edfc0-13d3-41b9-b126-2ab212e6577f" />
  <img width="635" alt="image" src="https://github.com/user-attachments/assets/65732096-b93b-4263-b644-16fe83e94985" />
  <img width="311" alt="image" src="https://github.com/user-attachments/assets/be547413-eaae-4625-bfab-041a069596a1" />


</p>
<p>

 ## 🔐 Group Policy: Account Lockout & Account Management

Now we are going to tackle **Group Policy** and managing accounts—like account lockouts, enabling/disabling users, and checking logs.

---

### 🧪 Step 1: Configure Account Lockout Policy

We’re going to configure a **Group Policy** so that accounts get locked after several failed login attempts.

**On DC-1:**

1. Click the **Start** menu 🔍, type `gpmc.msc`, and press **Enter**.
2. In the **Group Policy Management Console**, expand: Domains ↳ mydomain.com
3. Right-click **Default Domain Policy** and select **Edit**.
4. Navigate to: Computer Configuration ↳ Policies ↳ Windows Settings ↳ Security Settings ↳ Account Policies ↳ Account Lockout Policy
5. Configure the following settings:
- **Account lockout duration**: `30 minutes`
- **Account lockout threshold**: `5`
- **Reset account lockout counter after**: `10 minutes`
- ✅ Ensure **Allow administrator account lockout** is enabled

---

### 🔄 Step 2: Apply the Group Policy Immediately

Instead of waiting for the policy to automatically propagate, we’ll force an update.

**On Client-1 (logged in as `jane_admin`):**

1. Open **Command Prompt** (`cmd`)
2. Run the following command: gpupdate /force
This forces the client machine to immediately apply the new Group Policy settings.

---

### 🔐 Step 3: Test Account Lockout Policy

Now we’ll test the lockout feature using one of the randomly generated users from the **_EMPLOYEES** OU.

1. **Log out** of Client-1
2. Attempt to log in using incorrect passwords for a selected user
3. After 5 failed attempts, you should get a message like:
> *"The referenced account is currently locked out and may not be logged on to."*

---

### 🔓 Step 4: Unlock the User Account

Now we'll go unlock that account from the Domain Controller.

**On DC-1:**

1. Open **Active Directory Users and Computers**
2. Navigate to the **_EMPLOYEES** OU
3. Find the locked user and double-click their name
4. Go to the **Account** tab
5. You should see a message saying the account is locked out
6. ✅ Check the box to unlock the account and click **Apply**, then **OK**

Now you should be able to log in to **Client-1** using the correct password for that user.

---
</p>
<br />
<br />
<br />


<p>
<img width="383" alt="image" src="https://github.com/user-attachments/assets/7d608b36-5caf-4bd4-bb8d-9aeef84708f4" />
</p>
<p>

 ## 🔄 Resetting a User's Password in Active Directory

Sometimes users forget their passwords. As an admin, you can easily reset it.

---

### 🛠️ Step-by-Step: Reset a User Password

**On DC-1:**

1. Open **Active Directory Users and Computers**
2. Navigate to the **_EMPLOYEES** Organizational Unit (or wherever your user is located)
3. Right-click on the user account you want to reset
4. Select **Reset Password...**
5. Enter the new password  
   📝 (For example: `Password123!`)
6. ✅ Optionally check the box to **Unlock the user's account** if it was locked
7. Click **OK** to confirm

---

Now the user can log in with their new password.

</p>
<br />
<br />
<br />


<p>
<img width="387" alt="image" src="https://github.com/user-attachments/assets/e00199bf-a5d2-442c-9357-1c3f1e8b9e67" />
</p>
<p>

  ## 🚫 Enabling and Disabling User Accounts in Active Directory

Disabling an account is useful when an employee leaves the company, an account is compromised, or you need to temporarily restrict access. Let’s walk through how to disable and re-enable an account.

---

### 🔒 Disable a User Account

**On DC-1:**

1. Open **Active Directory Users and Computers**
2. Navigate to the user you want to disable (e.g., under the **_EMPLOYEES** OU)
3. Right-click the user account
4. Select **Disable Account**

✅ You’ll notice a **down arrow icon** on the user’s avatar indicating the account is disabled.

---

### 🔁 Try Logging in as the Disabled User

1. Go to **Client-1**
2. Try to log in using the disabled account credentials

❌ You’ll see an error — the user cannot log in because the account is disabled.

---

### 🔓 Re-Enable the User Account

**Back on DC-1:**

1. Right-click the same user account
2. Select **Enable Account**

The down arrow icon will disappear, and the user will now be able to log in again.

---

This is an essential skill for account lifecycle management in Active Directory 💼
</p>
<br />
<br />
<br />


<p>
<img width="953" alt="image" src="https://github.com/user-attachments/assets/ce9e29af-298e-42d5-b6e3-6d9833511a3e" />
</p>
<p>
  
## 📜 Observing Failed Login Attempts via Event Viewer

Now that we’ve set up account lockout policies and attempted multiple failed logins, it’s time to **inspect the logs** on **Client-1** to verify those activities were tracked.

---

### 🔍 View Failed Login Logs

**On Client-1 (logged in as `jane_admin`):**

1. Click the Start Menu and search for **Event Viewer**
2. In the left pane, navigate to:  
   `Windows Logs` ➡️ `Security`
3. Scroll through the logs until you see entries labeled **Audit Failure**

---

### 🧠 What to Look For

- **Event ID:** `4625`  
  This indicates a **failed login attempt**
- **Details section:**
  - **Account Name** that failed to log in
  - **Source IP Address** from where the login was attempted

This is useful for auditing login behavior and identifying possible unauthorized access attempts 🕵️‍♂️

---

### ✅ Wrap-Up

We’ve now:

- ✅ Installed and deployed Active Directory
- ✅ Created OUs, users, and groups
- ✅ Joined machines to a domain
- ✅ Configured Group Policies
- ✅ Observed login behavior through logs

This lab gave us a solid **foundation in AD and GPO**, setting the stage for more advanced configurations in the future 💪
</p>
<br />
