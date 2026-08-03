# Windows Server RRAS VPN Deployment and Connectivity Troubleshooting

**Remote Access Role | RRAS Configuration | Active Directory Authorization | NAT and Firewall Troubleshooting**

## Project Overview

I configured Windows Server 2019 Routing and Remote Access Service (RRAS) to accept remote-access VPN connections. The lab included installing the Remote Access role, enabling VPN access, configuring IPv4 address assignment through DHCP, granting a domain user dial-in permission, reviewing host-firewall access, creating a router port-forwarding rule, and building a Windows VPN client profile.

The client connection did not complete successfully. I documented the failed test and analyzed the likely network-path issues instead of presenting the deployment as a working production VPN.

> **Project status:** Server-side and client-side configuration completed. End-to-end VPN connectivity remained unresolved and access to internal resources was not validated.

## Business Scenario

An organization needs to provide an authorized user with remote access to resources on an internal Windows domain network. The remote client must reach a public VPN endpoint, pass through the edge router, authenticate with an approved domain account, and receive an internal IPv4 configuration from the RRAS server.

This lab was designed to model that connection path and identify the controls required at each layer.

## Connection Path

`Remote Windows Client → Internet → Home Router/NAT → RRAS Server → Internal Domain Network`

| Component | Configuration |
| --- | --- |
| Active Directory domain | `adeniyi.com` |
| RRAS server | `ADDS-SERVER` |
| Server FQDN | `ADDS-SERVER.adeniyi.com` |
| Server IPv4 address | `192.168.10.10` |
| Server operating system | Windows Server 2019 Standard Evaluation |
| Remote Access role service | DirectAccess and VPN (RAS) |
| RRAS configuration | Custom configuration with VPN access |
| VPN client address assignment | DHCP |
| Lab VPN protocol | PPTP |
| PPTP control channel | TCP `1723` |
| PPTP tunnel traffic | GRE, IP protocol `47` |
| Windows client profile | `AD-Connection` |
| Public endpoint | Redacted from this documentation |

## Project Objectives

- Install the Windows Server Remote Access role.
- Enable the DirectAccess and VPN (RAS) role service.
- Configure and start Routing and Remote Access Service.
- Enable remote-access VPN functionality.
- Configure IPv4 forwarding and DHCP-based client addressing.
- Grant a domain user permission to request remote access.
- Review Windows Defender Firewall access for the Remote Access service.
- Forward PPTP traffic from the edge router to the RRAS server.
- Create a Windows built-in VPN client profile.
- Test the remote connection and document the unsuccessful result.
- Develop a structured troubleshooting plan for the failed tunnel.

## Skills Demonstrated

- Windows Server role installation
- Routing and Remote Access Service administration
- Remote-access VPN architecture
- Active Directory user authorization
- IPv4 forwarding and DHCP integration
- Windows Defender Firewall review
- NAT and port-forwarding configuration
- Windows VPN client configuration
- Layered network troubleshooting
- Security and implementation documentation

## Implementation

### 1. Selected the Windows Server

In **Server Manager**, I opened **Manage > Add Roles and Features** and selected the target server:

| Field | Value |
| --- | --- |
| Server | `ADDS-SERVER.adeniyi.com` |
| IPv4 address | `192.168.10.10` |
| Operating system | Windows Server 2019 Standard Evaluation |

<p align="center">
  <img src="https://i.imgur.com/LHiCv5x.png" width="700" alt="Selecting ADDS-SERVER as the destination server">
</p>

### 2. Installed the Remote Access Role

I selected the **Remote Access** server role and enabled the **DirectAccess and VPN (RAS)** role service. The wizard added the supporting Remote Access management tools.

<p align="center">
  <img src="https://i.imgur.com/XpW7Mmd.png" width="700" alt="Selecting the Remote Access server role">
</p>

<p align="center">
  <img src="https://i.imgur.com/OY7171I.png" width="700" alt="Selecting the DirectAccess and VPN RAS role service">
</p>

The installation completed successfully on `ADDS-SERVER`.

<p align="center">
  <img src="https://i.imgur.com/35mQwTR.png" width="700" alt="Remote Access role installation completed successfully">
</p>

### 3. Enabled Routing and Remote Access

I opened **Routing and Remote Access**, right-clicked the local server, and selected **Configure and Enable Routing and Remote Access**.

I chose **Custom configuration** so that only the required RRAS service could be enabled.

<p align="center">
  <img src="https://i.imgur.com/fAC3ZX2.png" width="700" alt="Selecting a custom RRAS configuration">
</p>

I selected **VPN access**, completed the wizard, and started the Routing and Remote Access service.

<p align="center">
  <img src="https://i.imgur.com/9f1DLVm.png" width="700" alt="Enabling VPN access in the RRAS configuration wizard">
