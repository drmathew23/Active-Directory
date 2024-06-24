# Active Directory Lab Setup in Azure

## Overview
This lab demonstrates the setup and configuration of an Active Directory (AD) environment in Azure. The lab includes the creation of a Windows Server 2019 VM as a Domain Controller, a Windows 10 VM as a client machine, the creation of users using PowerShell, and the connection of the Windows 10 VM to the AD domain.

## Step-by-Step Implementation

### 1. Setting Up the Azure Environment
- Provisioned Virtual Machines:
  - Windows Server 2019 VM: Configured as the Domain Controller (DC).
  - Windows 10 VM: Configured as the client machine.
- Allocated appropriate resources (CPU, RAM, storage) based on workload requirements.

### 2. Configuring the Windows Server 2019 VM
- **Promoting to Domain Controller:**
  - Logged into the Windows Server 2019 VM using Remote Desktop Protocol (RDP).
  - Installed the AD DS role:
    ```powershell
    Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
    ```
  - Promoted the server to a Domain Controller:
    ```powershell
    Install-ADDSForest -DomainName "example.local" -DomainNetbiosName "EXAMPLE" -SafeModeAdministratorPassword (ConvertTo-SecureString "YourPassword" -AsPlainText -Force)
    ```
  - Restarted the server to apply changes.

### 3. Configuring the Windows 10 VM
- **Joining the Domain:**
  - Logged into the Windows 10 VM using RDP.
  - Joined the Windows 10 VM to the AD domain:
    - Opened Settings > System > About > Join a domain.
    - Entered the domain name `example.local` and provided domain administrator credentials.
  - Restarted the Windows 10 VM to apply changes.

### 4. Creating Users Using PowerShell
- **User Creation Script:**
  - Logged into the Domain Controller.
  - Created a PowerShell script to automate AD user creation:
    ```powershell
    Import-Module ActiveDirectory

    $users = @(
        @{FirstName="John"; LastName="Doe"; Username="jdoe"; Password="P@ssword1"},
        @{FirstName="Jane"; LastName="Smith"; Username="jsmith"; Password="P@ssword2"},
        @{FirstName="Robert"; LastName="Johnson"; Username="rjohnson"; Password="P@ssword3"}
    )

    foreach ($user in $users) {
        $password = ConvertTo-SecureString $user.Password -AsPlainText -Force
        New-ADUser -GivenName $user.FirstName -Surname $user.LastName -Name "$($user.FirstName) $($user.LastName)" -SamAccountName $user.Username -UserPrincipalName "$($user.Username)@example.local" -AccountPassword $password -Enabled $true -Path "OU=Users,DC=example,DC=local"
    }
    ```
  - Executed the script to create user accounts in AD.

### 5. Verifying the Configuration
- **Testing User Logins:**
  - Logged into the Windows 10 VM using the newly created AD user accounts to verify successful domain integration.
  - Checked user properties and group memberships in Active Directory Users and Computers (ADUC).

## Results
The lab setup successfully demonstrated:
- Deployment of Windows Server 2019 and Windows 10 VMs in Azure.
- Configuration of the Windows Server 2019 VM as an AD Domain Controller.
- Joining the Windows 10 VM to the AD domain.
- Automated creation of AD users using PowerShell.
- Successful domain logins by the created users, confirming proper integration and functionality.

## Conclusion
This lab showcases the ability to design and implement a fully functional Active Directory environment in Azure, leveraging PowerShell for automation and efficient management. It underscores proficiency in cloud-based infrastructure, Active Directory services, and scripting for administrative tasks.
