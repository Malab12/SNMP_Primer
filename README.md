# NET_SNMP setup and install instructions
Net-Snmp is an opensource suite of softwares and APIs which allow developers to work with and on the SNMP protocol. 

**For more Information on SNMP here are certain reference links:**

1.  https://www.site24x7.com/network/what-is-snmp.html
2.  http://web.deu.edu.tr/doc/oreily/networking/tcpip/ch11_09.htm
3.  https://www.webnms.com/cagent/help/technology_used/c_snmp_overview.html
4.  https://www.fortra.com/resources/articles/snmp-basics-what-it-and-how-it-works#:~:text=SNMP%20is%20used%20to%20collect,their%20managed%20devices%20and%20applications.
5.  O Reily Publication: Essential SNMP (2005 Edition)

Finally for most net-snmp related references refer to:
1. http://www.net-snmp.org/wiki/
2. http://www.net-snmp.org/wiki/index.php/Tutorials
3. http://www.net-snmp.org/tutorial/tutorial-5/

---
# Installing net-snmp on Debian systems
For the most part we will be following the official guide for installing net-snmp on Ubuntu: http://www.net-snmp.org/wiki/index.php/Net-Snmp_on_Ubuntu for the most part. Depending on your personal system and configurations, certain things might be different refer further for potential solutions.

**References for installation instructions:**
1. http://www.net-snmp.org/wiki/index.php/Net-Snmp_on_Ubuntu
2. http://www.net-snmp.org/docs/INSTALL.html

### Basic Setup Instructions
***This assumes that you have a regular install of your OS which includes gcc and other build tools, if not install them before starting***

1. Before even starting the configuration, install ***libperl-dev***: `sudo apt-get install libperl-dev` which is required for the perl libraries which come with the base config now
2. unzip the folder and from within the extracted directory run `./configure` and follow the instructions of the setup program
3. Once finished run `make`. Depending on your system this can take couple of minutes to execute.
4. Assuming the previous commands ran without errors run `make test` just to ensure the sanctity of the configuration
5. Finally, run `sudo make install` to install the files onto your system.
6. run `snmpget --version` to ensure that net-snmp has been installed

### Potential Issues duirng configuration
1. If `make` fails, check if you have all the dependencies installed.
2. If `make install` fails, try running the command as `sudo make install`
---
# Getting a simple manager and agent system running on local host
To set up a basic manager and agent system, we will be following the set of articles published on digital ocean: https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-an-snmp-daemon-and-client-on-ubuntu-18-04. 

The article specifically goes through the process of creating an snmp connection over remote hosts (However it utilizes the sudo packages snmp and smpd instead of the net-snmp library. For our puposes since net-snmp comes pre-packaged with the command line tools utilized in the article we do not need to install the dependencies seperately), we will repurpose it to work on local host first

## Steps to get a snmp connection over local host
1. try running the command `sudo snmpd -d -f -Le`
2. Since we have not specified an `snmpd.conf` file the command should prompt you to make a new one and provide you with the command to do so
3. utilize `sudo snmpconf -i` to create a new `snmpd.conf` file and place it in the global directory from where the agent process can read it 
4. Refer to the digital ocean article to understand what all should be there in the conf files and make updates accordingly
5. Instead of configuring snmpd on a seperate host machine do the instructions associated with it on the same machine as well
6. Run `sudo snmpd -d -f -Le` on an independent terminal instance
7. On a seperate terminal run the manager commands. Where-ever in the commands you observe terms like `manager-ip-address` or `agent-ip-address` replace them with `localhost` or `127.0.0.1`

## Certain potential issues to be vary of
1. There is a possibility that your snmp configuration does not include any MIB files (you do not require them to run the agent and manager in the above case but for future use and purposes). In such a case `mibs-downloader` provides you with most of the mib files required the rest you can download from the internet and place in one of mibs directories such as `/usr/local/share/snmp/mibs`
2. In case you get a timeout error, run the command again but with the `-d` flag (debug) whcih provides you with all the information about the packets leaving and being recieved by the manager
3. To run the same on the agent process, first shut down the running `snmpd` process and then run `sudo snmpd -f -Le -d`.
4. Taking note of the packages leaving and being recieved by both the manager and agent. This should give you an idea on which side the issue is on. Utilize the faq pages: http://www.net-snmp.org/FAQ.html and the internet for further help and clarification
---
# SNMP over remote host
For this we will utilize the digital ocean example again. https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-an-snmp-daemon-and-client-on-ubuntu-18-04.