</p>

<p align="center">
  <img src="https://i.imgur.com/Gshob4N.png" width="700" alt="Starting the Routing and Remote Access service">
</p>

### 4. Configured IPv4 Address Assignment

In the RRAS server properties, I opened the **IPv4** tab and configured:

| Setting | Selection |
| --- | --- |
| IPv4 forwarding | Enabled |
| VPN client address assignment | Dynamic Host Configuration Protocol (DHCP) |
| Network adapter | Allow RAS to select an adapter |

<p align="center">
  <img src="https://i.imgur.com/IOs6k8e.png" width="700" alt="Configuring RRAS IPv4 forwarding and DHCP address assignment">
</p>

This configuration instructs RRAS to obtain addresses for connected VPN clients through DHCP rather than from an RRAS static address pool.

### 5. Granted a Domain User Remote-Access Permission

In **Active Directory Users and Computers**, I opened the selected lab user's properties. On the **Dial-in** tab, I set **Network Access Permission** to **Allow access**.

<p align="center">
  <img src="https://i.imgur.com/QJHLbzH.png" width="700" alt="Granting an Active Directory lab user dial-in permission">
</p>

This per-user permission was suitable for a small lab. A larger environment should normally control VPN authorization through Network Policy Server policies and security-group membership.

### 6. Reviewed the Host Firewall Configuration

I verified that **Remote Access** appeared in the Windows Defender Firewall allowed-app list.

<p align="center">
  <img src="https://i.imgur.com/POsD7T6.png" width="700" alt="Remote Access listed in Windows Defender Firewall allowed apps">
</p>

For a production deployment, firewall access should be limited to the required protocol, interface, and network profile instead of being opened more broadly than necessary.

### 7. Created the Router Port-Forwarding Rule

On the edge router, I created a NAT rule that forwarded PPTP's TCP control connection to the RRAS server:

| Field | Value |
| --- | --- |
| Internal server | `192.168.10.10` |
| Protocol | TCP |
| Public port | `1723` |
| Private port | `1723` |

<p align="center">
  <img src="https://i.imgur.com/KrkkyiE.png" width="700" alt="Router rule forwarding TCP port 1723 to the RRAS server">
</p>

This rule addressed the PPTP control channel only. PPTP also requires the router and firewall path to pass **GRE IP protocol 47**, which is not a TCP or UDP port.

### 8. Created the Windows VPN Client Profile

On the Windows client, I created a built-in VPN profile with the following settings:

| Setting | Value |
| --- | --- |
| VPN provider | Windows (built-in) |
| Connection name | `AD-Connection` |
| Server name or address | Public endpoint, redacted |
| VPN type | Point-to-Point Tunneling Protocol (PPTP) |
| Sign-in method | Username and password |

The public IP address and user-identifying fields have been intentionally omitted from this README.

<p align="center">
  <img src="https://i.imgur.com/Fz7W7IY.png" width="700" alt="Windows VPN settings showing the AD-Connection profile">
</p>

### 9. Tested the Connection

I attempted to connect using `AD-Connection`. Windows reported that the network connection between the client and VPN server was interrupted.

<p align="center">
  <img src="https://i.imgur.com/RRz4qX1.png" width="700" alt="Windows VPN client showing an unsuccessful AD-Connection attempt">
</p>

An external TCP test also reported that port `1723` was not reachable. That result showed that the PPTP control channel was not externally accessible at the time of testing, but it did not identify the exact cause and could not validate GRE traffic.

## Troubleshooting Analysis

### What Was Confirmed

- The Remote Access role installed successfully.
- RRAS was configured for VPN access and the service was started.
- IPv4 forwarding and DHCP-based client address assignment were selected.
- A lab domain user was granted dial-in permission.
- The Windows host firewall contained a Remote Access entry.
- The router contained a TCP `1723` forwarding rule to `192.168.10.10`.
- A Windows PPTP client profile was created.
- The connection attempt produced an error before a usable tunnel was established.

### What Was Not Confirmed

- TCP `1723` reaching the RRAS service from the public internet.
- GRE IP protocol `47` passing through the router and upstream network.
- PPTP passthrough support on the router.
- Whether the public address was directly assigned to the router or placed behind another NAT layer.
- Successful domain authentication through the VPN tunnel.
- Assignment of an internal VPN client address.
- Access to DNS, file shares, Remote Desktop, or other internal resources.

### Likely Investigation Areas

