- > `The IPv6Spot project is an open-source, free, and non-commercial project. The goal of this system is to advance the development of IPv6.`
- **Project start date: 06/2021**
- **project publication date on GitHub: 24/09/2024**
- **important note: The IPv6 addresses used in the source code are for the example only, you must replace them with addresses compatible with the IPv6 standards.**
- **I hope everyone benefits from this open-source project for the development of IPv6.**

		abdulkader.alrezej@outlook.com

Research Paper: IPv6Spot - An Innovative IPv6 Captive Portal Solution with Integrated DNS and Proxy Services

Abstract

This paper introduces IPv6Spot, a pioneering captive portal system designed exclusively for IPv6 networks. Developed by Abdulkader Alrezej, IPv6Spot addresses the critical need for effective network access control in IPv6 environments. The system offers comprehensive features such as user authentication, dynamic IP and MAC address management, bandwidth control, usage monitoring, integrated DNS redirection, and custom proxy servers. The inclusion of custom DNS and proxy servers enhances the captive portal functionality by intercepting network requests and guiding unauthenticated users to the login page. This paper provides an in-depth examination of the system's architecture, implementation details, and the innovative approaches employed to manage IPv6 network access effectively.

Introduction

The global transition to IPv6 is necessitated by the exhaustion of IPv4 addresses and the growing number of internet-connected devices. Despite this shift, many network management tools and captive portal solutions remain focused on IPv4, leaving a gap in support for IPv6 networks. IPv6Spot fills this gap by providing a fully functional captive portal tailored for IPv6 environments. The system not only controls network access but also integrates custom DNS and proxy servers to enhance user experience and enforce network policies effectively.

Project Overview

IPv6Spot is a Python-based application utilizing the Flask web framework, various Linux networking utilities, a custom DNS server, and proxy servers. Key features of IPv6Spot include:

1. IPv6 Captive Portal with DNS and Proxy Redirection: Redirects unauthenticated users to a login page by intercepting DNS queries and HTTP requests, controlling network access.


2. Secure User Authentication: Implements robust authentication mechanisms with hashed passwords and secure session management.


3. Dynamic IP and MAC Address Management: Automatically detects and manages user devices based on their IPv6 addresses and MAC addresses.


4. Bandwidth Control and Traffic Shaping: Utilizes Linux traffic control (tc) to allocate and limit bandwidth per user.


5. Usage Monitoring and Enforcement: Tracks data usage and session duration, enforcing limits based on predefined user policies.


6. Custom DNS and Proxy Server Integration: Employs DNS and proxy servers to manage network requests, enhancing captive portal functionality.



System Architecture

The architecture of IPv6Spot is modular, comprising several interconnected components that work together to manage network access and enforce policies.

Web Interface

Flask Framework: Manages HTTP requests, user sessions, and renders templates for the web interface.

Templates and Static Files: Provide the user interface elements such as login pages and dashboards.


Networking Layer

IP and MAC Address Management: Utilizes system commands and the ipaddress module to manage IPv6 addresses and associate them with user sessions.

Traffic Control: Implements bandwidth limitations and traffic shaping using the tc command.

Firewall Rules: Uses ip6tables and nftables to enforce network access policies.


Database Layer

SQLite Database: Stores user credentials, session data, IP addresses, and usage statistics.

Schema Management: Ensures the database schema supports all required fields for new features.


Custom DNS Server

DNS Interception: A DNS server built with the dnslib library intercepts DNS queries from clients.

Domain Redirection: Redirects specific DNS queries to the captive portal or blocks them based on the client's authentication status.

IPv4-to-IPv6 Translation: Converts IPv4 addresses to IPv6 format for domains that do not have IPv6 records.


Custom Proxy Servers

HTTP Proxy Server: Intercepts HTTP requests from clients and redirects unauthenticated users to the captive portal.

Network Detection Responses: Provides appropriate responses to network connectivity checks performed by operating systems.