This time however, we will use the actual IP addresses instead of local host. We suggest running both the manager functions as well as the agent process with `-d` flag so you can track the packets.

## Certain potential issues to be vary about
1. As of the writing of this document, The official systems running on top of the office wifi can connect to other office devices using **LOCAL IP addresses**. 
2. Make sure to change the `agentaddress` in `snmpd.conf` for it allow communication aside from the local host
3. Depending on your firewall settings and system settings in general you might have to enable certain permissions before remote communications can take place. Look up on the internet on how to do the same for your specific system. As of the writing of this document, official laptops do not require any additional setup for local network communication.
---
# Writing your first piece of code: A simple synchronous management application
For this we heavily follow the tutorial provided on the official net-snmp wiki: http://www.net-snmp.org/wiki/index.php/TUT:Simple_Application

## Certain potential issues to be vary about
1. The endpoint provided in the original tutorial `test.net-snmp.org` is non functional or atleast unreachable from official laptops. Change the endpoint to either the agent running on your localhost or to a remote agent
2. You will also have to change the user id and password used in the code to one of the snmp users registered to the agent you are calling to (I perticularly suggest to utilize snmp -v2c community strings instead as they are easier to set up and write code for)
---
# Further on from here
1. Look into the agent tutorials, they are fairly more complex than the manager ones
2. Have a look at the manual pages and the source code for further detail and clarification http://www.net-snmp.org/docs/man/
3. Start with the mibs module to begin developing your own implementation of agents and subagents
-----
# Wait what are MIBS exactly?
Here are some resources for understanding the basics of MIBS and how to write one
1. **SNMP Mastery Chapter 3 and 4** by Michael Lucas for understanding the basics of MIBS and how to add them
2. http://www.net-snmp.org/wiki/index.php/Category:GoodAnswer is a very good repository of questions asked by the community and high quality answers provided for the same. Of perticular interest in our case is:
3. **Writing your own MIBs:** http://www.net-snmp.org/wiki/index.php/Writing_your_own_MIBs
4. **Loading your own MIBs:** http://www.net-snmp.org/wiki/index.php/Loading_your_own_MIBs
----
# Writing our own Mibs
1. Utilizing the articles from above we can create our own mibs. Just for testing purposes we recommend creating a MIB with a singular scaler int value first and integrating it
2. Once you have written your mib it needs to be validated. This can be done through several online mib validators as well as command line tools, take your pick and ensure your mib does not have any errors
3. Refer to **Writing your own MIBS** so that net-snmp can find it and refer to it
----
# Integrating Mibs to the agent as module
You have created a MIB but that does not mean that the agent can access it or handle any requests by the manager. For that we have to write code which tells the agent how to handle requests to our custom MIB. Luckily we do not have to write anything from scratch. Utilizing `mib2c` to create a template to which we can make additions according to our needs.

**References**
1. **mib2c basics**  http://www.net-snmp.org/tutorial/tutorial-5/toolkit/mib2c/index.html
2. For the MIB we created having a single scalar integer, We can basically refer to `scaler_int.c` for code reference. http://www.net-snmp.org/dev/agent/scalar__int_8c-example.html
3. In our installation directory there is a directory `agent\mibgroups\examples` which includes files for integrating different forms of data in a MIB and can be utilized as main reference
4. Once you have written the necessary files, refer to http://www.net-snmp.org/tutorial/tutorial-5/toolkit/mib_module/index.html to figure out how to integrate our newely created mibs with the agent

# Looking Ahead
Creating our own version of of the agent
Basic agent architecture and flow of the agent startup are here: [agent_creation_documentation](AGENT_README.md)
   

