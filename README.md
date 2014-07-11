<h1>Welcome-to-P800X</h1>

To work on any P800X project, please follow the instructions below on setting up your own, local, P800X development server!

<hr />

<h2>Installing Local P800X Development Server</h2>

<ol>
	<li>Download and Install Oracle VM VirtualBox

		<p>https://www.virtualbox.org/wiki/Downloads</p>
	</li>

	<li>Download Fedora 17 64-Bit

		<p>http://archive.fedoraproject.org/pub/archive/fedora/linux/releases/17/Fedora/x86_64/iso/</p>

		<p><em>(These instructions are based off of the net install .iso)</em></p>

		<p>For a fast install, download the net install .iso</p>

		<p>If you are planning an offline installation, download the full DVD .iso</p>
	</li>

	<li>While Fedora is downloading, open Oracle VM VirtualBox
		<ol>
			<li>Click on "New"
			<pre>Name: P800X-DevBox
Type: Linux
Version: Fedora (64-bit)
</pre></li>

			<li>Click "Next"

				<p>Adjust the amount of RAM you want to give your VM</p>
				<pre>Recommended: >1024MB</pre>
			</li>

			<li>Click "Next"

				<p>Select "Create a virtual hard drive now"</p>
			</li>

			<li>Click "Create"

				<p>Select "VDI (VirtualBox Disk Image)"</p>
			</li>

			<li>Click "Next"

				<p>Select "Dynamically allocated"</p>
			</li>

			<li>Click "Next"

				<p>Select file location and size</p>

				<pre>Recommended Size: >8GB</pre>
			</li>

			<li>Click "Create"</li>
			
			<li>Select your new VM</li>
			
			<li>Click "Settings"</li>
			
			<li>Navigate to "Network"</li>

			<li>For the "Attached to:" field, select "Bridged Adapter"</li>

			<p>Your VM is now setup with its' most basic settings.</p>
		</ol>
	</li>

	<li>When Fedora is fully downloaded, select your new VM and click "Start"
		<ol>
			<li>Click on the folder icon and browse to and select your Fedora iso; then click "Open"</li>

			<li>Click "Start"</li>

			<p>Your VM will now boot</p>
		</ol>
	</li>

	<li>Select "Install or upgrade Fedora"

		<p><em>Highlighted option will appear as white text<br />
		To select an option, hit "Enter"</em></p>

		<p>Follow the installation instructions</p>

		<p>Recommended Settings:</p>

		<pre>Select "Basic Storage Devices"
Select "Yes, discard any data"
Hostname: "P800X-DevBox"

Write down your root password!

Select "Use All Space"
Click "Write Changes to Disk"</pre>

		<p>When you are presented with a list of Software Packages</p>

		<ol>
			<li>Select "Web Server"</li>
			<li>Select "Customize Now"</li>

			<li>Click "Next"</li>
			
			<li>Under "Desktop Environments"
				<ul>
					<li>Deselect "GNOME Desktop Environment"</li>
				</ul>
			</li>

			<li>Under "Applications"
				<ul>
					<li>Deselect "Graphical Internet"</li>
					<li>Deselect "Text-based Internet"</li>
				</ul>
			</li>

			<li>Under "Servers"
				<ul>
					<li>Select "FTP Server"</li>
					<li>Select "MySQL Database"</li>
					<li>Select "Web Server"
						<Ul>
							<li>Click "Optional Packages"</li>
							<li>Select "phpMyAdmin"</li>
						</Ul>
					</li>
				</ul>
			</li>

			<li>Under "Base System"
				<ul>
					<li>Deselect "Administration Tools"</li>
					<li>Deselect "X Window System"</li>
				</ul>
			</li>

			<li>Click "Next"</li>
		</ol>
	</li>

	<li>When Fedora is done installing, click "Reboot", but quickly power the VM off (before it boots again).</li>

	<li>Open Oracle VM VirtualBox
		<ol>
			<li>Select your new Fedora VM</li>

			<li>Click on "Settings"</li>

			<li>Navigate to "Storage"</li>

			<li>Select your install iso under the IDE controller</li>

			<li>Click the "Remove Attachment" icon</li>

			<li>Click "Remove"</li>

			<li>Click "Ok"</li>

			<li>Select your new Fedora VM</li>

			<li>Click "Start"

			<p>Your VM will now boot with Fedora</p></li>
		</ol>
	</li>

	<li>When prompted with a login screen

		<p>Enter "root" and enter your root password</p>

		<p>You are now logged in.</p></li>

	<li>Create 811trac user
		<pre>$ adduser 811trac
$ passwd 811trac

Provide the password for your 811trac user, write it down!</pre></li>

    <li>Load Apache on startup:
        <pre>$ systemctl enable httpd.service</pre>
    </li>
    
    <li>Start Apache:
        <pre>$ /sbin/service httpd start</pre>
    </li>

	<li>Load MySQL on startup:
		<pre>$ systemctl enable mysqld.service</pre></li>

	<li>Start MySQL:
		<pre>$ /sbin/service mysqld start</pre></li>

	<li>Change MySQL Password:
		<pre>$ mysqladmin -uroot password '&lt;password_here&gt;'

Write it down!</pre></li>

	<li>Install Git:
		<pre>$ yum install -y git-core</pre>
	</li>
	
	<p>Fedora has some strict firewall rules, so let's fix that.</p>
	
	<li>Create the iptables config file
	    <pre>$ cd ~
$ nano iptables.conf</pre>

        <p>Copy this config file into the new iptables.conf file</p>
        
        <pre>*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [6227901:946342305]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 443 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT</pre>
	</li>
	
	<li>Load the iptables
	    <pre>$ iptables-restore &lt; ~/iptables.conf</pre>
	</li>
	
	<li>Save the iptables
	    <pre>$ /usr/libexec/iptables.init save</pre>
	</li>

	<li>For the new iptables config to take effect, you must reboot the VM.</li>

	<li>Login as 811trac

		<p>Enter "811trac" as the username</p>

		<p>Provide your password for 811trac</p>

		<pre>$ cd /var/www/html/</pre>

		<p>Clone your desired repo here!</p>

		<pre>$ git clone https://github.com/&lt;username&gt;/&lt;repo_name&gt;.git ./path/to/clone/to</pre>
	</li>
</ol>

<hr />

<dl>
	<dt>To access your VM via HTTP:</dt>
	<dd><pre>$ ifconfig</pre>

	Look for the "inet" ipaddress, not 127.0.0.1, and enter that in your browser</dd>

	<br />

	<dt>To access your VM via SSH:</dt>
	<dd><pre>$ ifconfig</pre>

	Look for the "inet" ipaddress, not 127.0.0.1, and enter that in your browser

	<pre>$ ssh &lt;username&gt;@&lt;ipaddress&gt;</dd>

	<br />

	<dt>To run VM in a headless-state:</dt>
	<dd>Power down your VM.

	Open Oracle VM VirtualBox.

	While holding shift, double click on your VM</dd>
	
	<br />
	
	<dt>To install node:</dt>
	<dd><pre>$ yum install -y gcc-c++ make openssl-devel
$ cd ~
$ wget http://nodejs.org/dist/node-latest.tar.gz
$ tar zxvf node-latest.tar.gz
$ cd node-latest
$ ./configure
$ make
$ make install
</pre></dd>
</dl>
