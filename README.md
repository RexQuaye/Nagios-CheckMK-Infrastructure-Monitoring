
<a name="top"></a>
<h1>Nagios and CheckMK Infrastructure Monitoring</h1>

### 📋 Tickets Covered in This Repository

- 👤 [**Ticket #47** — Create Nagios User and Configure Access](#ticket-47)
- 🖥️ [**Ticket #48** — Add the Servers to Nagios](#ticket-48)
- 📊 [**Ticket #60** — Add Both of Your Servers to CheckMK](#ticket-60)
- 🛠️ [**Ticket #61** — Fix the Issue on Your Web Server](#ticket-61)
- 📡 [**Ticket #69** — CheckMK Monitoring and Host Reachability](#ticket-69)

---

<br />
<br />

<a name="ticket-47"></a>
<h1>Ticket #47 — Create Nagios User and Configure Access</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>Nagios Core</b>
- <b>htpasswd (Apache utilities)</b>
- <b>systemctl</b>
- <b>vi</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>Nagios Core 4.4</b>
<br />
<h2>Project walk-through:</h2>

**Task:** To use the Nagios service, create an HTTP user on the server, following the instructions on the wiki page.

**Objective:** Strengthen monitoring expertise by setting up health checks in Nagios.

**Requirements:**
- VM: stage.nagios
- User added and authorized to use Nagios
- Using your IPA username, create a Nagios user with access to the console

<br />
<br />
<br />
<p align="center">
<b>Creating the Nagios web user with htpasswd:</b>
<br/>
<img width="878" height="738" alt="image" src="https://github.com/user-attachments/assets/fe4acde7-71d7-4c17-9af3-462959512345" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Authorizing the user in cgi.cfg:</b>
<br/>
<img width="1001" height="659" alt="image" src="https://github.com/user-attachments/assets/ea50f293-b102-4b90-a305-7baede69c67e" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Restarting Nagios and checking its status:</b>
<br/>
<img width="951" height="560" alt="image" src="https://github.com/user-attachments/assets/8afb0a38-832d-4666-949f-00b2c0c0b869" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Confirming the Nagios web interface is running:</b>
<br/>
<img width="1085" height="853" alt="image" src="https://github.com/user-attachments/assets/3b3a6ed6-08a7-40bf-9fd7-94f493d8dae0" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Removing the accidental system commands access and restarting:</b>
<br/>
<img width="837" height="821" alt="image" src="https://github.com/user-attachments/assets/7974198a-6a52-48d7-830c-c00bb95359ce" />



</p>

<br />

<b><u>Solution:</u></b>  I connected to the Nagios server with my own IPA username (the wiki's example username was a placeholder, not a real account) and created a Nagios web user with htpasswd, then confirmed the entry in the htpasswd.users file. To authorize the user, I edited cgi.cfg and added my account to the view-only lists: system information, configuration information, all services and all hosts. I restarted Nagios and confirmed the service was running and that the web interface loaded. While editing several directives at once, I had also added my account to authorized_for_system_commands, which would grant restart and shutdown authority and goes beyond the least-privilege access the wiki recommends. I noticed it, removed my account from that list, verified the change with grep, and restarted Nagios again to apply the corrected configuration.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---

<br />
<br />

---

<br />
<br />

<a name="ticket-48"></a>
<h1>Ticket #48 — Add the Servers to Nagios</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>Nagios Core</b>
- <b>NRPE (Nagios Remote Plugin Executor)</b>
- <b>Nagios plugins</b>
- <b>yum</b>
- <b>systemctl</b>
- <b>vi</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>Nagios Core 4.4</b>
<br />
<h2>Project walk-through:</h2>

**Task:** Add your virtual machines to the Nagios monitoring system.

**Objective:** Strengthen monitoring expertise by setting up health checks in Nagios.

**Requirements:**
- VM server: stage-nagios
- VM clients: dev-app-[initials].procore.prod1 and stage-web-[initials].procore.prod1

**Information:**
- Use yt-templates.cfg.bak to create your host file configuration under /usr/local/nagios/etc/objects
- Add the path to your host configuration under /usr/local/nagios/etc/nagios.cfg

<br />
<br />
<br />
<p align="center">
<b>Installing the EPEL repository on dev-app:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="sudo yum install epel-release -y on dev-app reports the package is already installed" src="PASTE_URL_1" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Installing NRPE on dev-app:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="sudo yum install nrpe -y on dev-app reports the package is already installed" src="PASTE_URL_2" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Installing the Nagios plugins on dev-app:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="sudo yum install of the nagios-plugins load, http, users, procs, disk, swap, nrpe and uptime packages on dev-app, all already installed" src="PASTE_URL_3" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Confirming the NRPE allowed_hosts setting on dev-app:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="grep of allowed_hosts in /etc/nagios/nrpe.cfg on dev-app showing the Nagios server address is allowed" src="PASTE_URL_4" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Verifying the NRPE service is running on dev-app:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="systemctl status nrpe on dev-app showing the service enabled and active, listening on port 5666 and allowing connections from the Nagios server" src="PASTE_URL_5" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Installing the EPEL repository on stage-web:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="sudo yum install epel-release -y on stage-web reports the package is already installed" src="PASTE_URL_6" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Installing NRPE on stage-web:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="sudo yum install nrpe -y on stage-web reports the package is already installed" src="PASTE_URL_7" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Installing the Nagios plugins on stage-web:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="sudo yum install of the nagios-plugins load, http, users, procs, disk, swap, nrpe and uptime packages on stage-web, all already installed" src="PASTE_URL_8" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Editing nrpe.cfg to allow the Nagios server on stage-web:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="Annotated: sudo vi /etc/nagios/nrpe.cfg on stage-web with the allowed_hosts line including the Nagios server address" src="PASTE_URL_9" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Confirming the NRPE allowed_hosts setting on stage-web:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="grep of allowed_hosts in /etc/nagios/nrpe.cfg on stage-web showing the Nagios server address is allowed" src="PASTE_URL_10" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Verifying the NRPE service is running on stage-web:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="systemctl status nrpe on stage-web showing the service enabled and active, listening on port 5666 and allowing connections from the Nagios server" src="PASTE_URL_11" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Defining both hosts and their PING and SSH checks on the Nagios server:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="rquaye-hosts.cfg on stage-nagios with host definitions for dev-app and stage-web and PING and SSH service definitions for both" src="PASTE_URL_12" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Validating the Nagios configuration with a pre-flight check:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="nagios -v on nagios.cfg reports 48 hosts and 85 services checked with 0 warnings and 0 errors" src="PASTE_URL_13" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Restarting Nagios:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="sudo systemctl restart nagios followed by systemctl status nagios showing the service active and running; other users' host names redacted" src="PASTE_URL_14" />


</p>

<br />
<br />
<br />

<p align="center">
<b>dev-app appears in the Nagios host list as UP:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="Nagios web interface host list showing dev-app as UP with a PING OK status; other users' hosts redacted" src="PASTE_URL_15" />


</p>

<br />
<br />
<br />

<p align="center">
<b>stage-web appears in the Nagios host list awaiting its first check:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="Nagios web interface host list showing stage-web as PENDING with its first host check scheduled; other users' hosts redacted" src="PASTE_URL_16" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Nagios host status page:</b>
<br/>
<img width="PASTE_WIDTH" height="PASTE_HEIGHT" alt="Nagios Host Status Details page logged in as rquaye, with the dev-app host listed as UP; other users' hosts redacted" src="PASTE_URL_17" />


</p>

<br />

<b><u>Solution:</u></b>  I installed the EPEL repository, NRPE and the required Nagios plugins on both client servers, dev-app and stage-web, and edited each server's nrpe.cfg so the allowed_hosts setting permits connections from the Nagios server. I then confirmed that the NRPE service was enabled and running on both. On the Nagios server, I built a host configuration file from the provided yt-templates.cfg.bak template, defining both hosts with PING and SSH service checks. The template contained host blocks from another user, which caused a duplicate-definition error during validation, so I rebuilt the file with only the blocks needed for this ticket. I added the file's path to nagios.cfg, validated the configuration with the Nagios pre-flight check (0 warnings, 0 errors), and restarted the service. Both hosts then appeared in the Nagios web interface, with dev-app reporting UP.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---

<br />
<br />
