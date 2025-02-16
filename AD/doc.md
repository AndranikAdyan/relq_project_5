# Windows Server and Active Directory (AD) Setup Guide

## Step 1: Assign a Static IP, Configure Hostname, and Update Windows Server

### Assign a Static IP Address
1. Open **Server Manager**.
2. Click **Local Server**.
3. Click the **Ethernet** link next to the network adapter.
4. In the **Network Connections** window, right-click the active network adapter and select **Properties**.
5. Select **Internet Protocol Version 4 (TCP/IPv4)** and click **Properties**.
6. Choose **Use the following IP address** and enter:
   - **IP Address**: If your IP looks like 192.168.123.x write 192.168.123.100 or any number in the end.
   - **Subnet Mask**: (255.255.255.0)
   - **Default Gateway**: (e.g., 192.168.123.1)
   - **Preferred DNS Server**: (Set to the same IP as the server, e.g., 192.168.123.10)
7. Click **OK**, then **Close**.

### Change Hostname
1. Open **Server Manager** > Click **Local Server**.
2. Click the **Computer name** link.
3. In the **System Properties** window, click **Change**.
4. Enter the new computer name (e.g., **AD-Server**), click **OK**.

### Update Windows Server
1. Open **Settings** > **Update & Security**.
2. Click **Check for updates**.
3. Install available updates and restart the server.

---

## Step 2: Install AD Domain Services Role
1. Open **Server Manager**.
2. Click **Manage** > **Add Roles and Features**.
3. Select **Role-based or feature-based installation**, click **Next**.
4. Choose **Select a server from the server pool**, click **Next**.
5. Select **Active Directory Domain Services**.
6. Click **Add Features** when prompted.
7. Click **Next**, then **Install**.

---

## Step 3: Promote Server to Domain Controller
1. Open **Server Manager**.
2. Click the **Notification flag** and select **Promote this server to a domain controller**.
3. Select **Add a new forest**, enter **mydomain.local**, click **Next**.
4. Choose **Forest Functional Level** and **Domain Functional Level** (default is fine).
5. Set **Directory Services Restore Mode (DSRM) password**.
6. Click **Next** through all default options.
7. Review settings and click **Install**.

---

## Step 4: Configure DNS and Setup a Forest
1. Open **Server Manager** > Click **Tools** > **DNS**.
2. Expand the server name, right-click **Forward Lookup Zones**, select **New Zone**.
3. Choose **Primary zone**, click **Next**.
4. Enter **mydomain.local**, click **Next**.
5. Choose **Allow only secure dynamic updates**, click **Next**.
6. Click **Finish**.

---

## Step 5: Create Organizational Units (OUs)
1. Open **Server Manager** > Click **Tools** > **Active Directory Users and Computers**.
2. Right-click **mydomain.local** > **New** > **Organizational Unit**.
3. Enter **IT**, click **OK**.
4. Repeat for other OUs (e.g., **Finance**).

---

## Step 6: Create User Accounts and Security Groups
### Create Users
1. Open **Active Directory Users and Computers**.
2. Navigate to the desired OU (e.g., IT).
3. Right-click the OU > **New** > **User**.
4. Enter details:
   - **First name**: Admin
   - **User logon name**: admin@mydomain.local
5. Set a password, uncheck **User must change password at next logon**, click **Next**, then **Finish**.
6. Repeat for **standarduser**.

### Create Security Groups
1. In **Active Directory Users and Computers**, right-click the OU > **New** > **Group**.
2. Enter the group name (e.g., IT_Admins), click **OK**.
3. Open the group properties, go to **Members**, click **Add**, enter users, and click **OK**.

---

## Step 7: Apply Group Policy Settings
1. Open **Group Policy Management** (Server Manager > Tools).
2. Expand **Forest** > **Domains** > **mydomain.local**.
3. Right-click **Default Domain Policy**, select **Edit**.
4. Configure security settings under:
   - **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings**.
   - Example: **Password Policy**, **Account Lockout Policy**.
5. Click **OK**, then close **Group Policy Management Editor**.
6. Apply policy by running `gpupdate /force` in **Command Prompt**.
