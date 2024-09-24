# Addendum : Troubleshooting

## Introduction
This addendum contains additional information and troubleshooting guides. 


## Trouble 1: Putty configuration 
Steps to configure putty connection to OCI compute instances.
1. Open putty

2. Insert the public IP of your server and a mnemonic session name
    ![MYSQLEE](images/putty-config.png "putty config")

3. Choose "Connection→SSH→Auth" and provide the id_rsa.ppk path
    ![MYSQLEE](images/putty-auth.png "putty auth")

4. Select "Connection→Data" and insert "opc" in "Auto-login username"
    ![MYSQLEE](images/putty-data.png "putty-data")

5. Choose Connection and insert "60" in "Seconds between keepalives"
    ![MYSQLEE](images/putty-connection.png "putty connection")

6. Return to "Session" and click save
    ![MYSQLEE](images/putty-save.png "putty save")


## Trouble 2: Windows Powershell SSH error on permissions 
1. In case you are using powershell and you receive an error like
    
    ![MYSQLEE](images/powershell-permissions-error.png "powershell permissions error")

    Please use below commands to fix the permissions (please change full path with your path (e.g. New-Variable -Name Key -Value "C:\mysql\id_rsa"))

    ```
    <span style="color:green">powershell></span><copy>New-Variable -Name Key -Value "<full path>\id_rsa"</copy>
    ```

    Verify that ls command retrieve the right file:
    ```
    <span style="color:green">powershell></span><copy>ls $Key</copy>
    ```
    ```
    <span style="color:green">powershell></span><copy>Icacls $Key /c /t /Inheritance:d</copy>
    ```
    ```
    <span style="color:green">powershell></span><copy>Icacls $Key /c /t /Grant ${env:UserName}:F</copy>
    ```
    ```
    <span style="color:green">powershell></span><copy>TakeOwn /F $Key</copy>
    ```
    ```
    <span style="color:green">powershell></span><copy>Icacls $Key /c /t /Grant:r ${env:UserName}:F</copy>
    ```
    ```
    <span style="color:green">powershell></span><copy>Icacls $Key /c /t /Remove:g Administrator "Authenticated Users" BUILTIN\Administrators BUILTIN Everyone System Users</copy>
    ```
    ```
    <span style="color:green">powershell></span><copy>Icacls $Key</copy>
    ```
    ```
    <span style="color:green">powershell></span><copy>Remove-Variable -Name Key</copy>
    ```

    Then retry the ssh connection.



## Acknowledgements
* **Author** - Marco Carlessi, Principal Sales Consultant
* **Contributors** -  Perside Foster, MySQL Solution Engineering, Selena Sánchez, MySQL Solutions Engineer
* **Last Updated By/Date** - Selena Sánchez, MySQL Solution Engineering, May 2023