Background Services

Periodic IP Checks: Monitors user activity and enforces policies through background threads.

Usage Data Collection: Continuously updates usage statistics.


Implementation Details

The implementation of IPv6Spot involves integrating Python scripts with system-level networking commands, a custom DNS server, and custom proxy servers.

User Authentication

Credential Security: Uses werkzeug.security for password hashing and verification.

Session Handling: Manages user sessions securely using Flask's session management and secret keys.


IP and MAC Address Management

MAC Address Retrieval: Executes ip -6 nei to map IPv6 addresses to MAC addresses.

Dynamic IP Handling: Adds or removes IPv6 addresses in nftables based on user authentication status.


Traffic Control

Bandwidth Allocation: Creates tc classes and filters to enforce per-user bandwidth limits.

Dynamic Adjustment: Updates tc rules in response to changes in user sessions.


Firewall and Access Control

Firewall Configuration: Manages ip6tables rules to control traffic based on user authentication.

Nftables Integration: Uses nftables for advanced packet filtering and accounting.


Custom DNS Server

The custom DNS server is a critical component that enhances the captive portal's functionality.

DNS Server Overview

Library Used: Built using the dnslib Python library, which allows the creation of custom DNS servers and resolvers.

Server Initialization: The DNS server listens on port 53 for both UDP and TCP connections, handling DNS queries from clients.


RedirectingResolver Class

Purpose: A custom DNS resolver that intercepts DNS queries and redirects them based on predefined rules.

Initialization Parameters:

redirect_ip_if_not_in_nft: The IPv6 address to redirect unauthenticated users.

redirect_ip_if_in_nft: The IPv6 address to redirect authenticated users for specific domains.

external_domains_file: A file containing additional domains to redirect or block.