| Area | Why it matters |
| --- | --- |
| TCP `1723` reachability | PPTP uses this connection for tunnel control |
| GRE IP protocol `47` | PPTP carries tunneled data through GRE, which a TCP-only forwarding rule does not prove is working |
| Router PPTP passthrough | Some NAT devices do not handle GRE mapping correctly without explicit PPTP support |
| Double NAT or carrier-grade NAT | A second upstream NAT device can prevent unsolicited inbound traffic from reaching the home router |
| NAT loopback | Testing the public address from the same internal network can fail when the router does not support hairpin NAT |
| Server listening state | RRAS must be running and listening for PPTP control connections |
| Windows and edge firewalls | Both the local server and upstream path must permit the required traffic |
| External test location | The tunnel should be tested from a genuinely separate network, such as a mobile hotspot |

The evidence does not prove that the ISP alone caused the failure. A complete diagnosis would need to verify each network layer and confirm whether GRE traffic reached the server.

## Recommended Diagnostic Commands

Run these checks on the RRAS server:

```powershell
# Confirm that Routing and Remote Access is running
Get-Service -Name RemoteAccess

# Check whether the server is listening for PPTP control traffic
Get-NetTCPConnection -LocalPort 1723 -State Listen

# Review firewall rules related to Remote Access
Get-NetFirewallRule |
    Where-Object DisplayName -Like "*Routing and Remote Access*" |
    Select-Object DisplayName, Enabled, Direction, Action

# Display IPv4 configuration and routing information
Get-NetIPConfiguration
route print
```

Run this from a client on a separate external network:

```powershell
# Tests only the TCP 1723 control channel, not GRE
Test-NetConnection -ComputerName <vpn-endpoint> -Port 1723
```

Additional checks should include:

- Compare the router's WAN address with the current public address.
- Confirm whether the router supports PPTP passthrough and GRE forwarding.
- Check for an ISP gateway or second router creating double NAT.
- Review RRAS and RemoteAccess events in Event Viewer during a connection attempt.
- Capture traffic on the server and check for TCP `1723` and GRE packets.
- Test from a mobile hotspot or another external network.

## Validation Results

| Validation item | Result |
| --- | --- |
| Remote Access role installed | Passed |
| DirectAccess and VPN (RAS) role service installed | Passed |
| RRAS configured for VPN access | Passed |
| Routing and Remote Access service started | Passed |
| DHCP selected for VPN client addressing | Passed |
| Domain user granted dial-in access | Passed |
| Host firewall Remote Access entry verified | Passed |
| Router TCP `1723` rule created | Passed |
| Windows VPN profile created | Passed |
| TCP `1723` externally reachable | Failed during captured test |
| End-to-end VPN tunnel established | Not completed |
| VPN client received an internal address | Not validated |
| Internal network resource access | Not validated |

## Security and Design Considerations

- PPTP was used only to study a legacy VPN configuration. Microsoft does not recommend PPTP because it lacks modern security features.
- Use IKEv2 or SSTP for a current Windows remote-access deployment.
- Use certificate-based authentication and stronger access controls where possible.
- Manage user authorization with NPS policies and dedicated security groups instead of individual account settings at scale.
- Do not publish public IP addresses, account names, passwords, router administration details, or unredacted configuration screenshots.
- Place RRAS on a dedicated member server or appropriately segmented remote-access host rather than increasing the attack surface of a domain controller.
- Restrict inbound firewall rules to the required protocols and interfaces.
- Keep the VPN server, router, and clients patched and monitor authentication and RRAS logs.
- Consider multifactor authentication for production remote access.

## Recommended Next Implementation

To complete this project as a modern remote-access deployment, I would:

1. Deploy RRAS on a dedicated domain member server.
2. Replace PPTP with IKEv2 or SSTP.
3. Configure a server certificate and certificate-based authentication.
4. Create an NPS policy tied to a dedicated VPN users security group.
5. Configure the required edge-firewall rules for the selected protocol.
6. Test from a separate external network.
7. Validate the assigned VPN address with `ipconfig /all`.
8. Confirm access to an approved internal resource.
9. Record relevant RRAS, NPS, and firewall logs.
10. Capture final evidence without exposing public addresses or credentials.

## Key Takeaways

This lab provided hands-on experience with Windows Server RRAS, Active Directory remote-access permissions, Windows Firewall, NAT, and VPN client configuration. It also demonstrated why successful role installation does not guarantee end-to-end connectivity.

The most valuable result was the troubleshooting lesson: PPTP requires both TCP `1723` and GRE IP protocol `47`, a TCP port test cannot validate the full tunnel path, and an unsuccessful connection should be documented accurately until authentication, address assignment, and internal resource access have all been verified.


## Related Projects

- [Active Directory Implementation](https://github.com/AdeniyiAdesakin/Active-Directory-Implementation)
- [Windows Server DNS Configuration and Administration](https://github.com/AdeniyiAdesakin/DNS-Configuration)
- [Windows Server DHCP Deployment and Client Lease Validation](https://github.com/AdeniyiAdesakin/DHCP-Installation-and-Configuration)

