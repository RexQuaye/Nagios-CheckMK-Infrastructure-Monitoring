
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
<img width="944" height="230" alt="image" src="https://github.com/user-attachments/assets/fcdbc7d9-c189-4e6d-9b4c-31b220ba81db" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Installing NRPE on dev-app:</b>
<br/><img width="972" height="293" alt="image" src="https://github.com/user-attachments/assets/312c2ffa-8925-4061-9fca-cdf5b8d7864e" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Installing the Nagios plugins on dev-app:</b>
<br/>
<img width="954" height="430" alt="image" src="https://github.com/user-attachments/assets/f5cfb842-d65c-48a3-8a9c-ea4ff8acaed1" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Confirming the NRPE allowed_hosts setting on dev-app:</b>
<br/>
<img width="964" height="223" alt="image" src="https://github.com/user-attachments/assets/874ae69d-75b3-48c3-8afb-17b2da5bdfad" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Verifying the NRPE service is running on dev-app:</b>
<br/>
<img width="950" height="487" alt="image" src="https://github.com/user-attachments/assets/53f37ea1-212b-45b3-bb26-32e2ed875e0c" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Installing the EPEL repository on stage-web:</b>
<br/>
<img width="898" height="394" alt="image" src="https://github.com/user-attachments/assets/ff27fd0f-2c1e-43f9-a785-9670b49bb5f1" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Installing NRPE on stage-web:</b>
<br/>
<img width="960" height="384" alt="image" src="https://github.com/user-attachments/assets/7fa9d276-7e90-41ca-91dc-cf2cf5c5fbdb" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Installing the Nagios plugins on stage-web:</b>
<br/><img width="955" height="437" alt="image" src="https://github.com/user-attachments/assets/4b62dd70-718d-4f53-9c0e-ecdc7006a611" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Editing nrpe.cfg to allow the Nagios server on stage-web:</b>
<br/>
<img width="1568" height="455" alt="image" src="https://github.com/user-attachments/assets/8ed0789a-ac0f-4bbc-abb5-0db8694b941c" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Confirming the NRPE allowed_hosts setting on stage-web:</b>
<br/><img width="809" height="168" alt="image" src="https://github.com/user-attachments/assets/98e72f24-f05e-40e6-afbd-ddeb3a7216fb" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Verifying the NRPE service is running on stage-web:</b>
<br/>
<img width="959" height="458" alt="image" src="https://github.com/user-attachments/assets/412f790a-4c2a-4974-bb03-725f2cf22f22" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Defining both hosts and their PING and SSH checks on the Nagios server:</b>
<br/>
<img width="822" height="783" alt="image" src="https://github.com/user-attachments/assets/c4dfd3b9-ba0b-48cf-9a7b-3083ee18b3b3" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Validating the Nagios configuration with a pre-flight check:</b>
<br/><img width="847" height="751" alt="image" src="https://github.com/user-attachments/assets/a2731ff5-5fbf-4100-9323-f9608b694865" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Restarting Nagios:</b>
<br/><img width="939" height="534" alt="image" src="https://github.com/user-attachments/assets/9c1b2930-5c98-4a12-a233-a2cb7878e797" />



</p>

<br />
<br />
<br />

<p align="center">
<b>dev-app appears in the Nagios host list as UP:</b>
<br/><img width="1562" height="120" alt="image" src="https://github.com/user-attachments/assets/04fe1be5-6fc4-4763-9d72-1cc8fab0c7e5" />



</p>

<br />
<br />
<br />

<p align="center">
<b>stage-web appears in the Nagios host list awaiting its first check:</b>
<br/><img width="1568" height="241" alt="image" src="https://github.com/user-attachments/assets/23a92ace-5c4e-48e0-86c2-4b92e34d5a5e" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Nagios host status page:</b>
<br/><img width="932" height="510" alt="image" src="https://github.com/user-attachments/assets/d64833b1-cea1-4a41-bb5f-10eef50397b2" />



</p>
<br />

<b><u>Solution:</u></b>  I installed the EPEL repository, NRPE and the required Nagios plugins on both client servers, dev-app and stage-web, and edited each server's nrpe.cfg so the allowed_hosts setting permits connections from the Nagios server. I then confirmed that the NRPE service was enabled and running on both. On the Nagios server, I built a host configuration file from the provided yt-templates.cfg.bak template, defining both hosts with PING and SSH service checks. The template contained host blocks from another user, which caused a duplicate-definition error during validation, so I rebuilt the file with only the blocks needed for this ticket. I added the file's path to nagios.cfg, validated the configuration with the Nagios pre-flight check (0 warnings, 0 errors), and restarted the service. Both hosts then appeared in the Nagios web interface, with dev-app reporting UP.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---



