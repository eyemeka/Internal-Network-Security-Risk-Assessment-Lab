# Internal Network Security & Risk Assessment Lab #

# <h2>1. Project Overview</h2> #
This project involved conducting an internal network security and risk assessment of a Windows 11 workstation in an isolated virtual lab environment.

The assessment combined asset discovery, vulnerability scanning, network and service enumeration, security control validation, risk assessment, and risk treatment planning.

The Windows 11 virtual machine was intentionally configured with outdated third-party software, including Mozilla Firefox 2.0.0.11 and VLC Media Player 2.2.1, to provide realistic vulnerability-assessment scenarios.

The assessment was performed using Kali Linux, Nmap, and Tenable Nessus Essentials.

# <h2>2. Objectives</h2> #
The main objectives were to:
<ul>
 	<li>Identify hosts within the internal network.</li>
 	<li>Build an asset inventory for the assessment scope.</li>
 	<li>Identify vulnerabilities affecting the Windows 11 workstation.</li>
 	<li>Perform both unauthenticated and authenticated vulnerability assessments.</li>
 	<li>Enumerate network services and exposed ports.</li>
 	<li>Validate relevant security controls.</li>
 	<li>Assess identified risks based on likelihood and impact.</li>
 	<li>Develop appropriate risk treatment recommendations</li>
</ul>

## <h2>3. Tools Used</h2> ##
<ul>
 	<li>VirtualBox</li>
 	<li>Kali Linux</li>
 	<li>Windows 11</li>
 	<li>Tenable Nessus Essentials</li>
 	<li>Nmap</li>
</ul>

## <h2>4. Scope and Lab Environment</h2> ##
The assessment was conducted in an isolated VirtualBox environment.
### <h3>In-Scope Assets</h3> ###
<table>
<thead>
<tr>
<th>Asset</th>
<th>Operating System</th>
<th>IP Address</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kali Linux</td>
<td>Kali Linux</td>
<td>192.168.56.10</td>
<td>Up</td>
</tr>
<tr>
<td>Windows Workstation</td>
<td>Windows 11</td>
<td>192.168.56.20</td>
<td>Up</td>
</tr>
</tbody>
</table>
A separate Host-Only network was used to provide the Nessus scanning path to the Windows 11 workstation.
<h3>Network Configuration</h3>
<table>
<thead>
<tr>
<th>Device</th>
<th>Interface</th>
<th>Network</th>
<th>IP Address</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kali Linux</td>
<td>eth0</td>
<td>Internal Network</td>
<td>192.168.56.10/24</td>
</tr>
<tr>
<td>Windows 11</td>
<td>Ethernet</td>
<td>Internal Network</td>
<td>192.168.56.20/24</td>
</tr>
<tr>
<td>Kali Linux</td>
<td>eth1</td>
<td>Host-Only</td>
<td>192.168.57.10/24</td>
</tr>
<tr>
<td>Windows Host</td>
<td>VirtualBox Host-Only Adapter</td>
<td>Host-Only</td>
<td>192.168.57.1/24</td>
</tr>
<tr>
<td>Windows 11</td>
<td>Ethernet 2</td>
<td>Host-Only</td>
<td>192.168.57.20/24</td>
</tr>
</tbody>
</table>
Nessus Essentials was running on the Windows host and accessed through the Nessus web interface.<code></code>

## <h2>5. Network Architecture</h2> ##
The laboratory environment was designed to isolate the assessment from the external network.

Kali Linux was used as the security assessment workstation for Nmap-based discovery and enumeration.

The Windows 11 workstation was the primary assessment target.

The Host-Only network provided connectivity between the Windows host, Kali Linux, and the Windows 11 target for vulnerability scanning.

## <h2>6. Asset Discovery and Inventory</h2> ##
The first assessment activity was network discovery using Nmap.

The following command was used:
<pre><code class="language-bash">sudo nmap -sn 192.168.56.0/24
</code></pre>
The scan identified two active hosts:
<ul>
 	<li><code>192.168.56.10</code> as the Kali Linux assessment system.</li>
 	<li><code>192.168.56.20</code> as the Windows 11 workstation.</li>
</ul>
The Windows host was identified with a VirtualBox virtual network adapter.
<h3>Asset Inventory</h3>
<table>
<thead>
<tr>
<th>Asset ID</th>
<th>Host</th>
<th>IP Address</th>
<th>Operating System</th>
<th>Status</th>
</tr>
</thead>
<tbody>
<tr>
<td>AST-01</td>
<td>Kali Linux</td>
<td>192.168.56.10</td>
<td>Kali Linux</td>
<td>Up</td>
</tr>
<tr>
<td>AST-02</td>
<td>Windows Workstation</td>
<td>192.168.56.20</td>
<td>Windows 11</td>
<td>Up</td>
</tr>
</tbody>
</table>