upstream_dns: The upstream DNS server (default is Google's 8.8.4.4).



Key Methods

resolve: Processes DNS queries and determines whether to redirect or forward them.

forward_to_upstream: Forwards DNS queries to the upstream DNS server when no redirection is needed.


Custom Proxy Servers

The proxy servers further enhance the captive portal by intercepting HTTP requests and providing appropriate responses.

Proxy Server for Unauthenticated Users

Purpose: Redirects HTTP requests from unauthenticated users to the captive portal login page.

Implementation:

Uses Python's http.server module to create a simple HTTP server.

Listens on a specific IPv6 address and port.

On receiving a GET request, responds with an HTTP 302 redirect to the login page.


Features:

Minimalistic design focused on redirection.

Handles requests efficiently with low overhead.



Proxy Server for Network Connectivity Checks

Purpose: Provides appropriate responses to network connectivity checks performed by various operating systems and devices.

Implementation:

Uses Python's http.server module to create a custom HTTP server.

Handles specific HTTP requests by matching the hostname and path.


Features:

Responds with expected content for connectivity check URLs (e.g., connectivitycheck.gstatic.com, captive.apple.com).

Ensures that devices recognize the network as having a captive portal, prompting the user to authenticate.

Provides informative error pages for blocked requests, including server time and client IP.



Key Methods

do_GET: Overrides the default GET request handler to implement custom redirection logic.

Checks the requested hostname and path.

Provides appropriate HTTP responses based on predefined rules.


Helper Functions:

get_network_from_db: Retrieves the network name from the database for dynamic handling of network-specific requests.



Background Services and Monitoring

Periodic Checks: Background threads monitor active sessions, IP addresses, and data usage.

Data Enforcement: Users exceeding data caps or session duration limits are automatically disconnected.


Error Handling and Logging

Robust Exception Handling: Catches exceptions in DNS resolution and system command executions.

Logging: Logs critical events and errors for troubleshooting and auditing purposes.


Features and Functionality

Enhanced Captive Portal with DNS and Proxy Redirection

User Experience: Improves the captive portal by intercepting DNS and HTTP requests, ensuring users are prompted to authenticate.

Seamless Integration: The DNS and proxy servers work in conjunction with firewall rules to control network access effectively.


Secure Authentication and Authorization

Password Security: Implements secure password storage and verification.

Session Binding: Associates sessions with specific IP and MAC addresses to prevent session hijacking.


Dynamic Network Management

Automatic IP Assignment: Dynamically assigns IPv6 addresses to users upon authentication.

Device Recognition: Identifies devices based on MAC addresses, allowing for device-specific policies.


Bandwidth and Usage Enforcement

Per-User Limits: Sets individual bandwidth limits and data caps.

Session Management: Enforces session duration limits and disconnects users who exceed their allotted time.


Real-Time Monitoring and Reporting

Usage Statistics: Provides users with real-time data on their usage and remaining quotas.

Administrative Oversight: Enables administrators to monitor network usage and manage user policies.


Security Measures

DNS and HTTP Request Control: Blocks or redirects DNS and HTTP requests to prevent access to unauthorized domains or content.

Anti-Spoofing Measures: Monitors network activity to detect and prevent IP or MAC address spoofing.


Results and Testing

IPv6Spot has been tested extensively to validate its functionality and performance.

Functional Testing: Confirmed that the DNS and proxy servers correctly intercept and redirect queries and requests based on user authentication status.

Performance Testing: Ensured that the servers handle queries and requests efficiently without introducing significant latency.

Security Testing: Tested against various attack vectors, including DNS spoofing and unauthorized access attempts.

Compatibility Testing: Verified that the system operates correctly with different client devices and operating systems supporting IPv6.


The integration of the custom DNS and proxy servers significantly improved the user experience by ensuring that unauthenticated users are seamlessly redirected to the captive portal login page.

Conclusion

IPv6Spot is an innovative solution that brings comprehensive captive portal functionality to IPv6 networks. By integrating custom DNS and proxy servers, the system enhances network access control and user experience. The ability to intercept and manipulate DNS queries and HTTP requests allows IPv6Spot to enforce network policies effectively and guide users through the authentication process smoothly.

The project's detailed implementation demonstrates how Python and Linux networking tools can be combined to create a robust network management system. IPv6Spot addresses a critical need in modern networking, providing a foundation for further development and adaptation as IPv6 adoption continues to grow.

Future Work

Potential enhancements to IPv6Spot include:

Advanced DNS Features: Implement DNSSEC validation and support for more complex DNS query types.

Enhanced Proxy Capabilities: Incorporate HTTPS interception and support for additional protocols.

Integration with Security Platforms: Incorporate intrusion detection and prevention systems.

Scalability Improvements: Optimize the system for larger networks with higher user volumes.

User Interface Enhancements: Develop a more intuitive web interface with responsive design.

Multi-Language Support: Localize the user interface to support multiple languages.


Acknowledgments

This project was developed by Abdulkader Alrezej, whose expertise and dedication have led to the creation of IPv6Spot. Special thanks to the developers and maintainers of open-source projects such as Flask, dnslib, SQLite, and Linux networking utilities, which were instrumental in developing this solution.

References

Hagen, Silvia. IPv6 Essentials. O'Reilly Media.

Flask Documentation. Flask.

dnslib Documentation. dnslib.

SQLite Documentation. SQLite.

The Linux Foundation. Linux Advanced Routing & Traffic Control HOWTO. Linux Foundation.

Netfilter Project. nftables. Netfilter.

Python Documentation. subprocess Module. Python Docs.

Python Documentation. http.server Module. Python Docs.



---

Appendix: Detailed Code Analysis

To provide a deeper understanding of IPv6Spot, we include an analysis of the key components and functions in the source code related to the custom proxy servers.

Custom Proxy Servers

The proxy servers are designed to intercept HTTP requests and provide appropriate responses to enhance the captive portal functionality.

Proxy Server for Unauthenticated Users

Purpose: Redirects all HTTP GET requests to the captive portal login page for unauthenticated users.

Implementation:

Class Definition: Inherits from http.server.SimpleHTTPRequestHandler.

do_GET Method:

Overrides the default GET request handler.

Sends an HTTP 302 redirect response to the user's browser, pointing to the captive portal login page.


Server Setup:

Uses socketserver.ThreadingTCPServer with socket.AF_INET6 to handle IPv6 connections.

Listens on a specific IPv6 address and port dedicated to unauthenticated user redirection.



Features:

Stateless Handling: Does not maintain any state between requests, ensuring scalability.

Logging: Overrides log_message to suppress unnecessary logging.



Proxy Server for Network Connectivity Checks

Purpose: Provides appropriate HTTP responses to network connectivity checks performed by various operating systems and devices, ensuring they detect the presence of a captive portal.

Implementation:

Class Definition: Inherits from http.server.SimpleHTTPRequestHandler.

do_GET Method:

Parses the requested URL and hostname.

Normalizes the hostname by removing any www. prefix.

Checks the hostname and path against a set of predefined rules corresponding to known network connectivity check URLs.

Provides the expected HTTP responses for each case, such as returning specific text or HTML content.

If the request does not match any known patterns, responds with an HTTP 403 Forbidden status and displays a custom error page.


Helper Functions:

get_network_from_db: Connects to the SQLite database to retrieve the network name, allowing dynamic handling of network-specific requests.



Features:

OS Compatibility: Supports connectivity checks for various operating systems, including Windows (msftconnecttest.com), Apple (captive.apple.com), GNOME (nmcheck.gnome.org), KDE (networkcheck.kde.org), and Android (connectivitycheck.gstatic.com).

Custom Error Pages: Provides informative error pages for blocked requests, including details like server time and client IP.

Dynamic Responses: Adjusts responses based on data retrieved from the database, allowing for flexible network configurations.



Server Setup

Server Initialization:

Uses socketserver.ThreadingTCPServer with socket.AF_INET6 to handle IPv6 connections.

Binds to a specific IPv6 address and port dedicated to handling network connectivity checks and other HTTP requests.


Concurrency:

Employs a threaded server model to handle multiple simultaneous connections efficiently.




---

Conclusion

The addition of custom proxy servers to IPv6Spot significantly enhances its ability to manage network access and improve user experience. By intercepting HTTP requests and providing tailored responses, the system ensures that devices recognize the presence of a captive portal and prompt users to authenticate. This seamless integration of DNS and proxy services with the captive portal functionality sets IPv6Spot apart as a comprehensive solution for IPv6 network management.

Abdulkader Alrezej's work on IPv6Spot demonstrates a deep understanding of networking principles and the challenges associated with IPv6 adoption. The project's open-source nature encourages collaboration and further development, contributing valuable resources to the networking community.



- Main File Directory: /mnt/cerr Copy all files and folders to the directory: /mnt/cerr.

- File tree directory:

		db  >> Database encryption and backup files
		web_dist >> Web Dashboard Pages Files
		console >> Create virtual interfaces for IPv6Spot
		daa >> Main system database Sqlite3
		external_domains >> The addresses of the sites you want to block are stored using the IPv6Spot system.
		sn >> DNS server
		tba >> nftables rules file
		tv >> IPv6Spot system
		wb >> Back-End of the web control panel
		xt >> IPv6Spot Web Proxy  A Captive portal detection URLs 
		xtt >> IPv6Spot Web Proxy  B Respond to detection URLs 
		OpenWRT version used in the system:
		OpenWRT 22.03.0
		Kernel Version	5.10.138

- Operating requirements:

		python --version > Python 3.10.13
		scapy version  2.5.0

	 -

		NAT64 for a IPv6-only network (Jool) >> https://openwrt.org/docs/guide-user/network/ipv6/nat64
  		conntrack
		python3-lzma	3.10.13-2
		python3-multiprocessing	3.10.13-2
		python3-ncurses	3.10.13-2
		python3-openssl	3.10.13-2
		python3-pillow	9.5.0-2
		python3-pip	23.0.1-1
		python3-pkg-resources
		python3-ply	3.11-2
		python3-psutil	5.9.0-2
		python3-pycparser	2.21-2
		python3-pydoc	3.10.13-2
		python3-readline	3.10.13-2
		python3-setuptools	65.5.0-1
		python3-sqlite3	3.10.13-2
		python3-unittest	3.10.13-2
		python3-urllib	3.10.13-2
		python3-uuid	3.10.13-2
		python3-xml python3-xml	3.10.13-2
		python3	3.10.13-2
		python3-asyncio	3.10.13-2
		python3-base	3.10.13-2
		python3-bidict	0.21.2-2
		python3-cffi	1.16.0-1
		python3-cgi	3.10.13-2
		python3-cgitb	3.10.13-2
		python3-codecs	3.10.13-2
		python3-ctypes	3.10.13-2
		python3-dbm	3.10.13-2
		python3-decimal	3.10.13-2
		python3-dev	3.10.13-2
		python3-distutils	3.10.13-2
		python3-email	3.10.13-2
		python3-lib2to3	3.10.13-2
		python3-light	3.10.13-2
		python3-logging	3.10.13-2
		tc-full
		tc-mod-iptables
		sqlite3-cli	3410200-1
		libsqlite3-0	3410200-1
		ip6tables
		nftables
		xtables-nft
		kmod-ip6tables
		ip-full
		iputils-arping
		nmap
		kmod-veth

- pip list
  Package                     Version
------------------------- --------
  	aiodns                    3.2.0
  	aiohttp                   3.9.5
  	aiosignal                 1.3.1
  	altgraph                  0.17.4
  	annotated-types           0.7.0
  	anyio                     4.4.0
  	APScheduler               3.10.4
  	arabic-reshaper           3.0.0
  	async-timeout             4.0.3
  	asyncio                   3.4.3
  	attrs                     23.2.0
  	bcrypt                    4.2.0
  	bidict                    0.21.2
  	blinker                   1.8.2
  	cachelib                  0.13.0
  	cachetools                5.5.0
  	certifi                   2024.7.4
  	cffi                      1.16.0
  	chardet                   5.2.0
  	charset-normalizer        3.3.2
  	click                     8.1.7
  	dnslib                    0.9.25
  	dnspython                 2.6.1
  	exceptiongroup            1.2.2
  	Flask                     3.0.3
  	Flask-KVSession           0.6.2
  	Flask-Session             0.8.0
  	Flask-SQLAlchemy          3.1.1
  	frozenlist                1.4.1
  	greenlet                  3.0.3
  	h11                       0.14.0
  	httpcore                  1.0.5
  	httpx                     0.27.0
  	humanize                  4.10.0
  	idna                      3.7
  	itsdangerous              2.2.0
  	Jinja2                    3.1.4
  	MarkupSafe                2.1.5
  	msgspec                   0.18.6
  	multidict                 6.0.5
  	packaging                 24.1
  	Pillow                    9.5.0
  	pip                       24.2
  	ply                       3.11
  	proxy.py                  2.4.4
  	psutil                    5.9.0
  	pyarmor.cli.core          3.2.9
  	pycares                   4.4.0
  	pycparser                 2.21
  	pycryptodome              3.20.0
  	pydantic                  2.8.2
  	pydantic_core             2.20.1
  	pyinstaller               6.10.0
  	pyinstaller-hooks-contrib 2024.8
  	pyminifier3               2.3.3
  	pyroute2                  0.7.12
  	pytz                      2024.1
  	reportlab                 4.2.2
  	requests                  2.32.3
  	scapy                     2.5.0
  	setuptools                73.0.1
  	simplekv                  0.14.1
  	six                       1.16.0
  	sniffio                   1.3.1
  	SQLAlchemy                2.0.31
  	typing_extensions         4.12.2
  	tzlocal                   5.2
  	urllib3                   2.2.2
  	Werkzeug                  3.0.3
  	wheel                     0.44.0
  	yarl                      1.9.4

- must be disabled Dnsmasq, Also give root permission to the following files: sn tv wb xt xtt >> chmod +x *.py

			chmod +x sn.py tv.py wb.py xt.py xtt.py

- Also nftable should be directed to the new nftable rules site at: /etc/init.d/nftables And replace this line " nft -f /etc/nftables.conf " with this line nft -f /mnt/cerr/tba

			nft -f /mnt/cerr/tba
- 
- Run services when OpenWrt boots:

			procd_open_instance
			procd_set_param command /mnt/cerr/xt.py
			procd_set_param respawn
			procd_close_instance

			procd_open_instance
			procd_set_param command /mnt/cerr/xtt.py
			procd_set_param respawn
			procd_close_instance

			procd_open_instance
			procd_set_param command /mnt/cerr/tv.py
			procd_set_param respawn
			procd_close_instance

			procd_open_instance
			procd_set_param command /mnt/cerr/sn.py
			procd_set_param respawn
			procd_close_instance

			procd_open_instance
			procd_set_param command /mnt/cerr/wb.py
			procd_set_param respawn
			procd_close_instance

**- Please let me know if there is any error during installation or operation to fix it in the previous guide.**




**- Screenshots of IPv6Spot v1.0.**


![ipv6spot1](https://github.com/user-attachments/assets/365bd93b-3215-4dc4-a524-3aee471cd5b3)


![ipv6spot2](https://github.com/user-attachments/assets/e94f0e85-4415-45cd-8cf5-084551bf5af5)


![7](https://github.com/user-attachments/assets/79c265fb-62a2-4200-b3f1-14415ce53b56)


![2](https://github.com/user-attachments/assets/34a387aa-9e23-4189-8301-a6757ee1d4db)


![12](https://github.com/user-attachments/assets/45ebba16-48eb-4563-9fb5-2a3c95784e97)


![image](https://github.com/user-attachments/assets/ad4ddee7-d3ac-4890-ba4d-de327a4b538e)


![image](https://github.com/user-attachments/assets/a8b6f84c-f7e4-4fa8-8498-bd6e46483806)


![image](https://github.com/user-attachments/assets/e84f0b67-721d-407e-a781-6e7d22c95ee8)


![image](https://github.com/user-attachments/assets/73825639-20d3-4587-b2ce-6bb330119bfc)


![image](https://github.com/user-attachments/assets/b0b48ab3-11c0-4a24-a6ed-9638745efd8c)


![image](https://github.com/user-attachments/assets/52664fb1-8e9f-47df-8c87-8b14ac064068)


![image](https://github.com/user-attachments/assets/146386af-59f0-43a7-9c1f-849a783d50b5)


![image](https://github.com/user-attachments/assets/d689a092-0a67-4dac-bc2f-f1f85d9fc850)


![image](https://github.com/user-attachments/assets/ef1a700b-24eb-4415-ae30-8b7876b17452)


![image](https://github.com/user-attachments/assets/17d956a3-89dd-4a9f-9c45-e415b6d753ba)


![image](https://github.com/user-attachments/assets/9a9fc9aa-91e7-478b-921d-72fbc8863e73)


![image](https://github.com/user-attachments/assets/62dfb250-58d6-400b-b0d6-bf6c79758aea)


![image](https://github.com/user-attachments/assets/097ce47c-0a64-4d2e-9b2a-d90cb052daa6)


![dns](https://github.com/user-attachments/assets/6ab21f94-648b-499e-99c2-361ee43c32a2)


![w3](https://github.com/user-attachments/assets/caee1d69-a01e-40fd-86db-cc7bc13523af)
