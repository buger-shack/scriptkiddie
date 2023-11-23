# Basics

## What is it ?

> **SCCM** (System Center Configuration Manager) or **MECM** (Microsoft Endpoint Configuration Manager) now, is a Microsoft on Premise Management Centralized Tool, used to manage servers, workstations in large Active Directory environments. Lots of features such as remote control, patch management, tasks automation, application distribution, hardware and software inventory, compliance management and security policy administration. New alternative tool : **Intune** (cloud)

## Crash Course

> **Site** The systems that make up an SCCM environment. Each site is identified by a three character site code (e.g., PS1).

> **Client** The systems that are joined to, managed by, and receive content from an SCCM site.

> **Primary Site** A site that clients are assigned to and that is administered using the Configuration Manager console.

> **Primary Site Server** The server responsible for processing client-generated data and interacting with the site database. Also referred to as the site server.

> **Site System** A computer that is assigned one or more site system roles in the site.

> **SMS Provider** A site system with Windows Management Instrumentation (WMI) and HTTPS REST API providers that allow indirect access to the site database. This role is installed on the primary site server by default but can also be installed elsewhere. SharpSCCM interacts with SMS providers via WMI and HTTP(S).

> **Management Point** A site system that receives client configuration data, forwards it to the site server to be processed, and responds to client requests for policy and service locations. SharpSCCM interacts with management points via HTTP(S).

> **Discovery Methods** Configurable methods the site uses to discover computers.

> **Boundary Group** Network locations (e.g., IP subnets/address ranges, Active Directory sites) that include client systems managed by the site.

> **Automatic Site Assignment** A setting that automatically assigns systems discovered in a specific boundary group to a specific site (e.g., all systems discovered in AD domain X are automatically assigned to site Y). This setting must be configured by the SCCM admin to enable automatic site-wide client push installation.

> **Client Push Installation** A method for deploying the SCCM client software where the site server connects to a machine’s ADMIN$ share, copies over the files needed for installation, and executes the installer (ccmsetup.exe). By default, this connection occurs over SMB, but can occur over HTTP if WebClient is enabled on the site server and the target machine’s NetBIOS name is set to a value that specifies a port number (e.g., machine@8080).

> **Client Push Installation Accounts** The list of accounts that the site server tries to authenticate with to install the client. By default, if none of the configured accounts can successfully authenticate, or if no accounts are configured, the site server attempts to authenticate with its machine account.

> **Automatic Site-wide Client Push Installation** When automatic site assignment and this setting are enabled, the site automatically tries client push installation on any computers it discovers within a boundary group. This option is not enabled by default.

> **Allow connection fallback to NTLM** A client push installation setting that allows the server to attempt NTLM authentication when Kerberos authentication fails. This option is enabled by default.

> **Registration Request** A message sent to the management point to register a new client with the site.
