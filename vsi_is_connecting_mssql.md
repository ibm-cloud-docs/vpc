---

copyright:
  years: 2026, 2026
lastupdated: "2026-06-30"

keywords: connecting, windows, sql, mssql

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to Windows MSSQL instances
{: #vsi_is_connecting_mssql}

After you create your Microsoft SQL virtual server instance, you can connect to it by completing these steps.
{: shortdesc}

After you create a virtual server instance with a Microsoft SQL stock image, you need to reset the default sqlserver ‘sa’ account password.

1. Log in to the MSSQL server.
1. Open an elevated Windows PowerShell prompt and run the following commands:

   ```text
   net stop mssqlserver
   net start mssqlserver /mSQLCMD
   SQLCMD
   LOGIN sa WITH PASSWORD = ‘<sa-account-password>’
   GO
   QUIT
   net stop mssqlserver
   net start mssqlserver
   ```
   {: codeblock}

1. Connect to the sql-server instance by using SQL-Server-Based Authentication with the username ‘sa’ and the password that you created. Then, you can add the Windows user with the following command:

   ```sh
   Expand DataBase Object -> Security -> Login -> Right Click -> New Login -> Login Name -> Domain/Administrator with Windows Authentication -> OK new ssms
   Connect using Windows Admin with Windows authentication
   ```
   {: codeblock}

   Domain refers to the hostname, so you need to replace the domain with the hostname. Then, you can log in with your Windows authentication.
   {: note}

For more information, see [Connect to SQL Server when system administrators are locked out](https://learn.microsoft.com/en-us/sql/database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out?view=sql-server-ver17){: external}.
