
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
- <b>wget / yum</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>CheckMK web interface</b>
<br />
<h2>Project walk-through:</h2>

**Task:** Add the servers to CheckMK monitoring, following the CheckMK wiki instructions.

**Objective:** Learn to implement CheckMK for detailed system health monitoring and analysis.

**Requirements:**
- dev-app (dev-app-rexq)
- dev-performance (dev-performance-rxnq2)
- stage-web (stage-web-rexq3)

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Downloading the CheckMK agent package from the CheckMK server:</b>
<br/>
<img width="930" height="243" alt="image" src="https://github.com/user-attachments/assets/ba424174-464c-4de7-96c6-e6c2168b740e" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Installing the CheckMK agent:</b>
<br/>
<img width="681" height="731" alt="image" src="https://github.com/user-attachments/assets/32212c91-2dc0-4d8c-88f4-b7a1c72a2b39" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Opening port 6556/tcp in the firewall and confirming the agent socket is active:</b>
<br/>
 <img width="946" height="352" alt="image" src="https://github.com/user-attachments/assets/2a232d02-f9f9-4fd8-b3a4-c22c8360a22e" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-performance-rxnq2 — Downloading the CheckMK agent package:</b>
<br/>
<img width="947" height="273" alt="image" src="https://github.com/user-attachments/assets/556aacfb-2e1c-40ba-ba4e-d967ff4845c1" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-performance-rxnq2 — Installing the CheckMK agent:</b>
<br/>
<img width="941" height="253" alt="image" src="https://github.com/user-attachments/assets/352edd0d-6770-4e0d-bd71-09ec846572d2" />

</p>

<br />
<br />
<br />
<p align="center">
<b>dev-performance-rxnq2 — Opening port 6556/tcp in the firewall and confirming the agent socket is active:</b>
<br/>
<img width="942" height="339" alt="image" src="https://github.com/user-attachments/assets/9577fd99-8712-49f5-a40f-026004d1a505" />


</p>

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — Installing wget, downloading and installing the CheckMK agent:</b>
<br/>
<img width="946" height="591" alt="image" src="https://github.com/user-attachments/assets/e3da16b3-bc6b-40bb-87a4-d312826ff817" />

</p>

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — Opening port 6556/tcp in the firewall and confirming the agent socket is active:</b>
<br/>
<img width="951" height="442" alt="image" src="https://github.com/user-attachments/assets/0a0e358a-b369-4190-83c5-ec7dc8193e63" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Entering the host name and IPv4 address:</b>
<br/><img width="1242" height="560" alt="image" src="https://github.com/user-attachments/assets/ad09a76d-0a46-4a67-9937-ad276550e526" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — First save returned an "already exists" error:</b>
<br/>
<img width="1245" height="646" alt="image" src="https://github.com/user-attachments/assets/59c9df4c-4495-40af-9464-09c67554ac84" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Host properties show the IPv4 address was not saved:</b>
<br/>
<img width="1015" height="789" alt="image" src="https://github.com/user-attachments/assets/14bdbc0e-8189-4f31-9ac3-65604b54ee27" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Re-entering the IPv4 address in the host properties:</b>
<br/>
<img width="1024" height="776" alt="image" src="https://github.com/user-attachments/assets/016f6cf8-22f3-4e7d-a1fc-93975b51a7ef" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Running service discovery (201 services found):</b>
<br/><img width="651" height="588" alt="image" src="https://github.com/user-attachments/assets/71b72deb-c3e7-4ecb-a4d3-b391ad27bf35" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Accepting all discovered services:</b>
<br/>
<img width="1016" height="930" alt="image" src="https://github.com/user-attachments/assets/956fdf98-5512-464b-ad6a-fe9a5cbb899a" />


</p>

<br />
<br />
<br />
<p align="center">
<b>CheckMK — Activating the pending changes completed successfully:</b>
<br/>
<img width="1013" height="337" alt="image" src="https://github.com/user-attachments/assets/b5141787-febc-4af0-a5bc-dc1f7a3aaccc" />


