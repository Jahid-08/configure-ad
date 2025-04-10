<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

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
- Step 3
- Step 4

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

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