<img class="alignnone size-full wp-image-386" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-1026081.png" alt="" width="647" height="295" />

## <h2>7. Network and Service Enumeration</h2> ##
Nmap was used to examine the services exposed by the Windows 11 workstation.

With the Windows Firewall temporarily disabled for assessment purposes, the following command was used:
<pre><code class="language-bash">sudo nmap -sV 192.168.56.20
</code></pre>
The scan identified:
<table>
<thead>
<tr>
<th>Port</th>
<th>Service</th>
</tr>
</thead>
<tbody>
<tr>
<td>135/tcp</td>
<td>Microsoft Windows RPC</td>
</tr>
<tr>
<td>139/tcp</td>
<td>NetBIOS Session Service</td>
</tr>
<tr>
<td>445/tcp</td>
<td>Microsoft-DS / SMB</td>
</tr>
</tbody>
</table>
These services represent part of the workstation's network attack surface.

<strong>Screenshot:</strong>

<img class="alignnone wp-image-380 size-full" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-104639.png" alt="" width="644" height="432" />

After the firewall was restored, a focused scan showed these ports as filtered, demonstrating that the Windows Firewall was restricting network access.
<pre><code class="language-bash">sudo nmap -sV -p 135,139,445 192.168.56.20
</code></pre>
<strong>Screenshot:</strong>

<img class="alignnone wp-image-381 size-full" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-154324.png" alt="" width="643" height="388" />

## <h2>8. Security Control Validation</h2> ##

### <h3>8.1 Windows Firewall</h3> ###
Windows Firewall was confirmed as enabled across the Domain, Private, and Public profiles.

The firewall was also observed filtering Windows RPC, NetBIOS, and SMB ports during normal operation.

This demonstrates that the firewall provides a network-level control that reduces exposure of these services.<code></code>

### <h3>8.2 SMB Configuration</h3> ###
The Windows SMB configuration was reviewed to determine which SMB protocols were enabled.

The result showed:
<ul>
 	<li>SMBv1: Disabled</li>
 	<li>SMBv2: Enabled</li>
</ul>
SMBv1 being disabled is a positive security control because the legacy protocol is not required for normal modern Windows operation.<code></code>

A focused Nmap SMB protocol check also showed TCP/445 as filtered while the firewall was enabled.

<strong>Screenshot:</strong>
<code><img class="alignnone size-full wp-image-383" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-105628.png" alt="" width="650" height="342" /></code>

## <h2>9. Vulnerability Assessment</h2> ##

### <h3>9.1 Unauthenticated Nessus Assessment</h3> ###
An initial unauthenticated Nessus scan was performed against:

<code>192.168.57.20</code>

The scan returned primarily informational and discovery results, including Ethernet card information, MAC address information, Nessus scan information, and traceroute information.

No severity-rated vulnerabilities were identified during this initial scan.

This demonstrated the difference between a network-based unauthenticated assessment and a credentialed Windows assessment.

<strong>Screenshot:</strong>

<img class="alignnone wp-image-374 size-large" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-152926-1024x394.png" alt="" width="640" height="246" />

### <h3>9.2 Authenticated Nessus Assessment</h3> ###
A separate authenticated Nessus assessment was configured for the Windows 11 workstation.

A dedicated local Windows assessment account was created with administrative privileges for the scan.

The Windows system was also configured to support the required credentialed assessment components, including Windows Management Instrumentation and Remote Registry access.

The authenticated scan provided substantially greater host visibility and identified multiple severity-rated vulnerabilities that were not visible during the initial unauthenticated assessment.

<strong>Screenshot:</strong>

<img class="alignnone wp-image-375 size-large" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-214009-1024x424.png" alt="" width="640" height="265" />

### <h3>9.3 Key Vulnerabilities Identified</h3> ###
The authenticated Nessus assessment identified vulnerabilities across the Windows operating system and installed applications.

#### <h4>Mozilla Firefox</h4> ####
The workstation contained <strong>Mozilla Firefox 2.0.0.11</strong>, an extremely outdated version.

Nessus identified multiple Firefox vulnerabilities and reported the grouped finding with a <strong>CVSS score of 10.0 and Critical severity</strong>.

This represents one of the most significant risks identified during the assessment.

<strong>Screenshot:</strong>

<img class="alignnone wp-image-376 size-large" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-220808-1024x454.png" alt="" width="640" height="284" />

#### <h4>VLC Media Player</h4> ####
The workstation also contained <strong>VLC Media Player 2.2.1</strong>.

