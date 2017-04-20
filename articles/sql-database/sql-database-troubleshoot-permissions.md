<properties
    pageTitle="How to do admin tasks, e.g. reset admin password | Microsoft Azure"
    description="Describes how to perform common administrative tasks in SQL Database. For example, resetting admin password, granting and removing access."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="reset admin password"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>How to perform common administrative tasks such as resetting admin password in Azure SQL Database
Use this topic for quick steps to grant and remove access to an Azure SQL database. For more comprehensive information, see:

- [Managing databases and logins in Azure SQL Database](sql-database-manage-logins.md)
- [Securing your SQL database](sql-database-security.md)
- [Security Center for SQL Server Database Engine and Azure SQL Database](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>To reset admin password for a logical server

- In the [Azure Portal](https://portal.azure.com) click **SQL Servers**, select the server from the list, and then click **Reset Password**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>To help make sure only authorized IP addresses are allowed to access the server
- See [How to: Configure firewall settings on SQL Database](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>To create contained database users in the user database
- Use the [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) statement and see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>To authenticate contained database users by using your Azure Active Directory
- See [Connecting to SQL Database By Using Azure Active Directory Authentication](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>To create additional logins for high-privileged users in the virtual master database
- Use the [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx) statement, and see the Managing Logins section of [Managing databases and logins in Azure SQL Database](sql-database-manage-logins.md) for more detail.