</p>

<br />
<br />
<br />
<p align="center">
<b>CheckMK — Wiki step failed: the Monitor &gt; All hosts view returns no results:</b>
<br/>
<img width="1026" height="457" alt="image" src="https://github.com/user-attachments/assets/49d13c0a-aae4-4f89-921f-2892def53491" />


</p>

<br />
<br />
<br />
<p align="center">
<b>Updated CheckMK wiki instructions: disable the row limitation and search for the host in Setup (other user's host name redacted):</b>
<br/><img width="665" height="621" alt="image" src="https://github.com/user-attachments/assets/88521206-4e2a-42c6-8a01-4dad0ca1a217" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Verifying the host in Setup &gt; Hosts &gt; Dev:</b>
<br/>
<img width="864" height="461" alt="image" src="https://github.com/user-attachments/assets/91cf0686-3a73-4677-9178-a541c88980bf" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-performance-rxnq2 — Adding the host with its IPv4 address:</b>
<br/>
<img width="1051" height="588" alt="image" src="https://github.com/user-attachments/assets/35e8e6b4-d8ec-4f21-95ec-85c660e99bd8" />


</p>

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — Adding the host with its IPv4 address:</b>
<br/><img width="1035" height="572" alt="image" src="https://github.com/user-attachments/assets/18dbac8b-812b-48f5-9136-b19442fc9da9" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — CheckMK reports a critical NTP Time alert:</b>
<br/>
<img width="929" height="184" alt="image" src="https://github.com/user-attachments/assets/ba4409ae-32fa-44ac-8404-7f7466f1047d" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — Checking chronyd, confirming the time source, and forcing a clock step:</b>
<br/>
<img width="989" height="397" alt="image" src="https://github.com/user-attachments/assets/b9a3d711-ab8c-41fe-a411-781b12a44c53" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-app-rexq — NTP Time returns to OK after the rescan:</b>
<br/>
<img width="986" height="311" alt="image" src="https://github.com/user-attachments/assets/be301247-830d-4aa1-9f9d-7ae1bfd48a81" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-performance-rxnq2 — CheckMK reports a critical NTP Time alert:</b>
<br/>
<img width="1105" height="278" alt="image" src="https://github.com/user-attachments/assets/8cd78421-608a-4992-8707-f7a58f825191" />


</p>

<br />
<br />
<br />
<p align="center">
<b>dev-performance-rxnq2 — NTP Time returns to OK after the rescan:</b>
<br/><img width="1070" height="252" alt="image" src="https://github.com/user-attachments/assets/295221d9-482f-4564-a772-b66b7eaa37bd" />


</p>

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — CheckMK reports a critical NTP Time alert:</b>
<br/>
<img width="1138" height="258" alt="image" src="https://github.com/user-attachments/assets/a2d6b80a-4486-4631-8fd1-055d03e86123" />


</p>

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — NTP Time returns to OK after the rescan:</b>
<br/>
<img width="1122" height="276" alt="image" src="https://github.com/user-attachments/assets/e7d20805-ac4d-4d15-aadd-13fb4f601c9c" />


</p>

<br />
<br />
<br />

<b><u>Solution:</u></b> I installed the CheckMK agent on all three servers (dev-app-rexq, dev-performance-rxnq2 and stage-web-rexq3) by downloading the agent package from the CheckMK server with wget and installing it with yum, then opened port 6556/tcp in firewalld and confirmed the agent socket was active and listening. In the CheckMK web interface I added each server to the Dev folder with its IPv4 address, ran service discovery, accepted the discovered services (201 for dev-app-rexq) and activated the pending changes. Two obstacles came up. The first save of dev-app-rexq returned an "already exists" error and the host was left without its IP address, so I reopened the host properties, re-entered the address and saved again. The wiki's verification step (Monitor &gt; All hosts) did not match this version of CheckMK and returned no results, so I escalated it to the administrator, who updated the wiki to verify through Setup &gt; Hosts &gt; Dev instead (disable the row limitation, search for the host name). CheckMK then raised a critical NTP Time alert on each server. chronyd was running and synced to the IPA time source, but CheckMK reported a stale last-sync time, so I forced a clock step with <code>chronyc makestep</code> on each server. After a rescan, NTP Time returned to OK on all three.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---

<br />
<br />


<a name="ticket-61"></a>
<h1>Ticket #61 — Fix the Issue on Your Web Server</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>ssh</b>
- <b>systemctl</b>
- <b>df</b>
- <b>tail</b>
- <b>curl</b>
- <b>CheckMK</b>
- <b>chrony (chronyc)</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>Apache HTTP Server (httpd)</b>
- <b>CheckMK web interface</b>
<br />
<h2>Project walk-through:</h2>

**Task:** The team had been receiving a lot of emails about issues on stage-web.

**Objective:** Learn to implement CheckMK for detailed system health monitoring and analysis.

**Requirements:**
- Use CheckMK to verify what is wrong and fix the issue
- stage-web (stage-web-rexq3)

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — Connecting from the bastion host and checking the Apache (httpd) service:</b>
<br/>
<img width="951" height="503" alt="image" src="https://github.com/user-attachments/assets/e2a6131f-fbea-4531-b10d-bba04c2b6323" />


</p>

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — Checking disk space across all mounted filesystems:</b>
<br/>
<img width="952" height="286" alt="image" src="https://github.com/user-attachments/assets/837929d0-0e8a-4b52-af15-5895253c99f7" />


</p>

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — Reviewing the most recent httpd error log entries:</b>
<br/><img width="947" height="311" alt="image" src="https://github.com/user-attachments/assets/79e0e33c-3176-4032-a175-c0cdc24f89e9" />


</p>

<br />
<br />
<br />
<p align="center">
<b>stage-web-rexq3 — Testing the web server locally:</b>
<br/>
<img width="963" height="266" alt="image" src="https://github.com/user-attachments/assets/2f7ad187-6433-49fb-aed9-162f97761a48" />


</p>

<br />
<br />
<br />
<p align="center">
<b>CheckMK — Host &amp; service problems view returns no entries:</b>
<br/>
<img width="1147" height="412" alt="image" src="https://github.com/user-attachments/assets/4252542f-9701-465e-8761-e91efc1ee9e8" />


</p>

<br />
<br />
<br />
<p align="center">
<b>CheckMK — Confirming there are no pending changes to activate:</b>
<br/>
<img width="1143" height="285" alt="image" src="https://github.com/user-attachments/assets/263e833f-a3e8-4cd6-9efa-80145df60f10" />


</p>

<br />
<br />
<br />
<p align="center">
<b>CheckMK — Host &amp; service history filtered to stage-web-rexq3 also returns nothing:</b>
<br/>
<img width="1143" height="749" alt="image" src="https://github.com/user-attachments/assets/0770be61-53c4-47ca-b234-c8eff7bfa0e6" />

</p>

<br />
<br />
<br />

<b><u>Solution:</u></b> I started with operating-system checks on stage-web-rexq3 and found nothing wrong: httpd was active and running, no filesystem was above 42% used, the httpd error log held only routine startup notices, and a local request returned HTTP 200 OK. Because the ticket required using CheckMK to find the problem, I then turned to its Monitor views. The Host &amp; service problems view and the Host &amp; service history view filtered to stage-web-rexq3 both came back empty, and there were no pending changes waiting to be activated. This was the same display issue I ran into in <a href="#ticket-60">Ticket #60</a>, where CheckMK's Monitor views returned no results, so I paused the ticket until the corrected verification method from that ticket was available (Setup &gt; Hosts &gt; Dev, search for the host). That let me read the service list for stage-web-rexq3 directly, where CheckMK showed a critical NTP Time alert. I resolved it by forcing a clock step with <code>chronyc makestep</code>, and NTP Time returned to OK (see Ticket #60).

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---

<br />
<br />