Nessus identified multiple vulnerabilities associated with VLC versions below the secure version threshold identified by the plugin.

The finding included Critical, High, and Medium severity results.

#### <h4>Microsoft Windows Security Updates</h4> ####
Nessus identified missing Microsoft security updates affecting the Windows 11 workstation.

The Microsoft bulletin findings included Critical vulnerabilities with CVSS scores reported as high as <strong>9.8</strong>, along with additional High and Medium severity findings.

<strong>Screenshot:</strong>

<img class="alignnone wp-image-377 size-large" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-220828-1024x484.png" alt="" width="640" height="303" />

### <h3>Microsoft .NET Framework</h3> ###
Nessus identified missing security updates affecting Microsoft .NET Framework components installed on the workstation.

These findings included Critical and High severity results.

### <h3>Microsoft Teams</h3> ###
A High severity vulnerability affecting Microsoft Teams for Desktop was also identified.

### <h3>Windows Defender</h3> ###
Nessus identified issues relating to Windows Defender and antivirus signature definitions, including High and Low severity results.

### <h3>Lower-Severity Findings</h3> ###
Additional Medium and Low findings included Windows Package Manager, Windows Defender configuration, Windows speculative execution configuration, and ICMP timestamp information disclosure.

The ICMP timestamp finding was reported with a <strong>CVSS score of 2.1 and Low severity</strong>.

## <h2>Top Vulnerabilities Found</h2> ##
<img class="alignnone wp-image-378 size-large" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-230817-1024x601.png" alt="" width="640" height="376" />

### <h3>9.4 Vulnerability Severity Summary</h3> ###
The authenticated assessment produced the following severity distribution shown in Nessus:
<table>
<thead>
<tr>
<th>Severity</th>
<th align="right">Findings</th>
</tr>
</thead>
<tbody>
<tr>
<td>Critical</td>
<td align="right">133</td>
</tr>
<tr>
<td>High</td>
<td align="right">110</td>
</tr>
<tr>
<td>Medium</td>
<td align="right">77</td>
</tr>
<tr>
<td>Informational</td>
<td align="right">162</td>
</tr>
</tbody>
</table>
The findings were not treated as 482 individual vulnerabilities. Informational and discovery plugins were separated from severity-rated vulnerabilities, and grouped application findings such as the Firefox multiple-vulnerability finding were treated as individual assessment findings rather than as hundreds of separate risks.

<strong>Screenshot:</strong>

<img class="alignnone wp-image-379 size-large" src="https://www.topbusiness.com.ng/wp-content/uploads/2026/09/Screenshot-2026-09-10-214009-1-1024x424.png" alt="" width="640" height="265" />

## <h2>10. Risk Assessment</h2> ##
The identified findings were assessed using a qualitative 5 × 5 likelihood and impact model.

<strong>Risk Score = Likelihood × Impact</strong>
<table>
<thead>
<tr>
<th align="right">Score</th>
<th>Rating</th>
</tr>
</thead>
<tbody>
<tr>
<td align="right">20–25</td>
<td>Critical</td>
</tr>
<tr>
<td align="right">15–19</td>
<td>High</td>
</tr>
<tr>
<td align="right">8–14</td>
<td>Medium</td>
</tr>
<tr>
<td align="right">1–7</td>
<td>Low</td>
</tr>
</tbody>
</table>