<br />
<br />

<a name="ticket-60"></a>
<h1>Ticket #60 — Add Both of Your Servers to CheckMK</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>CheckMK</b>
- <b>CheckMK agent</b>
- <b>chrony (chronyc)</b>
- <b>systemctl</b>
- <b>firewall-cmd</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>CheckMK web interface</b>
<br />
<h2>Project walk-through:</h2>

**Task:** Add the servers to CheckMK monitoring, following the CheckMK wiki instructions.

**Objective:** Learn to implement CheckMK for detailed system health monitoring and analysis.

**Requirements:**
- dev-app
- dev-performance
- stage-web

<br />
<br />
<br />
<p align="center">
<b>Confirming the CheckMK agent socket is active on dev-app:</b>
<br/>
<img width="961" height="287" alt="image" src="https://github.com/user-attachments/assets/473bc34f-d3a8-448a-a4a5-2e8402abda10" />



</p>

<br />
<br />
<br />

<p align="center">
<b>dev-app: CheckMK reports NTP Time as critical:</b>
<br/>
<img width="1006" height="201" alt="image" src="https://github.com/user-attachments/assets/a2566079-3309-4ebf-bcf5-7d148e4cca51" />



</p>

<br />
<br />
<br />

<p align="center">
<b>dev-app: checking chrony and forcing a time sync:</b>
<br/>
<img width="1008" height="413" alt="image" src="https://github.com/user-attachments/assets/aa720194-13e8-4b6f-9d8c-f276f2753ae0" />



</p>

<br />
<br />
<br />

<p align="center">
<b>dev-app: NTP Time back to OK in CheckMK:</b>
<br/>
<img width="1011" height="330" alt="image" src="https://github.com/user-attachments/assets/a1e3493e-f8fa-48f2-bfe6-56fd17609435" />



</p>

<br />
<br />
<br />

<p align="center">
<b>dev-performance: CheckMK reports NTP Time as critical:</b>
<br/>
<img width="892" height="285" alt="image" src="https://github.com/user-attachments/assets/1e6cc820-ab02-441d-b8a9-568ddf34d769" />



</p>

<br />
<br />
<br />

<p align="center">
<b>dev-performance: checking chrony and forcing a time sync:</b>
<br/>
<img width="896" height="471" alt="image" src="https://github.com/user-attachments/assets/07cfb7c9-742f-4bde-ad7d-1ff0aad06731" />



</p>

<br />
<br />
<br />

<p align="center">
<b>dev-performance: NTP Time back to OK in CheckMK:</b>
<br/>
<img width="1070" height="252" alt="image" src="https://github.com/user-attachments/assets/fb4431ee-880d-4e50-ab86-9632f291dbde" />


</p>

<br />
<br />
<br />

<p align="center">
<b>stage-web: CheckMK reports NTP Time as critical:</b>
<br/>
<img width="1138" height="258" alt="image" src="https://github.com/user-attachments/assets/f5fd11fd-c0b7-4039-aec7-41d8c15ce99a" />



</p>

<br />
<br />
<br />

<p align="center">
<b>stage-web: checking chrony and forcing a time sync:</b>
<br/>
<img width="1083" height="634" alt="image" src="https://github.com/user-attachments/assets/fd569b0f-2881-4179-8250-118158bf026e" />



</p>

<br />
<br />
<br />

<p align="center">
<b>stage-web: NTP Time back to OK in CheckMK:</b>
<br/>
<img width="1137" height="287" alt="image" src="https://github.com/user-attachments/assets/4de8de17-60e2-46c7-819a-42ca5e77265f" />



</p>

<br />

<b><u>Solution:</u></b>  Following the CheckMK wiki, I installed the CheckMK agent on each of my three servers (dev-app, dev-performance and stage-web), opened port 6556 in each firewall, and confirmed the agent socket was active and listening. In the CheckMK web interface I added each host to the Dev folder, ran service discovery, accepted the discovered services and activated the pending changes. Two obstacles came up. CheckMK sometimes reported that a host already existed even though it had saved without its IP address, so I reopened the host, re-entered the IP and saved again. The wiki's verification step also did not match the current CheckMK interface, and the Monitor views showed no results for my hosts. I escalated this to the administrator, who confirmed the issue and updated the wiki with a working method, which I used to verify each host. That check showed a critical NTP Time alert on each server. chrony was running and synced to the IPA time source, but CheckMK was reporting a stale last-sync time, so I forced a sync with chronyc makestep. After a rescan, NTP Time returned to OK on all three servers.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---

<br />
<br />