### <h3>Risk Register</h3> ###
<table>
<thead>
<tr>
<th>ID</th>
<th>Risk</th>
<th style="text-align: center;" align="right">Likelihood</th>
<th style="text-align: center;" align="right">Impact</th>
<th style="text-align: center;" align="right">Score</th>
<th style="text-align: right;">Rating</th>
</tr>
</thead>
<tbody>
<tr>
<td>R-01</td>
<td>Extremely outdated Firefox exposes the workstation to multiple known vulnerabilities</td>
<td style="text-align: center;" align="right">5</td>
<td style="text-align: center;" align="right">5</td>
<td style="text-align: center;" align="right">25</td>
<td style="text-align: center;">Critical</td>
</tr>
<tr>
<td>R-02</td>
<td>Outdated VLC exposes the workstation to multiple known vulnerabilities</td>
<td style="text-align: center;" align="right">4</td>
<td style="text-align: center;" align="right">5</td>
<td style="text-align: center;" align="right">20</td>
<td style="text-align: center;">Critical</td>
</tr>
<tr>
<td>R-03</td>
<td>Missing Windows security updates expose the workstation to known OS vulnerabilities</td>
<td style="text-align: center;" align="right">4</td>
<td style="text-align: center;" align="right">5</td>
<td style="text-align: center;" align="right">20</td>
<td style="text-align: center;">Critical</td>
</tr>
<tr>
<td>R-04</td>
<td>Missing .NET Framework security updates may expose applications and the workstation to known vulnerabilities</td>
<td style="text-align: center;" align="right">4</td>
<td style="text-align: center;" align="right">4</td>
<td style="text-align: center;" align="right">16</td>
<td style="text-align: center;">High</td>
</tr>
<tr>
<td>R-05</td>
<td>Vulnerable Microsoft Teams installation introduces additional application attack surface</td>
<td style="text-align: center;" align="right">3</td>
<td style="text-align: center;" align="right">4</td>
<td style="text-align: center;" align="right">12</td>
<td style="text-align: center;">Medium</td>
</tr>
<tr>
<td>R-06</td>
<td>Outdated Windows Defender security definitions may reduce malware detection effectiveness</td>
<td style="text-align: center;" align="right">3</td>
<td style="text-align: center;" align="right">4</td>
<td style="text-align: center;" align="right">12</td>
<td style="text-align: center;">Medium</td>
</tr>
<tr>
<td>R-07</td>
<td>ICMP timestamp responses disclose host timing information</td>
<td style="text-align: center;" align="right">2</td>
<td style="text-align: center;" align="right">2</td>
<td style="text-align: center;" align="right">4</td>
<td style="text-align: center;">Low</td>
</tr>
</tbody>
</table>
The risk assessment prioritizes vulnerabilities that could have the greatest effect on the confidentiality, integrity, and availability of the workstation.

## <h2>11. Risk Treatment Plan</h2> ##
<table>
<thead>
<tr>
<th>Risk ID</th>
<th>Recommended Treatment</th>
<th>Priority</th>
</tr>
</thead>
<tbody>
<tr>
<td>R-01</td>
<td>Remove Firefox 2.0.0.11 and install a supported version of Firefox, or remove the application if it is no longer required.</td>
<td style="text-align: center;">Immediate</td>
</tr>
<tr>
<td>R-02</td>
<td>Upgrade VLC to a supported version or remove it if it is not required.</td>
<td style="text-align: center;">Immediate</td>
</tr>
<tr>
<td>R-03</td>
<td>Apply outstanding Microsoft Windows security updates and establish a regular patching cycle.</td>
<td style="text-align: center;">Immediate</td>
</tr>
<tr>
<td>R-04</td>
<td>Apply current Microsoft .NET Framework security updates.</td>
<td style="text-align: center;">High</td>
</tr>
<tr>
<td>R-05</td>
<td>Update Microsoft Teams to a supported version.</td>
<td style="text-align: center;">High</td>
</tr>
<tr>
<td>R-06</td>
<td>Update Windows Defender security intelligence and verify that automatic updates are functioning.</td>
<td style="text-align: center;">High</td>
</tr>
<tr>
<td>R-07</td>
<td>Disable unnecessary ICMP timestamp responses where operationally appropriate.</td>
<td style="text-align: center;">Low</td>
</tr>
</tbody>
</table>

### <h3>Existing Controls to Maintain</h3> ###
The following controls should remain enabled:
<ul>
 	<li>Windows Firewall across all active profiles.</li>
 	<li>SMBv1 disabled.</li>
 	<li>Network access restricted to required systems and services.</li>
</ul>

## <h2>12. Conclusion</h2> ##
The internal network security and risk assessment identified significant vulnerabilities on the Windows 11 workstation.

The authenticated Nessus assessment provided substantially greater visibility than the initial unauthenticated scan and I identified Critical, High, Medium, and Low severity findings.

The most significant issues were the presence of highly outdated third-party applications, including Mozilla Firefox 2.0.0.11 and VLC Media Player 2.2.1, together with missing Microsoft security updates.

Network enumeration also identified Windows RPC, NetBIOS, and SMB services. However, testing confirmed that the Windows Firewall was filtering these services during normal operation. SMBv1 was also confirmed to be disabled.

The assessment demonstrates the importance of authenticated vulnerability scanning, software and patch management, network security controls, and risk-based prioritisation when assessing an internal Windows environment.

## <h2>Key Skills Demonstrated</h2> ##
<ul>
 	<li>Vulnerability assessment</li>
 	<li>Authenticated vulnerability scanning,</li>
 	<li>Asset discovery,</li>
 	<li>Network enumeration,</li>
 	<li>Windows security assessment,</li>
 	<li>Vulnerability prioritisation,</li>
 	<li>Risk assessment,</li>
 	<li>Risk treatment planning,</li>
 	<li>Security control validation.</li>
</ul>
